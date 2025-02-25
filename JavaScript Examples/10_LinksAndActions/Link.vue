<template>
  <span>The following example demonstrates how to add links to PDF document</span>
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
        await wasmModule.FetchFileToVFS("Lucida Sans Unicode.ttf", "", `${import.meta.env.BASE_URL}static/font/`);

        // Create a new PDF document.
        let doc = spirepdf.PdfDocument.Create();

        // Set margins for the document.
        let unitCvtr = spirepdf.PdfUnitConvertor.Create();
        let margin = spirepdf.PdfMargins.Create();
        margin.Top = unitCvtr.ConvertUnits(2.54, spirepdf.PdfGraphicsUnit.Centimeter, spirepdf.PdfGraphicsUnit.Point);
        margin.Bottom = margin.Top;
        margin.Left = unitCvtr.ConvertUnits(3.17, spirepdf.PdfGraphicsUnit.Centimeter, spirepdf.PdfGraphicsUnit.Point);
        margin.Right = margin.Left;

        // Create a new page with specified size and margins.
        let page = doc.Pages.Add({ size: spirepdf.PdfPageSize.A4(), margins: margin });
        let y = 100;
        let x = 10;

        // Set font and label for the first text link.
        let trueTypeFont1 = wasmModule.PdfTrueTypeFont.Create({
            fontFile: "Lucida Sans Unicode.ttf",
            size: 14,
            style: spirepdf.PdfFontStyle.Regular
        });
        let font = trueTypeFont1;
        let label = "Simple Text Link: ";

        // Draw the label on the page.
        let format = spirepdf.PdfStringFormat.Create();
        format.MeasureTrailingSpaces = true;
        page.Canvas.DrawString({ s: label, font: font, brush: spirepdf.PdfBrushes.get_Orange(), x: 0, y: y, format: format });
        x = font.MeasureString({ text: label, format: format }).Width;

        // Set font and URL for the first text link.
        let trueTypeFont2 = wasmModule.PdfTrueTypeFont.Create({
            fontFile: "Lucida Sans Unicode.ttf",
            size: 14,
            style: spirepdf.PdfFontStyle.Underline
        });
        let font1 = trueTypeFont2;
        let url1 = "http://www.e-iceblue.com";

        // Draw the URL on the page.
        page.Canvas.DrawString({ s: url1, font: font1, brush: spirepdf.PdfBrushes.get_CadetBlue(), x: x, y: y });
        y = y + font1.MeasureString({ text: url1 }).Height + 25;

        // Set font and label for the second web link.
        label = "Web Link: ";
        page.Canvas.DrawString({ s: label, font: font, brush: spirepdf.PdfBrushes.get_Orange(), x: 0, y: y, format: format });
        x = font.MeasureString({ text: label, format: format }).Width;

        // Set text and properties for the second web link.
        let text = "E-iceblue home";
        let link2 = spirepdf.PdfTextWebLink.Create();
        link2.Text = text;
        link2.Url = url1;
        link2.Font = font1;
        link2.Brush = spirepdf.PdfBrushes.get_CadetBlue();

        // Draw the second web link on the page. graphics, location
        link2.DrawTextWebLink({ graphics: page.Canvas, location: spirepdf.PointF.Create({ x: x, y: y }) });
        y = y + font1.MeasureString({ text: text }).Height + 30;

        // Set font and label for the URI annotation.
        label = "URI Annotation: ";
        page.Canvas.DrawString({ s: label, font: font, brush: spirepdf.PdfBrushes.get_Orange(), x: 0, y: y, format: format });
        x = font.MeasureString({ text: label, format: format }).Width;
        text = "Google";
        let location = spirepdf.PointF.Create({ x: x, y: y });
        let size = font1.MeasureString({ text: text });
        let linkBounds = spirepdf.RectangleF.Create({ location: location, size: size });

        // Create a URI annotation and set its properties.
        let link3 = spirepdf.PdfUriAnnotation.Create({ rectangle: linkBounds });
        link3.Border = spirepdf.PdfAnnotationBorder.Create({ borderWidth: 0 });
        link3.Uri = "http://www.google.com";

        // Add the URI annotation to the page's annotations collection.
        page.Annotations.Add(link3);

        // Draw the text for the URI annotation.
        page.Canvas.DrawString({ s: text, font: font1, brush: spirepdf.PdfBrushes.get_CadetBlue(), x: x, y: y });
        y = y + size.Height + 30;

        // Set font and label for the URI annotation with an action.
        label = "URI Annotation Action: ";
        page.Canvas.DrawString({ s: label, font: font, brush: spirepdf.PdfBrushes.get_Orange(), x: 0, y: y, format: format });
        x = font.MeasureString({ text: label, format: format }).Width;
        text = "JavaScript Action (Click Me)";
        location = spirepdf.PointF.Create({ x: x - 2, y: y - 2 });
        size = font1.MeasureString({ text: text });
        size = spirepdf.SizeF.Create({ width: size.Width + 5, height: size.Height + 5 });
        linkBounds = spirepdf.RectangleF.Create({ location: location, size: size });

        // Create a URI annotation with border and color.
        let link4 = spirepdf.PdfUriAnnotation.Create({ rectangle: linkBounds });
        link4.Border = spirepdf.PdfAnnotationBorder.Create({ borderWidth: 0.75 });
        link4.Color = spirepdf.PdfRGBColor.Create({ color: spirepdf.Color.get_CadetBlue() });// Color.CadetBlue;

        // Define JavaScript action script.
        let script = "app.alert({" +
          "cMsg: \"Hello.\"," +
          "nIcon: 3," +
          "cTitle: \"JavaScript Action\"" +
          "});";

        // Create a JavaScript action and assign it to the URI annotation.
        let action = spirepdf.PdfJavaScriptAction.Create(script);
        link4.Action = action;

        // Add the URI annotation with the JavaScript action to the page's annotations collection.
        page.Annotations.Add(link4);

        // Draw the text for the URI annotation with the JavaScript action.
        page.Canvas.DrawString({ s: text, font: font1, brush: spirepdf.PdfBrushes.get_CadetBlue(), x: x, y: y });
        y = y + size.Height + 30;

        label = "Need Help:  ";
        page.Canvas.DrawString({ s: label, font: font, brush: spirepdf.PdfBrushes.get_Orange(), x: 0, y: y, format: format });
        x = font.MeasureString({ text: label, format: format }).Width;
        text = "Go to forum to ask questions";
        link2 = spirepdf.PdfTextWebLink.Create();
        link2.Text = text;
        link2.Url = "https://www.e-iceblue.com/forum/components-f5.html";
        link2.Font = font1;
        link2.Brush = spirepdf.PdfBrushes.get_CadetBlue();
        link2.DrawTextWebLink({ graphics: page.Canvas, location: spirepdf.PointF.Create({ x: x, y: y }) });
        y = y + font1.MeasureString({ text: text }).Height + 30;

        label = "Contct us:  ";
        page.Canvas.DrawString({ s: label, font: font, brush: spirepdf.PdfBrushes.get_Orange(), x: 0, y: y, format: format });
        x = font.MeasureString({ text: label, format: format }).Width;
        text = "Send an email";
        link2 = spirepdf.PdfTextWebLink.Create();
        link2.Text = text;
        link2.Url = "mailto:support@e-iceblue.com";
        link2.Font = font1;
        link2.Brush = spirepdf.PdfBrushes.get_CadetBlue();
        link2.DrawTextWebLink({ graphics: page.Canvas, location: spirepdf.PointF.Create({ x: x, y: y }) });
        y = y + font1.MeasureString({ text: text }).Height + 30;

        // Define the output file name
        const outputFileName = "Link.pdf";

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
