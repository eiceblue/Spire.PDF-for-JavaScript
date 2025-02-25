<template>
  <span>The following example demonstrates how to draw line in PDF document.</span>
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
        // Load the sample file into the virtual file system (VFS)
        let inputFileName = "DrawingTemplate.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        // Load the PDF fle
        doc.LoadFromFile({ fileName: inputFileName });

        // Get the first page
        let page = doc.Pages.get_Item(0);

        // Save graphics state
        let state = page.Canvas.Save();

        // Set location and size
        let x = 95;
        let y = 95;
        let width = 400;
        let height = 500;

        // Create pens
        let pdfRgbColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Black() });
        let pen = wasmModule.PdfPen.Create({ pdfRgbColor: pdfRgbColor, width: 0.1 });

        let pdfRgbColor1 = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Red() });
        let pen1 = wasmModule.PdfPen.Create({ pdfRgbColor: pdfRgbColor1, width: 0.1 });

        // Draw a rectangle  pen, x, y, width, height
        page.Canvas.DrawRectangle({ pen: pen, x: x, y: y, width: width, height: height });

        // Draw two crossed lines
        page.Canvas.DrawLine({ pen: pen1, x1: x, y1: y, x2: x + width, y2: y + height });
        page.Canvas.DrawLine({ pen: pen1, x1: x + width, y1: y, x2: x, y2: y + height });

        // Restore graphics
        page.Canvas.Restore({ state });

        // Define the output file name
        const outputFileName = "DrawLine_result.pdf";

        // Save the document to the specified path
        doc.SaveToFile({ fileName: outputFileName });

        // Clean up resources
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
