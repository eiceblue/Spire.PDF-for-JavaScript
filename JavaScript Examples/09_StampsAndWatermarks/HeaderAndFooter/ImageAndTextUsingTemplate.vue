<template>
  <span>The following example shows how to add image/text in header/footer using PDF template</span>
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
        // await wasmModule.FetchFileToVFS("ARIALUNI.TTF", "/Library/Fonts/", `${import.meta.env.BASE_URL}static/font/`);
        await wasmModule.FetchFileToVFS("impact.ttf", "/", `${import.meta.env.BASE_URL}static/font/`);

        // Load the input file into the virtual file system (VFS)
        const inputFileName = "PDFTemplate_HF.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);
        const inputImageName1 = "E-iceblueLogo.png";
        await wasmModule.FetchFileToVFS(inputImageName1, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a new PDF document object.
        let doc = wasmModule.PdfDocument.Create();
        // Load an existing PDF document from a file.
        doc.LoadFromFile({ fileName: inputFileName });

        // Get the first page of the loaded document.
        let page = doc.Pages.get_Item(0);

        //Get the margins of Pdf
        let margin = doc.PageSettings.Margins;

        //Define font and brush
        let trueTypeFont1 = wasmModule.PdfTrueTypeFont.Create({
          fontFile: "impact.ttf",
          size: 14,
          style: spirepdf.PdfFontStyle.Regular
        });
        let font = trueTypeFont1;
        let brush = spirepdf.PdfSolidBrush.Create({ color: spirepdf.Color.get_Gray() });

        //Load an image
        let image = spirepdf.PdfImage.FromFile(inputImageName1);

        //Specify the image size
        let imageSize = spirepdf.SizeF.Create({ width: image.Width / 2, height: image.Height / 2 });

        //Create a header template
        let headerTemplate = spirepdf.PdfTemplate.Create({
          width: page.ActualSize.Width - margin.Left - margin.Right,
          height: imageSize.Height
        });

        //Draw the image in the template  image, point, size
        headerTemplate.Graphics.DrawImage({ image: image, point: spirepdf.PointF.Create({ x: 0, y: 0 }), size: imageSize });

        //Create a retangle
        let rect = headerTemplate.GetBounds();

        //string format  alignment, lineAlignment
        let format1 = spirepdf.PdfStringFormat.Create({
          alignment: spirepdf.PdfTextAlignment.Right,
          lineAlignment: spirepdf.PdfVerticalAlignment.Middle
        });

        //Draw a string in the template
        headerTemplate.Graphics.DrawString({ s: "Header", font: font, brush: brush, layoutRectangle: rect, format: format1 });

        //Create a footer template and draw a text
        let footerTemplate = spirepdf.PdfTemplate.Create({
          width: page.ActualSize.Width - margin.Left - margin.Right,
          height: imageSize.Height
        });
        let format2 = spirepdf.PdfStringFormat.Create({
          alignment: spirepdf.PdfTextAlignment.Center,
          lineAlignment: spirepdf.PdfVerticalAlignment.Middle
        });
        footerTemplate.Graphics.DrawString({ s: "Footer", font: font, brush: brush, layoutRectangle: rect, format: format2 });

        let x = margin.Left;
        let y = 0;

        //Draw the header template on page at specified location template, location
        page.Canvas.DrawTemplate({ template: headerTemplate, location: spirepdf.PointF.Create({ x: x, y: y }) });

        //Draw the footer template on page at specified location
        y = page.ActualSize.Height - footerTemplate.Height - 10;
        page.Canvas.DrawTemplate({ template: footerTemplate, location: spirepdf.PointF.Create({ x: x, y: y }) });

        // Define the output file name
        const outputFileName = "ImageAndTextUsingTemplate.pdf";

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
