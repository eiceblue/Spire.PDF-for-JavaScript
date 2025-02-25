<template>
  <span>The following example demonstrates how to add image and page number in header and footer in PDF document</span>
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

        // Load the input file into the virtual file system (VFS)
        const inputImageName1 = "E-iceblueLogo.png";
        await wasmModule.FetchFileToVFS(inputImageName1, "/Library/Fonts/", `${import.meta.env.BASE_URL}static/data/`);

        // Create a new PDF document object.
        let doc = wasmModule.PdfDocument.Create();
        doc.PageSettings.Size = spirepdf.PdfPageSize.A4();

        //reset the default margins to 0
        doc.PageSettings.Margins = spirepdf.PdfMargins.Create();

        //create a PdfMargins object, the parameters indicate the page margins you want to set left, top, right, bottom
        let margins = spirepdf.PdfMargins.Create({ left: 50, top: 50, right: 50, bottom: 50 });

        //get page size
        let pageSize = doc.PageSettings.Size;

        //create a header template with content and apply it to page template
        doc.Template.Top = CreateHeaderTemplate(doc, margins, pageSize, inputImageName1);

        //create a footer template with content and apply it to page template
        doc.Template.Bottom = CreateFooterTemplate(doc, margins, pageSize);

        //apply blank templates to other parts of page template   width, height
        doc.Template.Left = spirepdf.PdfPageTemplateElement.Create({
          width: margins.Left,
          height: doc.PageSettings.Size.Height
        });
        doc.Template.Right = spirepdf.PdfPageTemplateElement.Create({
          width: margins.Right,
          height: doc.PageSettings.Size.Height
        });

        // Define the output file name
        const outputFileName = "ImageAndPageNumber.pdf";

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

function CreateHeaderTemplate(doc, margins, pageSize, inputImageName1) {
  //create a PdfPageTemplateElement object as header space
  let headerSpace = spirepdf.PdfPageTemplateElement.Create({ width: pageSize.Width, height: margins.Top });
  headerSpace.Foreground = false;

  //declare two float variables
  let x = margins.Left;
  let y = 0;

  //draw image in header space
  // Load the footer image
  let headerImage = spirepdf.PdfImage.FromFile(inputImageName1);
  let width = headerImage.Width / 2;
  let height = headerImage.Height / 2;
  headerSpace.Graphics.DrawImage({
    image: headerImage,
    x: x,
    y: margins.Top - height - 5,
    width: width,
    height: height
  });

  //draw line in header space
  let pen = spirepdf.PdfPen.Create({ brush: spirepdf.PdfBrushes.get_LightGray(), width: 1 });
  headerSpace.Graphics.DrawLine({
    pen: pen,
    x1: x,
    y1: y + margins.Top - 2,
    x2: pageSize.Width - x,
    y2: y + margins.Top - 2
  });

  //return headerSpace
  return headerSpace;
}

function CreateFooterTemplate(doc, margins, pageSize) {
  // Create a PdfPageTemplateElement object to serve as the footer space
  let footerSpace = spirepdf.PdfPageTemplateElement.Create({ width: pageSize.Width, height: margins.Bottom });
  footerSpace.Foreground = false;

  // Declare two float variables for positioning
  let x = margins.Left;
  let y = 0;

  // Draw a line in the footer space using a gray pen
  let pen = spirepdf.PdfPen.Create({ brush: spirepdf.PdfBrushes.get_Gray(), width: 1 });
  footerSpace.Graphics.DrawLine({ pen: pen, x1: x, y1: y, x2: pageSize.Width - x, y2: y });

  // Draw text in the footer space
  y = y + 5;

  // Define the font for the text
  let trueTypeFont1 = wasmModule.PdfTrueTypeFont.Create({
    fontFile: "ARIALUNI.TTF",
    size: 10,
    style: spirepdf.PdfFontStyle.Regular
  });
  let font = trueTypeFont1;

  // Create a dynamic field for the page number
  let number = spirepdf.PdfPageNumberField.Create();

  // Create a dynamic field for the page count
  let count = spirepdf.PdfPageCountField.Create();
  let lists = [number, count];

  // Create a composite field that combines the page number and page count fields
  let compositeField = spirepdf.PdfCompositeField.Create({
    font: font,
    brush: spirepdf.PdfBrushes.get_Black(),
    text: "Page {0} of {1}",
    list: lists
  });
  // Set the string format for the composite field (left-aligned, top vertical alignment)
  compositeField.StringFormat = spirepdf.PdfStringFormat.Create({
    alignment: spirepdf.PdfTextAlignment.Left,
    lineAlignment: spirepdf.PdfVerticalAlignment.Top
  });

  // Measure the size of the composite field to determine its bounds
  let size = font.MeasureString({ text: compositeField.Text });
  compositeField.Bounds = spirepdf.RectangleF.Create({ x: x, y: y, width: size.Width, height: size.Height });

  // Draw the composite field on the footer space
  let graphicsWidget = new spirepdf.PdfGraphicsWidget(compositeField.H);
  graphicsWidget.Draw({ graphics: footerSpace.Graphics });

  // Return the footer space element
  return footerSpace;
}

</script>
