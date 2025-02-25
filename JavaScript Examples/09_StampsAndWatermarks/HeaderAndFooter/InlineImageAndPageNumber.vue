<template>
  <span>The following example demonstrates how to add inline image and text in header</span>
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
        await wasmModule.FetchFileToVFS("impact.ttf", "/Library/Fonts/", `${import.meta.env.BASE_URL}static/font/`);

        const inputFileName = "PDFTemplate_HF.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);
        const inputImageName1 = "Top-logo.png";
        await wasmModule.FetchFileToVFS(inputImageName1, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a new PDF document object.
        let doc = wasmModule.PdfDocument.Create();
        // Load an existing PDF document from a file.
        doc.LoadFromFile({ fileName: inputFileName });

        // Get the first page of the loaded document.
        let page = doc.Pages.get_Item(0);

        // Define text and image
        let text1 = "Spire.Pdf is a robust component by";
        let text2 = "Technology Co., Ltd.";
        let image = spirepdf.PdfImage.FromFile(inputImageName1);

        // Define font and brush
        let trueTypeFont1 = wasmModule.PdfTrueTypeFont.Create({
            fontFile: "impact.ttf",
            size: 10,
            style: spirepdf.PdfFontStyle.Regular
        });
        let font = trueTypeFont1;
        let brush = spirepdf.PdfBrushes.get_DarkGray();

        // Measure the size of the text
        let s1 = font.MeasureString({ text: text1 });
        let s2 = font.MeasureString({ text: text2 });

        let x = 10;
        let y = 10;

        // Define the size of the image
        let imgSize = spirepdf.SizeF.Create({ width: image.Width / 2, height: image.Height / 2 });

        // Define the rectangle and string format alignment, lineAlignment
        let size = spirepdf.SizeF.Create({ width: s1.Width, height: imgSize.Width });
        let rect1 = spirepdf.RectangleF.Create({ location: spirepdf.PointF.Create({ x: x, y: y }), size: size });
        let format = spirepdf.PdfStringFormat.Create({
          alignment: spirepdf.PdfTextAlignment.Left,
          lineAlignment: spirepdf.PdfVerticalAlignment.Middle
        });

        // Draw the text1
        page.Canvas.DrawString({ s: text1, font: font, brush: brush, layoutRectangle: rect1, format: format });

        // Draw the image
        x += s1.Width;
        page.Canvas.DrawImage({ image: image, point: spirepdf.PointF.Create({ x: x, y: y }), size: imgSize });

        // Draw the text2
        x += imgSize.Width;
        size = spirepdf.SizeF.Create({ width: s2.Width, height: imgSize.Height });
        rect1 = spirepdf.RectangleF.Create({ location: spirepdf.PointF.Create({ x: x, y: y }), size: size });
        page.Canvas.DrawString({ s: text2, font: font, brush: brush, layoutRectangle: rect1, format: format });



        // Define the output file name
        const outputFileName = "InlineImageAndPageNumber.pdf";

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
