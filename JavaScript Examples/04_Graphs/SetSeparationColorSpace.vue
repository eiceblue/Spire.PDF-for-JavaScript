<template>
  <span>The following example demonstrates how to set separation color space.</span>
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

        // Initialize an instance of PdfSeparationColorSpace with RGB color
        let pdfRGBColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Purple() });
        let rgb = wasmModule.PdfSeparationColorSpace.Create("MySpotColor", pdfRGBColor);

        // Set tint = 1 for the color space
        let color = wasmModule.PdfSeparationColor.Create(rgb, 1);

        // Create a brush with spot color
        let brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });

        let font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 10 });

        // Draw pie with tint=1.0  brush, x, y, width, height, startAngle, sweepAngle
        page.Canvas.DrawPie({ brush: brush, x: 10, y: 30, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });
        page.Canvas.DrawString({ s: "Tint=1.0", font: font, brush: brush, point: wasmModule.PointF.Create({ x: 22, y: 100 }) });

        // Change tint to 0.5
        color = wasmModule.PdfSeparationColor.Create(rgb, 0.5);
        brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });

        // Draw pie with tint=0.5
        page.Canvas.DrawPie({ brush: brush, x: 80, y: 30, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });
        page.Canvas.DrawString({ s: "Tint=0.5", font: font, brush: brush, point: wasmModule.PointF.Create({ x: 92, y: 100 }) });

        // Change tint to 0.25
        color = wasmModule.PdfSeparationColor.Create(rgb, 0.25);
        brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });

        // Draw pie with tint=0.25
        page.Canvas.DrawPie({ brush: brush, x: 150, y: 30, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });
        page.Canvas.DrawString({ s: "Tint=0.25", font: font, brush: brush, point: wasmModule.PointF.Create({ x: 162, y: 100 }) });

        // Define the output file name
        const outputFileName = "SetSeparationColorSpace_result.pdf";

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
