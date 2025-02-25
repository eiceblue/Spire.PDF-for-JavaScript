<template>
  <span>The following example demonstrates how to draw rectangles in PDF document.</span>
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
        let x = 130;
        let y = 100;
        let width = 300;
        let height = 400;

        // Draw rectangles
        let pdfRgbColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Black() });
        let pen = wasmModule.PdfPen.Create({ pdfRgbColor: pdfRgbColor, width: 0.1 });

        let point = wasmModule.PointF.Create({ x: x, y: y });
        let size = wasmModule.SizeF.Create({ width: width, height: height });
        let rect = wasmModule.RectangleF.Create({ location: point, size: size });
        page.Canvas.DrawRectangle({ pen: pen, rectangle: rect });

        y = y + height - 50;
        width = 100;
        height = 50;

        // Initialize an instance of PdfSeparationColorSpace
        let pdfRgbColor1 = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.FromArgb({ alpha: 0, red: 100, green: 0, blue: 0 }) });
        let cs = wasmModule.PdfSeparationColorSpace.Create("MySpotColor", pdfRgbColor1);

        let pdfRgbColor2 = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Red() });
        let pen2 = wasmModule.PdfPen.Create({ pdfRgbColor: pdfRgbColor2, width: 1 });

        // Create a brush with spot color
        let separationColor = wasmModule.PdfSeparationColor.Create(cs, 0.1);
        let brush = wasmModule.PdfSolidBrush.Create({ complexColor: separationColor });

        // Draw rectangles
        let point1 = wasmModule.PointF.Create({ x: x, y: y });
        let size1 = wasmModule.SizeF.Create({ width: width, height: height });
        let rect1 = wasmModule.RectangleF.Create({ location: point1, size: size1 });
        page.Canvas.DrawRectangle({ pen: pen2, brush: brush, rectangle: rect1 });

        // Restore graphics
        page.Canvas.Restore({ state: state });

        // Define the output file name
        const outputFileName = "DrawRectangles_result.pdf";

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
