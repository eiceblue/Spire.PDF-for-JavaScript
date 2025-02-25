<template>
  <span>The following example demonstrates how to draw dashed line in PDF document.</span>
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

        // Set location and size for line
        let x = 150;
        let y = 200;
        let width = 300;

        // Create pens and set syle for it
        let pdfRgbColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Red() });
        let pen = wasmModule.PdfPen.Create({ pdfRgbColor: pdfRgbColor, width: 3 });

        // Set dash style and pattern
        pen.DashStyle = wasmModule.PdfDashStyle.Dash;
        pen.DashPattern = [1.0, 4.0, 1.0];

        // Draw dashed lines
        page.Canvas.DrawLine({ pen: pen, x1: x, y1: y, x2: x + width, y2: y });

        // Restore graphics
        page.Canvas.Restore({ state: state });

        // Define the output file name
        const outputFileName = "DrawDashedLine_result.pdf";

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
