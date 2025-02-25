<template>
  <span>The following example demonstrates how to draw shape in a PDF document.</span>
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

        // Draw a path
        DrawPath(page);

        // Draw a spirograph shape
        DrawSpiro(page);

        // Draw a rectangle 
        DrawRectangle(page);

        // Draw a pie
        DrawPie(page);

        // Draw an ellipse
        DrawEllipse(page);

        // Define the output file name
        const outputFileName = "DrawShape_result.pdf";

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
    function DrawPath(page) {
      // Define arry to store point of line
      let points = new Array(5);

      // Calculate the points for a pentagon shape using trigonometry
      for (let i = 0; i < points.length; i++) {
        let x = Math.cos(i * 2 * Math.PI / 5);
        let y = Math.sin(i * 2 * Math.PI / 5);
        let pointf = wasmModule.PointF.Create({ x: x, y: y });
        points[i] = pointf;
      }

      // Create a path and define the lines connecting the points
      let path = wasmModule.PdfPath.Create();
      path.AddLine({ point1: points[2], point2: points[0] });
      path.AddLine({ point1: points[0], point2: points[3] });
      path.AddLine({ point1: points[3], point2: points[1] });
      path.AddLine({ point1: points[1], point2: points[4] });
      path.AddLine({ point1: points[4], point2: points[2] });

      // Save the current graphics state
      let state = page.Canvas.Save();

      // Define a pen and brush with specific colors and styles
      let pdfrgbColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_DeepSkyBlue() });
      let pen = wasmModule.PdfPen.Create({ pdfRgbColor: pdfrgbColor, width: 0.02 });
      let brush1 = wasmModule.PdfSolidBrush.Create({ color: wasmModule.Color.get_CadetBlue() });

      // Apply scaling and translation to the graphics context
      page.Canvas.ScaleTransform(50, 50);
      page.Canvas.TranslateTransform(5, 1.2);

      // Draw the path with the defined pen
      page.Canvas.DrawPath({ pen: pen, path: path });

      // Translate and draw with different fill modes
      page.Canvas.TranslateTransform(2, 0);
      path.FillMode = wasmModule.PdfFillMode.Alternate;
      page.Canvas.DrawPath({ pen: pen, brush: brush1, path: path });

      page.Canvas.TranslateTransform(2, 0);
      path.FillMode = wasmModule.PdfFillMode.Winding;
      page.Canvas.DrawPath({ pen: pen, brush: brush1, path: path });

      // Create a linear gradient brush for another path
      let point1 = wasmModule.PointF.Create({ x: -2, y: 0 });
      let point2 = wasmModule.PointF.Create({ x: 2, y: 0 });
      let regColor1 = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Red() });
      let regColor2 = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Blue() });
      let brush2 = wasmModule.PdfLinearGradientBrush.Create({
        point1: point1,
        point2: point2,
        color1: regColor1,
        color2: regColor2
      });

      // Translate and draw the path with the gradient brush
      page.Canvas.TranslateTransform(-4, 2);
      path.FillMode = wasmModule.PdfFillMode.Alternate;
      page.Canvas.DrawPath({ pen: pen, brush: brush2, path: path });

      // Create a radial gradient brush for another effect
      let point3 = wasmModule.PointF.Create({ x: 0, y: 0 });
      let point4 = wasmModule.PointF.Create({ x: 0, y: 0 });
      let brush3 = wasmModule.PdfRadialGradientBrush.Create({
        centreStart: point3,
        radiusStart: 0,
        centreEnd: point4,
        radiusEnd: 1,
        colorStart: regColor1,
        colorEnd: regColor2
      });

      // Translate and draw the path with the radial gradient brush
      page.Canvas.TranslateTransform(2, 0);
      path.FillMode = wasmModule.PdfFillMode.Winding;
      page.Canvas.DrawPath({ pen: pen, brush: brush3, path: path });

      // Create a tiling brush for a pattern
      let rect = wasmModule.RectangleF.Create({ x: 0, y: 0, width: 4, height: 4 });
      let brush4 = wasmModule.PdfTilingBrush.Create({ rectangle: rect });
      brush4.Graphics.DrawRectangle({ brush: brush2, x: 0, y: 0, width: 4, height: 4 });

      // Translate and draw the path with the tiling brush
      page.Canvas.TranslateTransform(2, 0);
      path.FillMode = wasmModule.PdfFillMode.Winding;
      page.Canvas.DrawPath({ pen: pen, brush: brush4, path: path });

      // Restore the previous graphics state
      page.Canvas.Restore({ state: state });
    };

    function DrawSpiro(page) {
      // Save the current graphics state
      let state = page.Canvas.Save();

      // Draw a spirograph shape
      let pen = wasmModule.PdfPens.get_DeepSkyBlue();

      // Define parameters for the spirograph
      let nPoints = 1000;
      let r1 = 30;
      let r2 = 25;
      let p = 35;
      let x1 = r1 + r2 - p;
      let y1 = 0;
      let x2 = 0;
      let y2 = 0;

      // Translate to the desired position
      page.Canvas.TranslateTransform(100, 100);

      // Calculate and draw the spirograph points
      for (let i = 0; i < nPoints; i++) {
        let t = i * Math.PI / 90;
        x2 = (r1 + r2) * Math.cos(t) - p * Math.cos((r1 + r2) * t / r2);
        y2 = (r1 + r2) * Math.sin(t) - p * Math.sin((r1 + r2) * t / r2);
        page.Canvas.DrawLine({ pen: pen, x1: x1, y1: y1, x2: x2, y2: y2 });
        x1 = x2;
        y1 = y2;
      }

      // Restore the previous graphics state
      page.Canvas.Restore({ state: state });
    };

    function DrawRectangle(page) {
      // Save the current graphics state
      let state = page.Canvas.Save();

      // Create a pen with a chocolate color and set its width
      let pdfrgbColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Chocolate() });
      let pen = wasmModule.PdfPen.Create({ pdfRgbColor: pdfrgbColor, width: 1 });

      // Define the position and size of the rectangle
      let point = wasmModule.PointF.Create({ x: 20, y: 310 });
      let size = wasmModule.SizeF.Create({ width: 150, height: 120 });
      let rect = wasmModule.RectangleF.Create({ location: point, size: size });

      // Draw the rectangle on the canvas
      page.Canvas.DrawRectangle({ pen: pen, rectangle: rect });

      // Restore the previous graphics state
      page.Canvas.Restore({ state: state });
    };

    function DrawPie(page) {
      // Save the current graphics state
      let state = page.Canvas.Save();

      // Create a pen with a dark red color and set its width
      let pdfrgbColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_DarkRed() });
      let pen = wasmModule.PdfPen.Create({ pdfRgbColor: pdfrgbColor, width: 2 });

      // Draw a pie shape with specified parameters
      // Parameters: pen, x position, y position, width, height, start angle, sweep angle
      page.Canvas.DrawPie({ pen: pen, x: 220, y: 320, width: 100, height: 90, startAngle: 360, sweepAngle: 360 });

      // Restore the previous graphics state
      page.Canvas.Restore({ state: state });
    };

    function DrawEllipse(page) {
      // Save the current graphics state
      let state = page.Canvas.Save();

      // Create a brush with a cadet blue color
      let brush = wasmModule.PdfSolidBrush.Create({ color: wasmModule.Color.get_CadetBlue() });

      // Draw an ellipse with specified parameters
      page.Canvas.DrawEllipse({ brush: brush, x: 380, y: 325, width: 80, height: 80 });

      // Restore the previous graphics state
      page.Canvas.Restore({ state: state });
    };

    return {
      startProcessing,
      downloadName,
      downloadUrl,
    };
  },
};
</script>
