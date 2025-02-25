<template>
  <span>The following example demonstrates how to add header and footer in PDF document</span>
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

        // Load the input file into the virtual file system (VFS)
        const inputFileName = "HeaderAndFooter.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);
        const inputImageName1 = "Header.png";
        await wasmModule.FetchFileToVFS(inputImageName1, "", `${import.meta.env.BASE_URL}static/data/`);
        const inputImageName2 = "Footer.png";
        await wasmModule.FetchFileToVFS(inputImageName2, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a new PDF document object.
        let doc = wasmModule.PdfDocument.Create();
        // Load an existing PDF document from a file.
        doc.LoadFromFile({ fileName: inputFileName });

        // Define the brush for text drawing
        let brush = spirepdf.PdfBrushes.get_Black();

        // Define the pen for line drawing
        let pen = spirepdf.PdfPen.Create({ brush: brush, width: 0.75 });

        // Define the font for text drawing
        let trueTypeFont1 = wasmModule.PdfTrueTypeFont.Create({
          fontFile: "ARIALUNI.TTF",
          size: 10,
          style: spirepdf.PdfFontStyle.Italic
        });
        let font = trueTypeFont1

        // Define the string format for right-aligned text
        let rightAlign = spirepdf.PdfStringFormat.Create({ alignment: spirepdf.PdfTextAlignment.Right });

        // Define the string format for left-aligned text
        let leftAlign = spirepdf.PdfStringFormat.Create({ alignment: spirepdf.PdfTextAlignment.Left });

        rightAlign.MeasureTrailingSpaces = true;
        leftAlign.MeasureTrailingSpaces = true;

        // Get the page margins of the document
        let margin = doc.PageSettings.Margins;

        // Calculate the spacing between lines
        let space = font.Height * 0.75;

        // Initialize variables for position and width calculation
        let x = 0;
        let y = 0;
        let width = 0;

        // Create a new PDF document
        let newPdf = spirepdf.PdfDocument.Create();
        let newPage;

        for (let i = 0; i < doc.Pages.Count; i++) {
          let page = doc.Pages.get_Item(i);
          // Add a new page to the new PDF document size, margins
          newPage = newPdf.Pages.Add({ size: page.Size, margins: spirepdf.PdfMargins.Create({ margin: 0 }) });

          // Set transparency for the canvas
          newPage.Canvas.SetTransparency({ alpha: 0.5 });

          // Calculate the position and width for header elements
          x = margin.Left;
          width = page.Canvas.ClientSize.Width - margin.Left - margin.Right;
          y = margin.Top - space;

          // Draw a line as a header separator
          newPage.Canvas.DrawLine({ pen: pen, x1: x, y1: y + 15, x2: x + width, y2: y + 15 });
          y = y + 10 - font.Height;

          // Load the header image
          let headerImage = spirepdf.PdfImage.FromFile(inputImageName1);

          // Draw the header image on the new page
          newPage.Canvas.DrawImage({ image: headerImage, point: spirepdf.PointF.Create({ x: 0, y: 0 }) });

          // Draw the header text on the new page with right alignment
          newPage.Canvas.DrawString({
            s: "Demo of Spire.Pdf",
            font: font,
            brush: brush,
            x: x + width,
            y: y,
            format: rightAlign
          });

          // Load the footer image
          let footerImage = spirepdf.PdfImage.FromFile(inputImageName2);

          // Draw the footer image on the new page
          let point1 = spirepdf.PointF.Create({
            x: 0,
            y: newPage.Canvas.ClientSize.Height - footerImage.PhysicalDimension.Height
          });
          newPage.Canvas.DrawImage({ image: footerImage, point: point1 });

          // Change the font and brush for the footer text
          brush = spirepdf.PdfBrushes.get_DarkBlue();
          let trueTypeFont2 = wasmModule.PdfTrueTypeFont.Create({
            fontFile: "ARIALUNI.TTF",
            size: 12,
            style: spirepdf.PdfFontStyle.Bold
          });
          font = trueTypeFont2;
          y = newPage.Canvas.ClientSize.Height - margin.Bottom - font.Height;

          // Draw the footer text on the new page with left alignment
          newPage.Canvas.DrawString({
            s: "Created by E-iceblue Co,.Ltd",
            font: font,
            brush: brush,
            x: x,
            y: y,
            format: leftAlign
          });

          // Reset the transparency of the canvas
          newPage.Canvas.SetTransparency({ alpha: 1 });

          // Draw the original page content onto the new page
          let template = page.CreateTemplate();
          let graphicsWidget = new spirepdf.PdfGraphicsWidget(template.H);
          graphicsWidget.Draw({ graphics: newPage.Canvas, location: spirepdf.PointF.Create({ x: 0, y: 0 }) });
        }

        // Define the output file name
        const outputFileName = "HeaderAndFooter.pdf";

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
