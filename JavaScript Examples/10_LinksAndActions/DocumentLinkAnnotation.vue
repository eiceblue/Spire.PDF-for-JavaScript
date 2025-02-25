<template>
  <span>The following example demonstrates how to create a local document link in PDF document</span>
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

        // Create a new PDF document.
        let pdf = spirepdf.PdfDocument.Create();

        // Create a PdfUnitConvertor to convert units.
        let unitCvtr = spirepdf.PdfUnitConvertor.Create();

        // Set the page margins.
        let margin = spirepdf.PdfMargins.Create();
        margin.Top = unitCvtr.ConvertUnits(2.54, spirepdf.PdfGraphicsUnit.Centimeter, spirepdf.PdfGraphicsUnit.Point);
        margin.Bottom = margin.Top;
        margin.Left = unitCvtr.ConvertUnits(2.0, spirepdf.PdfGraphicsUnit.Centimeter, spirepdf.PdfGraphicsUnit.Point);
        margin.Right = margin.Left;

        // Add the first page with specified size and margins.
        let page1 = pdf.Pages.Add({ size: wasmModule.PdfPageSize.A4(), margins: margin });

        // Define a brush for drawing.
        let brush1 = spirepdf.PdfBrushes.get_Black();

        // Define a font for text.
        let trueTypeFont1 = wasmModule.PdfTrueTypeFont.Create({
            fontFile: "ARIALUNI.TTF",
            size: 12,
            style: spirepdf.PdfFontStyle.Bold
        });
        let font1 = trueTypeFont1;

        // Set the string format for text alignment.
        let format1 = spirepdf.PdfStringFormat.Create({ alignment: spirepdf.PdfTextAlignment.Left });

        // Set the position for drawing text.
        let x = 0;
        let y = 50;

        // Specify the text string.
        let specification = "The sample demonstrates how to create a local document link in PDF document.";

        // Draw the text string on the first page.
        page1.Canvas.DrawString({ s: specification, font: font1, brush: brush1, x: x, y: y, format: format1 });

        // Use MeasureString to get the height of the string.
        y = y + font1.MeasureString({ text: specification, format: format1 }).Height + 10;

        // Add the second page with specified size and margins.
        let page2 = pdf.Pages.Add({ size: wasmModule.PdfPageSize.A4(), margins: margin });

        // Specify the text string for the second page.
        let pageContent = "This is the second page!";

        // Draw the text string on the second page.
        page2.Canvas.DrawString({ s: pageContent, font: font1, brush: brush1, x: x, y: y, format: format1 });

        // Add a DocumentLinkAnnotation on the first page and link it to the second page.
        AddDocumentLinkAnnotation(pdf, 0, 1, y);

        // Define the output file name
        const outputFileName = "DocumentLinkAnnotation.pdf";

        // Save the document to the specified path
        pdf.SaveToFile({ fileName: outputFileName });
        pdf.Close();

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
function AddDocumentLinkAnnotation(pdf, AddPage, DestinationPage, y) {
    // Define a font for text.
    let trueTypeFont2 = wasmModule.PdfTrueTypeFont.Create({
            fontFile: "ARIALUNI.TTF",
            size: 12,
            style: spirepdf.PdfFontStyle.Regular
        });
    let font = trueTypeFont2;

    // Set the string format for text alignment.
    let format = spirepdf.PdfStringFormat.Create({alignment: spirepdf.PdfTextAlignment.Left});

    // Specify the prompt text for the link.
    let prompt = "Local document Link: ";

    // Draw the prompt text on the page at the specified vertical position.
    pdf.Pages.get_Item(AddPage).Canvas.DrawString({
        s: prompt,
        font: font,
        brush: spirepdf.PdfBrushes.get_DodgerBlue(),
        x: 0,
        y: y
    });

    // Use MeasureString to get the width of the prompt string.
    let x = font.MeasureString({text: prompt, format: format}).Width;

    // Create a PdfDestination with the specified destination page.
    let dest = spirepdf.PdfDestination.Create({page: pdf.Pages.get_Item(DestinationPage)});

    // Set the location of the destination.
    dest.Location = spirepdf.PointF.Create({x: 0, y: y});

    // Set the zoom factor for the destination.
    dest.Zoom = 0.5;

    // Specify the label string for the link.
    let label = "Click here to link the second page.";

    // Use MeasureString to get the SizeF of the label string.
    let size = font.MeasureString({text: label});

    // Create a rectangle that defines the area for the link.
    let bounds = spirepdf.RectangleF.Create({x: x, y: y, width: size.Width, height: size.Height});

    // Draw the label string on the page.
    pdf.Pages.get_Item(AddPage).Canvas.DrawString({
        s: label,
        font: font,
        brush: spirepdf.PdfBrushes.get_OrangeRed(),
        x: x,
        y: y
    });

    // Create a PdfDocumentLinkAnnotation on the rectangle and link it to the destination.
    let annotation = spirepdf.PdfDocumentLinkAnnotation.Create({rectangle: bounds, destination: dest});

    // Set the color for the annotation.
    annotation.Color = spirepdf.PdfRGBColor.Create({color: spirepdf.Color.get_Blue()});

    // Add the annotation to the page.
    pdf.Pages.get_Item(AddPage).Annotations.Add(annotation);
}

</script>
