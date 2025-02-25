<template>
  <span>The following example demonstrates how to add page number in footer in PDF document</span>
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
        const inputFileName = "MultipagePDF.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a new PDF document object.
        let doc = wasmModule.PdfDocument.Create();
        // Load an existing PDF document from a file.
        doc.LoadFromFile({ fileName: inputFileName });

        // Set the margin
        let margin = doc.PageSettings.Margins;

        // Draw page numbers
        DrawPageNumber(doc, margin, 1, doc.Pages.Count);

        // Define the output file name
        const outputFileName = "PageNumberInFooter.pdf";

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
function DrawPageNumber(doc, margin, startNumber, pageCount) {
  // Iterate through each page in the document
  for (let i = 0; i < doc.Pages.Count; i++) {
    let page = doc.Pages.get_Item(i);
    // Set transparency for the canvas
    page.Canvas.SetTransparency({ alpha: 0.5 });

    // Define brush, pen, font, and string format for drawing the page number
    let brush = spirepdf.PdfBrushes.get_Black();
    let pen = spirepdf.PdfPen.Create({ brush: brush, width: 0.75 });
    let trueTypeFont1 = wasmModule.PdfTrueTypeFont.Create({
      fontFile: "ARIALUNI.TTF",
      size: 12,
      style: spirepdf.PdfFontStyle.Regular
    });
    let font = trueTypeFont1;
    let format = spirepdf.PdfStringFormat.Create({ alignment: spirepdf.PdfTextAlignment.Right });
    format.MeasureTrailingSpaces = true;

    // Calculate the spacing between lines
    let space = font.Height * 0.75;

    // Define the initial position
    let x = margin.Left;
    let width = page.Canvas.ClientSize.Width - margin.Left - margin.Right;
    let y = page.Canvas.ClientSize.Height - margin.Bottom + space;

    // Draw a line above the page number
    page.Canvas.DrawLine({ pen: pen, x1: x, y1: y, x2: x + width, y2: y });

    // Adjust the position and draw the page number label
    y = y + 1;
    let numberLabel = (startNumber++) + " of " + pageCount ;
    page.Canvas.DrawString({ s: numberLabel, font: font, brush: brush, x: x + width, y: y, format: format });

    // Reset the transparency for the canvas
    page.Canvas.SetTransparency({ alpha: 1 });
  }
}

</script>
