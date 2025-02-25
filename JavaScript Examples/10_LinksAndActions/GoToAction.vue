<template>
  <span>The following example demonstrates how to create Go-To action in PDF document</span>
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
        await wasmModule.FetchFileToVFS("ARIALUNI.TTF", "", `${import.meta.env.BASE_URL}static/font/`);

        // Create a new PDF document
        let pdf = spirepdf.PdfDocument.Create();

        // Add a page to the document
        let page = pdf.Pages.Add();

        // Add an embedded GoToE (Go-To Embedded) action to the PDF
        EmbeddedGoToAction(pdf, page);

        // Create an action that jumps to a specific location.
        JumpToSpecificLocationAction(pdf, page);

        // Define the output file name
        const outputFileName = "GoToAction.pdf";

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
function EmbeddedGoToAction(pdf, page) {
  
  // Add an attachment to the PDF
  const inputFileName = "GoToAction.pdf";
  wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);
  let attachment = spirepdf.PdfAttachment.Create({ fileName: inputFileName });
  pdf.Attachments.Add({ attachment: attachment });

  // Specify the text to be displayed on the page.
  let text = "Test embedded go-to action! Clicking this will open the attached PDF in a new window.";

  // Define the font and dimensions of the text box.
  let trueTypeFont1 = wasmModule.PdfTrueTypeFont.Create({
            fontFile: "ARIALUNI.TTF",
            size: 13,
            style: spirepdf.PdfFontStyle.Regular
        });
  let font = trueTypeFont1;
  let width = 490;
  let height = font.Height * 2.2;
  let rect = spirepdf.RectangleF.Create({ x: 0, y: 100, width: width, height: height });

  // Draw the text on the page.
  page.Canvas.DrawString({ s: text, font: font, brush: spirepdf.PdfBrushes.get_Black(), layoutRectangle: rect });

  // Create a PdfDestination with a specific page, location, and zoom factor.
  let dest = spirepdf.PdfDestination.Create({ page: page, location: spirepdf.PointF.Create({ x: 0, y: 842 }) });

  // Create a GoToE (Go-To Embedded) action with the specified destination.
  let action = spirepdf.PdfEmbeddedGoToAction.Create(attachment.FileName, dest, true);

  // Create a PdfActionAnnotation with the action and the annotation rectangle.
  let annotation = spirepdf.PdfActionAnnotation.Create(rect, action);

  // Add the annotation to the page.
  page.Annotations.Add(annotation);
}

function JumpToSpecificLocationAction(pdf, page) {
  // Add a new page to the PDF document.
  let pagetwo = pdf.Pages.Add();

  // Draw text on the second page.
  pagetwo.Canvas.DrawString({
    s: "This is Page Two.",
    font: spirepdf.PdfFont.Create({ fontFamily: spirepdf.PdfFontFamily.Helvetica, size: 20 }),
    brush: spirepdf.PdfSolidBrush.Create({ color: spirepdf.Color.get_Black() }),
    x: 10, y: 10
  });

  // Create a PdfDestination instance and link it to a PdfGoToAction.
  let pageBottomDest = spirepdf.PdfDestination.Create({ page: pagetwo });
  pageBottomDest.Location = spirepdf.PointF.Create({ x: 0, y: 5 });
  pageBottomDest.Mode = spirepdf.PdfDestinationMode.Location;
  pageBottomDest.Zoom = 1;
  let action = spirepdf.PdfGoToAction.Create({ destination: pageBottomDest });

  // Define the font, dimensions, and formatting for a button.
  let trueTypeFont1 = wasmModule.PdfTrueTypeFont.Create({
            fontFile: "ARIALUNI.TTF",
            size: 10,
            style: spirepdf.PdfFontStyle.Bold
        });
  let buttonFont = trueTypeFont1;
  let buttonWidth = 70;
  let buttonHeight = buttonFont.Height * 1.5;
  let format2 = spirepdf.PdfStringFormat.Create({
    alignment: spirepdf.PdfTextAlignment.Center,
    lineAlignment: spirepdf.PdfVerticalAlignment.Middle
  });
  let buttonBounds = spirepdf.RectangleF.Create({ x: 0, y: 200, width: buttonWidth, height: buttonHeight });

  // Create a rectangle and draw text on the first page.
  page.Canvas.DrawRectangle({ brush: spirepdf.PdfBrushes.get_DarkGray(), rectangle: buttonBounds });
  page.Canvas.DrawString({
    s: "To Last Page",
    font: buttonFont,
    brush: spirepdf.PdfBrushes.get_CadetBlue(),
    layoutRectangle: buttonBounds,
    format: format2
  });

  // Add the annotation to the first page.
  let annotation = spirepdf.PdfActionAnnotation.Create(buttonBounds, action);
  annotation.Border = spirepdf.PdfAnnotationBorder.Create({ borderWidth: 0.75 });

  annotation.Color = spirepdf.PdfRGBColor.Create({ color: spirepdf.Color.get_LightGray() });
  page.Annotations.Add(annotation);
}



</script>
