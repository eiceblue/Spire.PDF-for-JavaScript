<template>
  <span>The following example demonstrates how to add file link annotation in PDF document</span>
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
        let doc = spirepdf.PdfDocument.Create();

        // Create a PdfUnitConvertor to convert units.
        let unitCvtr = spirepdf.PdfUnitConvertor.Create();

        // Set the page margins.
        let margin = spirepdf.PdfMargins.Create();
        margin.Top = unitCvtr.ConvertUnits(2.54, spirepdf.PdfGraphicsUnit.Centimeter, spirepdf.PdfGraphicsUnit.Point);
        margin.Bottom = margin.Top;
        margin.Left = unitCvtr.ConvertUnits(3.0, spirepdf.PdfGraphicsUnit.Centimeter, spirepdf.PdfGraphicsUnit.Point);
        margin.Right = margin.Left;

        // Add the first page with specified size and margins.
        let page = doc.Pages.Add({ size: wasmModule.PdfPageSize.A4(), margins: margin });

        //Define a PdfBrush
        let brush1 = spirepdf.PdfBrushes.get_Black();

        //Define a font
        let trueTypeFont1 = wasmModule.PdfTrueTypeFont.Create({
            fontFile: "ARIALUNI.TTF",
            size: 13,
            style: spirepdf.PdfFontStyle.Bold
        });
        let font1 = trueTypeFont1;// new PdfTrueTypeFont(new Font("Arial", 13f, FontStyle.Bold), true);

        //Set the string format
        let format1 = spirepdf.PdfStringFormat.Create({ alignment: spirepdf.PdfTextAlignment.Left });

        //Set the position for drawing
        let x = 0;
        let y = 50;

        //Text string
        let specification = "The sample demonstrates how to create a file link in PDF document.";

        //Draw text string on page canvas
        page.Canvas.DrawString({ s: specification, font: font1, brush: brush1, x: x, y: y, format: format1 });

        //Use MeasureString to get the height of string
        y = y + font1.MeasureString({ text: specification, format: format1 }).Height + 10;

        //Add file link annotation
        AddFileLinkAnnotation(page, y);


        // Define the output file name
        const outputFileName = "FileLinkAnnotation.pdf";

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
function AddFileLinkAnnotation(page, y) {
    //Define a font
    let trueTypeFont1 = wasmModule.PdfTrueTypeFont.Create({
            fontFile: "ARIALUNI.TTF",
            size: 12,
            style: spirepdf.PdfFontStyle.Regular
        });
    let font = trueTypeFont1;

    //Set the string format
    let format = spirepdf.PdfStringFormat.Create({alignment: spirepdf.PdfTextAlignment.Left});

    //Text string
    let prompt = "Launch a File: ";

    //Draw text string on page canvas
    page.Canvas.DrawString({s: prompt, font: font, brush: spirepdf.PdfBrushes.get_DodgerBlue(), x: 0, y: y});

    //Use MeasureString to get the width of string
    let x = font.MeasureString({text: prompt, format: format}).Width;

    //String of file name
    let label = "Sample.pdf";

    //Use MeasureString to get the SizeF of string
    let size = font.MeasureString({text: label});

    //Create a rectangle
    let bounds = spirepdf.RectangleF.Create({x: x, y: y, width: size.Width, height: size.Height});

    //Draw label string
    page.Canvas.DrawString({s: label, font: font, brush: spirepdf.PdfBrushes.get_OrangeRed(), x: x, y: y});

    //Create PdfFileLinkAnnotation on the rectangle and link file "Sample.pdf"
    const inputFileName1 = "Sample.pdf";
    wasmModule.FetchFileToVFS(inputFileName1, "", `${import.meta.env.BASE_URL}static/data/`);

    //let soundAction = spirepdf.PdfSoundAction.Create(inputFileName1);
    let annotation = spirepdf.PdfFileLinkAnnotation.Create(bounds, inputFileName1);

    //Set color for annotation
    annotation.Color = spirepdf.PdfRGBColor.Create({color: spirepdf.Color.get_Blue()});

    page.Annotations.Add(annotation);
}


</script>
