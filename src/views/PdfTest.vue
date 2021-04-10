<template>
  <div>
    <button @click="generatePdf">
      生成
    </button>
    <div style="visibility: visible">

      <div style="width: 1500px; height: 1000px;">
        <pie-chart ref="pieChart"/>
      </div>
    </div>
  </div>
</template>

<script>
import PieChart from "@/components/PieChart";
import {ref} from "vue";

const pdfFonts = require("@/assets/vfs_fonts");
const pdfMake = require('pdfmake/build/pdfmake.js');
export default {
  name: "PdfTest",
  components: {
    PieChart,
  },
  setup: () => {
    const pieChart = ref();

    return {
      pieChart
    }
  },
  methods: {
    generatePdf() {
      pdfMake.vfs = pdfFonts.pdfMake.vfs;

      pdfMake.fonts = {
        sarasa: {
          normal: "sarasa-ui-sc-regular.ttf"
        }
      };

      const docDefinition = {
        content: [
          {
            text: '居中标题This paragraph uses header style and extends the alignment property',
            style: 'header',
            alignment: 'center'
          },
          {
            type: 'none',
            ul: [
              'item 1',
              'item 2',
              'item 3'
            ]
          },
          {
            style: 'tableExample',
            table: {
              widths: ['auto', 'auto', 'auto', 'auto', 'auto'],
              body: [
                ['th0', 'th1', 'th2', 'th3', 'th4'],
                ['短', 'long long long long long long long long ',
                  'short', 'short', 'long long long long long long long long '],
                ['short', 'long long long long long long long long ',
                  'short', 'short', 'long long long long long long long long '],
                ['short', 'long long long long long long long long ',
                  'short', 'short', 'long long long long long long long long ']
              ]
            }
          },

          {
            image: this.$refs.pieChart.getDataURL(),
            width: 750,
            pageOrientation: 'landscape',
            pageBreak: 'before'
          },
        ],
        styles: {
          header: {
            fontSize: 18,
            alignment: 'justify'
          },
          tableExample: {
            margin: [0, 5, 0, 15]
          },
        },
        defaultStyle: {
          font: 'sarasa'
        }
      };

      const pdf = pdfMake.createPdf(docDefinition);
      // const now = new Date();
      // pdf.download(now.toDateString());
      pdf.open();
    }
  }
}
</script>

<style scoped>

</style>
