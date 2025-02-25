<template>
  <span>The following example demonstrates how to draw rectangles with transparency.</span>
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

        // Get the first page of the document
        let page = doc.Pages.get_Item(0);

        // Save the current graphics state
        let state = page.Canvas.Save();

        // Define rectangle properties
        let x = 200;
        let y = 300;
        let width = 200;
        let height = 100;

        // Create a black pen for outlining the rectangles
        let pdfrgbColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Black() });
        let pen = wasmModule.PdfPen.Create({ pdfRgbColor: pdfrgbColor, width: 1 });
        
        // Create a red brush for filling the rectangles
        let brush = wasmModule.PdfSolidBrush.Create({ color: wasmModule.Color.get_Red() });

        // Set the blend mode for transparency effects
        let mode = wasmModule.PdfBlendMode.Normal;

        // Set transparency for the first rectangle (50% transparency)
        page.Canvas.SetTransparency({ alphaPen: 0.5, alphaBrush: 0.5, blendMode: mode });

        // Define and draw the first rectangle
        let point = wasmModule.PointF.Create({ x: x, y: y });
        let size = wasmModule.SizeF.Create({ width: width, height: height });
        let rect = wasmModule.RectangleF.Create({ location: point, size: size });
        page.Canvas.DrawRectangle({ pen: pen, brush: brush, rectangle: rect });

        // Update position for the second rectangle
        x = x + width / 2;
        y = y - height / 2;

        // Set transparency for the second rectangle (20% transparency)
        page.Canvas.SetTransparency({ alphaPen: 0.2, alphaBrush: 0.2, blendMode: mode });

        // Define and draw the second rectangle
        let point1 = wasmModule.PointF.Create({ x: x, y: y });
        let size1 = wasmModule.SizeF.Create({ width: width, height: height });
        let rect1 = wasmModule.RectangleF.Create({ location: point1, size: size1 });
        page.Canvas.DrawRectangle({ pen: pen, brush: brush, rectangle: rect1 });

        // Restore the graphics state to the saved state
        page.Canvas.Restore({ state: state });

        // Define the output file name
        const outputFileName = "SetRectangleTransparency_result.pdf";

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
