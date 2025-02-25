<template>
  <span>The following example demonstrates how to add text stamp in PDF document</span>
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
        // Load the ARIALUNI.TTF font file into the virtual file system (VFS)
        await wasmModule.FetchFileToVFS("ELEPHNTI.ttf", "", `${import.meta.env.BASE_URL}static/font/`);
        await wasmModule.FetchFileToVFS("Lucida Sans Unicode.ttf", "", `${import.meta.env.BASE_URL}static/font/`);

        // Load the input file into the virtual file system (VFS)
        const inputFileName = "AddTextStamp.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a new PDF document object
        let doc = wasmModule.PdfDocument.Create();
        // Load an existing PDF document from a file
        doc.LoadFromFile({ fileName: inputFileName });

        // Get the first page of the loaded document.
        let page = doc.Pages.get_Item(0);

        // Create a template with specific dimensions to hold the stamp
        let template = spirepdf.PdfTemplate.Create({ width: 125, height: 55 });

        // Set the font, brush, and pen for drawing the stamp
        let trueTypeFont = wasmModule.PdfTrueTypeFont.Create({
            fontFile: "ELEPHNTI.ttf",
            size: 12,
            style: spirepdf.PdfFontStyle.Italic
        });
        let font1 = trueTypeFont;

        let brush = spirepdf.PdfSolidBrush.Create({ color: spirepdf.Color.get_DarkRed() });
        let pen = spirepdf.PdfPen.Create({ brush: brush });

        // Define a rectangle that represents the bounds of the stamp
        let rectangle = spirepdf.RectangleF.Create({ location: spirepdf.PointF.Create({ x: 5, y: 5 }), size: template.Size });

        // Define the corner radius for the stamp's rounded corners
        let CornerRadius = 20;

        // Create a path for the stamp shape using arcs and lines
        let path = spirepdf.PdfPath.Create();
        //x, y, width, height, startAngle, sweepAngle
        path.AddArc({
          x: template.GetBounds().X,
          y: template.GetBounds().Y,
          width: CornerRadius,
          height: CornerRadius,
          startAngle: 180,
          sweepAngle: 90
        });
        path.AddArc({
          x: template.GetBounds().X + template.Width - CornerRadius,
          y: template.GetBounds().Y,
          width: CornerRadius,
          height: CornerRadius,
          startAngle: 270,
          sweepAngle: 90
        });
        path.AddArc({
          x: template.GetBounds().X + template.Width - CornerRadius,
          y: template.GetBounds().Y + template.Height - CornerRadius,
          width: CornerRadius,
          height: CornerRadius,
          startAngle: 0,
          sweepAngle: 90
        });
        path.AddArc({
          x: template.GetBounds().X,
          y: template.GetBounds().Y + template.Height - CornerRadius,
          width: CornerRadius,
          height: CornerRadius,
          startAngle: 90,
          sweepAngle: 90
        });
        path.AddLine({
          x1: template.GetBounds().X,
          y1: template.GetBounds().Y + template.Height - CornerRadius,
          x2: template.GetBounds().X,
          y2: template.GetBounds().Y + CornerRadius / 2
        });

        // Draw the stamp shape on the template
        template.Graphics.DrawPath({ pen: pen, path: path });

        // Draw the stamp text on the template
        let s1 = "REVISED\n";
        let s2 = "by E-iceblue at " + spirepdf.DateTime.get_Now().ToString({ format: "MM dd, yyyy" });
        template.Graphics.DrawString({ s: s1, font: font1, brush: brush, point: spirepdf.PointF.Create({ x: 5, y: 10 }) });

        let trueTypeFont2 = wasmModule.PdfTrueTypeFont.Create({
            fontFile: "Lucida Sans Unicode.ttf",
            size: 9,
            style: spirepdf.PdfFontStyle.Bold
        });
        let font2 = trueTypeFont2;
        template.Graphics.DrawString({ s: s2, font: font2, brush: brush, point: spirepdf.PointF.Create({ x: 2, y: 30 }) });

        // Create a rubber stamp annotation with the specified rectangle for its size and position
        let stamp = spirepdf.PdfRubberStampAnnotation.Create({ rectangle: rectangle });

        // Create an appearance object for the rubber stamp annotation
        let apprearance = spirepdf.PdfAppearance.Create(stamp);

        // Set the normal appearance of the stamp to use the created template
        apprearance.Normal = template;

        // Assign the custom appearance to the rubber stamp annotation
        stamp.Appearance = apprearance;

        // Add the rubber stamp annotation to the page's annotations widget
        page.AnnotationsWidget.Add(stamp);

        // Define the output file name
        const outputFileName = "AddTextStamp.pdf";

        // Save the document to the specified path
        doc.SaveToFile({ fileName: outputFileName });
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
