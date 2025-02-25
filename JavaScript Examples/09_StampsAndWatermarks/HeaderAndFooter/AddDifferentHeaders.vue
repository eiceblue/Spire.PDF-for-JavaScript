<template>
  <span>The following example demonstrates how to add different headers in PDF document</span>
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

        // Create a new PDF document object
        let doc = wasmModule.PdfDocument.Create();
        // Load an existing PDF document from VFS
        doc.LoadFromFile({ fileName: inputFileName });

        // Define header texts
        let header1 = "Header 1";
        let header2 = "Header 2";

        // Define the font style for the headers
        let trueTypeFont = wasmModule.PdfTrueTypeFont.Create({
          fontFile: "ARIALUNI.TTF",
          size: 12,
          style: spirepdf.PdfFontStyle.Bold
        });

        let font = trueTypeFont

        // Define the brush color for the headers
        let brush = spirepdf.PdfBrushes.get_Red();

        // Define the rectangle to position the header on the first page
        let point = spirepdf.PointF.Create({ x: 0, y: 20 });
        let size = spirepdf.SizeF.Create({ width: doc.PageSettings.Size.Width, height: 50 });
        let rect = spirepdf.RectangleF.Create({ location: point, size: size });

        // Define the string format for the headers, aligning them in the center
        let format = spirepdf.PdfStringFormat.Create();
        format.Alignment = spirepdf.PdfTextAlignment.Center;

        // Draw the first header with the defined font, brush, rectangle, and format on the first page of the document
        doc.Pages.get_Item(0).Canvas.DrawString({
          s: header1,
          font: font,
          brush: brush,
          layoutRectangle: rect,
          format: format
        });

        // Change the font style and brush color for the second header
        // new PdfTrueTypeFont(new Font("Aleo", 15f, FontStyle.Regular));

        let trueTypeFont1 = wasmModule.PdfTrueTypeFont.Create({
          fontFile: "ARIALUNI.TTF",
          size: 15,
          style: spirepdf.PdfFontStyle.Regular
        });
        font = trueTypeFont1;
        brush = spirepdf.PdfBrushes.get_Black();

        // Change the alignment of the string format to left alignment for the second header
        format.Alignment = spirepdf.PdfTextAlignment.Left;

        // Draw the second header with the updated font, brush, rectangle, and format on the second page of the document

        doc.Pages.get_Item(1).Canvas.DrawString({
          s: header2,
          font: font,
          brush: brush,
          layoutRectangle: rect,
          format: format
        });

        // Define the output file name
        const outputFileName = "AddingDifferentHeaders.pdf";

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
