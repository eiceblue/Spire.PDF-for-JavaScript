<template>
  <span>The following example demonstrates how to draw a filled rectangle in PDF document.</span>
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

        // Set rectangle display location and size
        let x = 200;
        let y = 300;
        let width = 200;
        let height = 120;

        // Create a pen and a brush
        let pdfRgbColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Black() });
        let pen = wasmModule.PdfPen.Create({ pdfRgbColor: pdfRgbColor, width: 1 });
        let brush = wasmModule.PdfSolidBrush.Create({ color: wasmModule.Color.get_OrangeRed() });

        // Draw a filled rectangle
        let point = wasmModule.PointF.Create({ x: x, y: y });
        let size = wasmModule.SizeF.Create({ width: width, height: height });
        let rect = wasmModule.RectangleF.Create({ location: point, size: size });
        page.Canvas.DrawRectangle({ pen: pen, brush: brush, rectangle: rect });

        // Restore graphics
        page.Canvas.Restore({ state: state });

        // Define the output file name
        const outputFileName = "DrawFilledRectangles_result.pdf";

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
