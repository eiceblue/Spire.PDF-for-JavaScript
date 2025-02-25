<template>
  <span>The following example demonstrates how to write a "HelloWorld" to a PDF document.</span>
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
        let pagebase = doc.Pages.Add();
        
        const text = "Hello World";
        let pdffont = wasmModule.PdfFont.Create(wasmModule.PdfFontFamily.Helvetica, 30);
        let pdfBrush = wasmModule.PdfSolidBrush.Create({color:wasmModule.Color.get_Blue()});
        // Draw the text
        pagebase.Canvas.DrawString({s: text, font: pdffont, brush: pdfBrush, x: 10, y: 10});

        // Define the output file name
        const outputFileName = "HelloWorld_out.pdf";

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
    };

    return {
      startProcessing,
      downloadName,
      downloadUrl,
    };
  },
};
</script>
