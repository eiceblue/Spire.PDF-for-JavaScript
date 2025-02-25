<template>
  <span>The following example demonstrates how to draw content with spot color in PDF document.</span>
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

        // Add a new page
        let page = doc.Pages.Add();

        // Initialize an instance of PdfSeparationColorSpace
        let pdfRgbColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_DarkViolet() });
        let cs = wasmModule.PdfSeparationColorSpace.Create("MySpotColor", pdfRgbColor);

        // Set tini = 1 for the cs
        let color = wasmModule.PdfSeparationColor.Create(cs, 1);

        // Create a brush with spot color
        let brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });
        let font1 = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 10 });

        // Draw a string
        page.Canvas.DrawString({ s: "Tint=1.0", font: font1, brush: brush, point: wasmModule.PointF.Create({ x: 160, y: 160 }) });

        // Draw pie with spot color(DarkViolet)
        page.Canvas.DrawPie({ brush: brush, x: 148, y: 200, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });

        // Create a font
        let font2 = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 10 });
        page.Canvas.DrawString({ s: "Tint=0.7", font: font2, brush: brush, point: wasmModule.PointF.Create({ x: 230, y: 160 }) });
        
        // Create a separation color with tint 0.7 and draw a pie
        color = wasmModule.PdfSeparationColor.Create(cs, 0.7);
        brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });
        page.Canvas.DrawPie({ brush: brush, x: 218, y: 200, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });

        // Create a font
        let font3 = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 10 });
        page.Canvas.DrawString({ s: "Tint=0.4", font: font3, brush: brush, point: wasmModule.PointF.Create({ x: 300, y: 160 }) });
        
        // Create a separation color with tint 0.4 and draw a pie
        color = wasmModule.PdfSeparationColor.Create(cs, 0.4);
        brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });
        page.Canvas.DrawPie({ brush: brush, x: 288, y: 200, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });

        // Create a font
        let font4 = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 10 });
        page.Canvas.DrawString({ s: "Tint=0.1", font: font4, brush: brush, point: wasmModule.PointF.Create({ x: 370, y: 160 }) });
        
        // Create a separation color with tint 0.1 and draw a pie
        color = wasmModule.PdfSeparationColor.Create(cs, 0.1);
        brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });
        page.Canvas.DrawPie({ brush: brush, x: 358, y: 200, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });

        // Draw pie with spot color(Purple)  colorant, baseColor
        pdfRgbColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Purple() });
        cs = wasmModule.PdfSeparationColorSpace.Create("MySpotColor", pdfRgbColor);
        color = wasmModule.PdfSeparationColor.Create(cs, 1);
        brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });
        page.Canvas.DrawPie({ brush: brush, x: 148, y: 280, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });

        // Draw pies with different tints of the purple spot color
        color = wasmModule.PdfSeparationColor.Create(cs, 0.7);
        brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });
        page.Canvas.DrawPie({ brush: brush, x: 218, y: 280, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });

        color = wasmModule.PdfSeparationColor.Create(cs, 0.4);
        brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });
        page.Canvas.DrawPie({ brush: brush, x: 288, y: 280, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });

        color = wasmModule.PdfSeparationColor.Create(cs, 0.1);
        brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });
        page.Canvas.DrawPie({ brush: brush, x: 358, y: 280, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });

        // Draw pie with spot color (DarkSlateBlue) and base color
        pdfRgbColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_DarkSlateBlue() });
        cs = wasmModule.PdfSeparationColorSpace.Create("MySpotColor", pdfRgbColor);
        color = wasmModule.PdfSeparationColor.Create(cs, 1);
        brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });
        page.Canvas.DrawPie({ brush: brush, x: 148, y: 360, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });

        // Draw pies with different tints of the dark slate blue spot color
        color = wasmModule.PdfSeparationColor.Create(cs, 0.7);
        brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });
        page.Canvas.DrawPie({ brush: brush, x: 218, y: 360, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });

        color = wasmModule.PdfSeparationColor.Create(cs, 0.4);
        brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });
        page.Canvas.DrawPie({ brush: brush, x: 288, y: 360, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });

        color = wasmModule.PdfSeparationColor.Create(cs, 0.1);
        brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });
        page.Canvas.DrawPie({ brush: brush, x: 358, y: 360, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });

        // Define the output file name
        const outputFileName = "HelloWorld_out.pdf";

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
