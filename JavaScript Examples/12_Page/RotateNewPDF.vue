<template>
  <span>The following example demonstrates how to rotate a new PDF page.</span>
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

        //Load the document from disk
        let doc = wasmModule.PdfDocument.Create();
        //Create PdfUnitConvertor to convert the unit
        let unitCvtr = wasmModule.PdfUnitConvertor.Create();
        //Setting for page margin
        let margin = wasmModule.PdfMargins.Create();
        // Set the page margins using converted units (2.54 cm top and bottom, 2.0 cm left and right)
        margin.Top = unitCvtr.ConvertUnits(
            2.54, wasmModule.PdfGraphicsUnit.Centimeter, wasmModule.PdfGraphicsUnit.Point)
        margin.Bottom = margin.Top
        margin.Left = unitCvtr.ConvertUnits(
            2.0, wasmModule.PdfGraphicsUnit.Centimeter, wasmModule.PdfGraphicsUnit.Point)
        margin.Right = margin.Left
        //Create PdfSection
        let section = doc.Sections.Add()
        //Set "A4" for Pdf page
        section.PageSettings.Size = wasmModule.PdfPageSize.A4()
        //Set page margin
        section.PageSettings.Margins = margin
        //Set rotating angle
        section.PageSettings.Rotate = wasmModule.PdfPageRotateAngle.RotateAngle90
        //Add the page
        let page = section.Pages.Add()
        //Define a PdfBrush
        let brush = wasmModule.PdfBrushes.get_Black()

        //Define a font
        let font = wasmModule.PdfTrueTypeFont.Create({
            fontFile: "/Library/Fonts/ARIALUNI.TTF",
            size: 13,
            style: wasmModule.PdfFontStyle.Bold
        });
        //Set the string format
        let format = wasmModule.PdfStringFormat.Create({alignment: wasmModule.PdfTextAlignment.Left});

        //Set the position for drawing
        let x = 0.0
        let y = 50.0
        //Text string
        let specification = "The sample demonstrates how to rotate page when creating a PDF document."
        //Draw text string on page canvas
        page.Canvas.DrawString({s: specification, font: font, brush: brush, x: x, y: y, format: format});

        // Define the output file name
        const outputFileName = "RotateNewPDF_result.pdf";

        // Save the document to the specified path
        doc.SaveToFile({fileName: outputFileName});
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
