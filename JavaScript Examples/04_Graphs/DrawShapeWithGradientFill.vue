<template>
  <span>The following example demonstrates how to draw shape with gradient fill.</span>
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
        // Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        // Create a new page
        let page = doc.Pages.Add();

        // Create a PdfLinearGradientBrush and set its style
        let point1 = wasmModule.PointF.Create({ x: 100, y: 100 });
        let size1 = wasmModule.SizeF.Create({ width: 200, height: 120 });
        let rect1 = wasmModule.RectangleF.Create({ location: point1, size: size1 });
        let pdfRgbColor1 = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_BlueViolet() });
        let pdfRgbColor2 = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_DarkBlue() });

        // Create the linear gradient brush with horizontal mode
        let brush1 = wasmModule.PdfLinearGradientBrush.Create({
          rect: rect1,
          color1: pdfRgbColor1,
          color2: pdfRgbColor2,
          mode: wasmModule.PdfLinearGradientMode.Horizontal
        });

        // Draw a rectangle using the linear gradient brush
        page.Canvas.DrawRectangle({ brush: brush1, rectangle: rect1 });

        // Create a PdfRadialGradientBrush and set its style
        let point2 = wasmModule.PointF.Create({ x: 200, y: 400 });
        let point3 = wasmModule.PointF.Create({ x: 300, y: 500 });
        let pdfRgbColor3 = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_SkyBlue() });
        let pdfRgbColor4 = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_DarkBlue() });

        // Create the radial gradient brush
        let brush2 = wasmModule.PdfRadialGradientBrush.Create({ centreStart: point2, radiusStart: 100, centreEnd: point3, radiusEnd: 100, colorStart: pdfRgbColor3, colorEnd: pdfRgbColor4 });

        // Draw an ellipse using the radial gradient brush
        let point5 = wasmModule.PointF.Create({ x: 100, y: 300 });
        let size5 = wasmModule.SizeF.Create({ width: 200, height: 200 });
        let rect5 = wasmModule.RectangleF.Create({ location: point5, size: size5 });
        page.Canvas.DrawEllipse({ brush: brush2, rectangle: rect5 });

        // Define the output file name
        const outputFileName = "DrawShapeWithGradientFill_result.pdf";

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
