<template>
  <span>The following example demonstrates how to draw text as watermark in PDF document</span>
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
        const inputFileName = "TextWaterMark.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a new PDF document object
        let doc = wasmModule.PdfDocument.Create();

        // Load an existing PDF document from VFS
        doc.LoadFromFile({ fileName: inputFileName });

        // Get the first page of the loaded document.
        let page = doc.Pages.get_Item(0);

        // Create a tiling brush for drawing a text watermark.
        let size = spirepdf.SizeF.Create({
          width: page.Canvas.ClientSize.Width / 2,
          height: page.Canvas.ClientSize.Height / 3
        });
        let brush = spirepdf.PdfTilingBrush.Create({ size: size });

        // Set transparency for the brush graphics.
        brush.Graphics.SetTransparency(0.3);

        // Save the current state of the brush graphics.
        brush.Graphics.Save();

        // Translate and rotate the brush graphics to position the watermark.
        brush.Graphics.TranslateTransform(brush.Size.Width / 2, brush.Size.Height / 2);
        brush.Graphics.RotateTransform({ angle: -45 });

        // Draw the text watermark using the brush graphics.
        let font = spirepdf.PdfFont.Create({ fontFamily: spirepdf.PdfFontFamily.Helvetica, size: 24 });
        let format = spirepdf.PdfStringFormat.Create({ alignment: spirepdf.PdfTextAlignment.Center });
        brush.Graphics.DrawString({
          s: "Spire.Pdf Demo",
          font: font, brush: spirepdf.PdfBrushes.get_Violet(), x: 0, y: 0,
          format: format
        });

        // Restore the previous state of the brush graphics.
        brush.Graphics.Restore();

        // Reset the transparency to fully opaque.
        brush.Graphics.SetTransparency({ alpha: 1 });

        // Draw a rectangle filled with the tiling brush as the watermark on the page.
        let rect = spirepdf.RectangleF.Create({
          location: spirepdf.PointF.Create({ x: 0, y: 0 }),
          size: page.Canvas.ClientSize
        });
        page.Canvas.DrawRectangle({ brush: brush, rectangle: rect });

        // Define the output file name
        const outputFileName = "TextWaterMark.pdf";

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
