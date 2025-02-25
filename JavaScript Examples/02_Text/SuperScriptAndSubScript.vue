<template>
  <span>The following example demonstrates how to add SuperScript and SubScript in PDF document.</span>
  <el-button @click="startProcessing">Start</el-button>
  <a v-if="downloadUrl" :href="downloadUrl" :download="downloadName">
    Click here to download the generated file
  </a>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    const downloadUrl = ref(null);
    const downloadName = ref("");

    const startProcessing = async () => {
      wasmModule = window.wasmModule;
      if (wasmModule) {
 
        // Create a pdf instance
        let doc = wasmModule.PdfDocument.Create();
         // Create one page
        let page = doc.Pages.Add();
        
        let text = "Spire.PDF for JavaScript";
        let font = wasmModule.PdfFont.Create(wasmModule.PdfFontFamily.Helvetica, 20);
        let brush = wasmModule.PdfSolidBrush.Create({color:wasmModule.Color.get_Black()});

        //Draw Superscript
        DrawSuperscript(page, text, font, brush);

        //Draw Subscript
        DrawSubscript(page, text, font, brush);

        // Define the output file name
        const outputFileName = "SuperScriptAndSubScript_out.pdf";

        // Save the document to the specified path
        doc.SaveToFile({fileName: outputFileName});
        doc.Close();

        // Read the saved file and convert to a Blob object
        const modifiedFileArray = wasmModule.FS.readFile(outputFileName);
        const modifiedFile = new Blob([modifiedFileArray], { type: "application/pdf" });

        // Download the file
        downloadName.value = outputFileName;
        downloadUrl.value = URL.createObjectURL(modifiedFile);
      }
function DrawSuperscript(page, text, font, brush) {
    let x = 120;
    let y = 100;
    page.Canvas.DrawString({s: text, font: font, brush: brush, point: wasmModule.PointF.Create({x: x, y: y})});

    //Measure the string
    let size = font.MeasureString({text: text});

    //Set the x coordinate of the superscript text
    x += size.Width;

    //Instantiate a PdfStringFormat instance
    let format = wasmModule.PdfStringFormat.Create();

    //Set format as superscript
    format.SubSuperScript = wasmModule.PdfSubSuperScript.SuperScript;

    //Draw superscript text with format
    text = "Superscript";
    let point = wasmModule.PointF.Create({x: x, y: y});
    //s, font, pen, point, format
    page.Canvas.DrawString({s: text, font: font, brush: brush, point: point, format: format});
}

function DrawSubscript(page, text, font, brush) {
    let x = 120;
    let y = 150;
    let point = wasmModule.PointF.Create({x: x, y: y});
    // s, font, brush, point
    page.Canvas.DrawString({s: text, font: font, brush: brush, point: point});

    //Measure the string
    let size = font.MeasureString({text: text});

    //Set the x coordinate of the superscript text
    x += size.Width;

    //Instantiate a PdfStringFormat instance
    let format = wasmModule.PdfStringFormat.Create();

    //Set format as superscript
    format.SubSuperScript = wasmModule.PdfSubSuperScript.SubScript;

    //Draw superscript text with format
    text = "SubScript";
    let point1 = wasmModule.PointF.Create({x: x, y: y});
    page.Canvas.DrawString({s: text, font: font, brush: brush, point: point1, format: format});
}
    };

    return {
      startProcessing,
      downloadName,
      downloadUrl,
    };
  },
};
</script>
