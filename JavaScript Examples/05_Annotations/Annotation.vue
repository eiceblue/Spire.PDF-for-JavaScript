<template>
  <span>The following example demonstrates how to add annotations in PDF document.</span>
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
        await wasmModule.FetchFileToVFS("ARIALUNI.TTF", "/Library/Fonts/", `${import.meta.env.BASE_URL}static/font/`);

        // Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        // Set margins for the page
        let unitCvtr = wasmModule.PdfUnitConvertor.Create();
        let margin = wasmModule.PdfMargins.Create();
        margin.Top = unitCvtr.ConvertUnits(2.54, wasmModule.PdfGraphicsUnit.Centimeter, wasmModule.PdfGraphicsUnit.Point);
        margin.Bottom = margin.Top;
        margin.Left = unitCvtr.ConvertUnits(3, wasmModule.PdfGraphicsUnit.Centimeter, wasmModule.PdfGraphicsUnit.Point);
        margin.Right = margin.Left;

        // Create a page with specified size and margins
        let page = doc.Pages.Add({ size: wasmModule.PdfPageSize.A4(), margins: margin });

        // Add a title to the page
        let brush1 = wasmModule.PdfBrushes.get_Black();
        let fontstyle = wasmModule.PdfFontStyle.Bold.value + spirepdf.PdfFontStyle.Italic.value;
        let font1 = wasmModule.PdfTrueTypeFont.Create({
          fontName: "Arial",
          emSize: 13,
          styleNumber: fontstyle,
          unicode: true
        });
        let format1 = wasmModule.PdfStringFormat.Create({ alignment: wasmModule.PdfTextAlignment.Left });
        let y = 50;
        let s = "The sample demonstrates how to add annotations in PDF document.";

        page.Canvas.DrawString({ s: s, font: font1, brush: brush1, x: 0, y: y - 5, format: format1 });
        y = y + font1.MeasureString({ text: s, format: format1 }).Height;
        y = y + 15;

        // Add various types of annotations to the page
        y = AddDocumentLinkAnnotation(page, y);
        y = y + 6;
        y = AddFileLinkAnnotation(page, y);
        y = y + 6;
        y = AddFreeTextAnnotation(page, y);
        y = y + 6;
        y = AddLineAnnotation(page, y);
        y = y + 6;
        y = AddTextMarkupAnnotation(page, y);
        y = y + 6;
        y = AddPopupAnnotation(page, y);
        y = y + 6;
        y = AddRubberStampAnnotation(page, y);

        // Define the output file name
        const outputFileName = "Annotation_result.pdf";

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

    function AddDocumentLinkAnnotation(page, y) {
      // Set up font and formatting
      let font = wasmModule.PdfTrueTypeFont.Create({
        fontFile: "/Library/Fonts/ARIALUNI.TTF",
        size: 12,
        style: wasmModule.PdfFontStyle.Regular
      });
      let format = wasmModule.PdfStringFormat.Create();
      format.MeasureTrailingSpaces = true;

      let prompt = "Document Link: ";
      let size = font.MeasureString({ text: prompt });

      // Draw the prompt text
      page.Canvas.DrawString({ s: prompt, font: font, brush: wasmModule.PdfBrushes.get_DodgerBlue(), x: 0, y: y });
      let x = font.MeasureString(prompt, format).Width;

      // Set up the destination for the link annotation
      let dest = wasmModule.PdfDestination.Create({ page: page });
      dest.Mode = wasmModule.PdfDestinationMode.Location;
      dest.Location = wasmModule.PointF.Create({ x: 0, y: y });
      dest.Zoom = 2;

      let label = "Click me, Zoom 200%";
      size = font.MeasureString({ text: label });

      // Draw the label text
      let bounds = wasmModule.RectangleF.Create({ x: x, y: y, width: size.Width, height: size.Height });
      page.Canvas.DrawString({ s: label, font: font, brush: wasmModule.PdfBrushes.get_OrangeRed(), x: x, y: y });

      // Create a document link annotation and set its properties  rectangle, destination
      let annotation = wasmModule.PdfDocumentLinkAnnotation.Create({ rectangle: bounds, destination: dest });
      annotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Blue() });

      // Add the annotation to the page's annotation collection
      page.Annotations.Add(annotation);
      y = bounds.Bottom;

      return y;
    };

    function AddFileLinkAnnotation(page, y) {
      // Set up font and formatting
      let font = wasmModule.PdfTrueTypeFont.Create({
        fontFile: "/Library/Fonts/ARIALUNI.TTF",
        size: 12,
        style: wasmModule.PdfFontStyle.Regular
      });
      let format = wasmModule.PdfStringFormat.Create();
      format.MeasureTrailingSpaces = true;

      // Draw the prompt text
      let prompt = "Launch File:  ";
      let size = font.MeasureString({ text: prompt });

      // Draw the prompt text
      page.Canvas.DrawString({ s: prompt, font: font, brush: wasmModule.PdfBrushes.get_DodgerBlue(), x: 0, y: y });
      let x = font.MeasureString(prompt, format).Width;

      let label = "Launch Notepad.exe";
      size = font.MeasureString({ text: label });

      // Draw the label text
      let bounds = wasmModule.RectangleF.Create({ x: x, y: y, width: size.Width, height: size.Height });
      page.Canvas.DrawString({ s: label, font: font, brush: wasmModule.PdfBrushes.get_OrangeRed(), x: x, y: y });

      // Create a file link annotation and set its properties
      let annotation = wasmModule.PdfFileLinkAnnotation.Create(bounds, "D:\\Notepad.exe");
      annotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Blue() });

      // Add the annotation to the page's annotation collection
      page.Annotations.Add(annotation);
      y = bounds.Bottom;

      return y;
    };

    function AddFreeTextAnnotation(page, y) {
      // Set up font and formatting
      let font = wasmModule.PdfTrueTypeFont.Create({
        fontFile: "/Library/Fonts/ARIALUNI.TTF",
        size: 12,
        style: wasmModule.PdfFontStyle.Regular
      });
      let format = wasmModule.PdfStringFormat.Create();
      format.MeasureTrailingSpaces = true;

      // Draw the prompt text
      let prompt = "Text Markup: ";
      let size = font.MeasureString({ text: prompt, format: format });
      page.Canvas.DrawString({ s: prompt, font: font, brush: wasmModule.PdfBrushes.get_DodgerBlue(), x: 0, y: y });
      let x = size.Width;

      // Draw the label text and bounding rectangle
      let label = "I'm a text box, not a TV";
      size = font.MeasureString({ text: label });
      let bounds = wasmModule.RectangleF.Create({ x: x, y: y, width: size.Width, height: size.Height });
      let pdfRgbColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Blue() });
      let pen = wasmModule.PdfPen.Create({ pdfRgbColor: pdfRgbColor, width: 0.1 });
      page.Canvas.DrawRectangle({ pen: pen, rectangle: bounds });
      page.Canvas.DrawString({ s: label, font: font, brush: wasmModule.PdfBrushes.get_OrangeRed(), x: x, y: y });

      // Set up the free text annotation with its properties
      let location = wasmModule.PointF.Create({ x: bounds.Right + 16, y: bounds.Top - 16 });
      let annotaionBounds = wasmModule.RectangleF.Create({
        location: location,
        size: wasmModule.SizeF.Create({ width: 80, height: 32 })
      });

      let annotation = wasmModule.PdfFreeTextAnnotation.Create({ rectangle: annotaionBounds });
      annotation.AnnotationIntent = wasmModule.PdfAnnotationIntent.FreeTextCallout;
      annotation.Border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 0.5 });
      annotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Red() });
      location = wasmModule.PointF.Create({ x: bounds.Right + 16, y: bounds.Top - 16 });

      let point1 = wasmModule.PointF.Create({ x: bounds.Right, y: bounds.Top });
      let point2 = wasmModule.PointF.Create({ x: bounds.Right + 8, y: bounds.Top - 8 });
      annotation.CalloutLines = [point1, point2, location];
      annotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Yellow() });
      annotation.Flags = wasmModule.PdfAnnotationFlags.Locked;
      annotation.Font = font;
      annotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.OpenArrow;
      annotation.MarkupText = "Just a joke.";
      annotation.Opacity = 0.75;
      annotation.TextMarkupColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Green() });

      // Add the free text annotation to the page's annotation collection
      page.Annotations.Add(annotation);
      y = bounds.Bottom;

      return y;
    };

    function AddLineAnnotation(page, y) {
      // Set up font and formatting
      let font = wasmModule.PdfTrueTypeFont.Create({
        fontFile: "/Library/Fonts/ARIALUNI.TTF",
        size: 12,
        style: wasmModule.PdfFontStyle.Regular
      });
      let format = wasmModule.PdfStringFormat.Create();
      format.MeasureTrailingSpaces = true;

      // Draw the prompt text
      let prompt = "Line Annotation: ";
      let size = font.MeasureString({ text: prompt, format: format });
      page.Canvas.DrawString({ s: prompt, font: font, brush: wasmModule.PdfBrushes.get_DodgerBlue(), x: 0, y: y });
      let x = size.Width;

      // Draw the label text and bounding rectangle
      let label = "Line Annotation";
      size = font.MeasureString({ text: label });
      page.Canvas.DrawString({ s: label, font: font, brush: wasmModule.PdfBrushes.get_OrangeRed(), x: x, y: y });
      let bounds = wasmModule.RectangleF.Create({ x: x, y: y, width: size.Width, height: size.Height });
      let linePoints = [bounds.Left, bounds.Top, bounds.Right, bounds.Bottom];

      // Create a line annotation and set its properties
      let annotation = wasmModule.PdfLineAnnotation.Create({ linePoints: linePoints, text: "Annotation" });
      annotation.BeginLineStyle = wasmModule.PdfLineEndingStyle.ClosedArrow;
      annotation.EndLineStyle = wasmModule.PdfLineEndingStyle.ClosedArrow;
      annotation.LineCaption = true;
      annotation.BackColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Black() });
      annotation.CaptionType = wasmModule.PdfLineCaptionType.Inline;

      // Add the line annotation to the page's annotation collection
      page.Annotations.Add(annotation);
      y = bounds.Bottom;

      return y;
    };

    function AddTextMarkupAnnotation(page, y) {
      // Set up font and formatting
      let font = wasmModule.PdfTrueTypeFont.Create({
        fontFile: "/Library/Fonts/ARIALUNI.TTF",
        size: 12,
        style: wasmModule.PdfFontStyle.Regular
      });
      let format = wasmModule.PdfStringFormat.Create();
      format.MeasureTrailingSpaces = true;

      // Draw the prompt text
      let prompt = "Highlight incorrect spelling: ";
      let size = font.MeasureString({ text: prompt, format: format });
      page.Canvas.DrawString({ s: prompt, font: font, brush: wasmModule.PdfBrushes.get_DodgerBlue(), x: 0, y: y });
      let x = size.Width;

      // Draw the label text and bounding rectangle
      let label = "demo of anotation";
      page.Canvas.DrawString({ s: label, font: font, brush: wasmModule.PdfBrushes.get_OrangeRed(), x: x, y: y });
      size = font.MeasureString({ text: "demo of ", format: format });
      x = x + size.Width;
      let incorrectWordLocation = wasmModule.PointF.Create({ x: x, y: y });
      let markupText = "Should be 'annotation'";

      // Create a text markup annotation and set its properties
      let rect = wasmModule.RectangleF.Create({ x: x, y: y, width: 100, height: 100 });
      let annotation = wasmModule.PdfTextMarkupAnnotation.Create({
        title: markupText,
        text: "anotation",
        rect: rect,
        font: font
      });
      annotation.TextMarkupAnnotationType = wasmModule.PdfTextMarkupAnnotationType.Highlight;
      annotation.TextMarkupColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightSkyBlue() });

      // Add the text markup annotation to the page's annotation collection
      page.Annotations.Add(annotation);
      y = y + size.Height;

      return y;
    };

    function AddPopupAnnotation(page, y) {
      // Set up font and formatting
      let font = wasmModule.PdfTrueTypeFont.Create({
        fontFile: "/Library/Fonts/ARIALUNI.TTF",
        size: 12,
        style: wasmModule.PdfFontStyle.Regular
      });
      let format = wasmModule.PdfStringFormat.Create();
      format.MeasureTrailingSpaces = true;

      // Draw the prompt text
      let prompt = "Markup incorrect spelling: ";
      let size = font.MeasureString({ text: prompt, format: format });
      page.Canvas.DrawString({ s: prompt, font: font, brush: wasmModule.PdfBrushes.get_DodgerBlue(), x: 0, y: y });
      let x = size.Width;

      // Draw the label text and bounding rectangle
      let label = "demo of annotation";
      page.Canvas.DrawString({ s: label, font: font, brush: wasmModule.PdfBrushes.get_OrangeRed(), x: x, y: y });
      x = x + font.MeasureString({ text: label, format: format }).Width;
      let markupText = "All words were spelled correctly";
      size = font.MeasureString({ text: markupText });

      // Create a popup annotation and set its properties  rectangle
      let pointf = wasmModule.PointF.Create({ x: x, y: y });
      let sizef = wasmModule.SizeF.Create({ width: 0, height: 0 });
      let rect = wasmModule.RectangleF.Create({ location: pointf, size: sizef });
      let annotation = wasmModule.PdfPopupAnnotation.Create({ rectangle: rect, text: markupText });
      annotation.Icon = wasmModule.PdfPopupIcon.Paragraph;
      annotation.Open = true;
      annotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Yellow() });

      // Add the popup annotation to the page's annotation collection
      page.Annotations.Add(annotation);
      y = y + size.Height;

      return y;
    };

    function AddRubberStampAnnotation(page, y) {
      // Set up font and formatting
      let font = wasmModule.PdfTrueTypeFont.Create({
        fontFile: "/Library/Fonts/ARIALUNI.TTF",
        size: 12,
        style: wasmModule.PdfFontStyle.Regular
      });
      let format = wasmModule.PdfStringFormat.Create();
      format.MeasureTrailingSpaces = true;

      // Draw the prompt text
      let prompt = "Markup incorrect spelling: ";
      let size = font.MeasureString({ text: prompt, format: format });
      page.Canvas.DrawString({ s: prompt, font: font, brush: wasmModule.PdfBrushes.get_DodgerBlue(), x: 0, y: y });
      let x = size.Width;

      // Draw the label text and bounding rectangle
      let label = "demo of annotation";
      page.Canvas.DrawString({ s: label, font: font, brush: wasmModule.PdfBrushes.get_OrangeRed(), x: x, y: y });
      x = x + font.MeasureString({ text: label, format: format }).Width;
      let markupText = "Just a draft, not checked.";
      size = font.MeasureString({ text: markupText });

      // Create a rubber stamp annotation and set its properties
      let pointf = wasmModule.PointF.Create({ x: x, y: y });
      let sizef = wasmModule.SizeF.Create({ width: font.Height, height: font.Height });
      let rect = wasmModule.RectangleF.Create({ location: pointf, size: sizef });
      let annotation = wasmModule.PdfRubberStampAnnotation.Create({ rectangle: rect, text: markupText });
      annotation.Icon = wasmModule.PdfRubberStampAnnotationIcon.Draft;
      annotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Plum() });

      // Add the rubber stamp annotation to the page's annotation collection
      page.Annotations.Add(annotation);
      y = y + size.Height;

      return y;
    };

    return {
      startProcessing,
      downloadName,
      downloadUrl,
    };
  },
};
</script>
