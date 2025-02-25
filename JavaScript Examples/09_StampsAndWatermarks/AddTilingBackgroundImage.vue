<template>
  <span>The following example demonstrates how to add tiling background image in PDF document</span>
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
        const inputFileName = "PDFTemplate_N.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);
        const inputImageName1 = "E-iceblueLogo.png";
        await wasmModule.FetchFileToVFS(inputImageName1, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a new PDF document object
        let doc = wasmModule.PdfDocument.Create();
        // Load an existing PDF document from VFS
        doc.LoadFromFile({ fileName: inputFileName });

        // Load an image to be used as the background
        let image = spirepdf.PdfImage.FromFile(inputImageName1);

        // Iterate through each page in the PDF document
        for (let i = 0; i < doc.Pages.Count; i++) {
          let page = doc.Pages.get_Item(i);
          // Create a PdfTilingBrush with a size relative to the page canvas
          let brush = spirepdf.PdfTilingBrush.Create({
            size: spirepdf.SizeF.Create({
              width: page.Canvas.Size.Width / 3,
              height: page.Canvas.Size.Height / 5
            })
          });

          // Set the transparency of the brush graphics
          brush.Graphics.SetTransparency({ alpha: 0.3 });

          // Draw the image onto the brush graphics, centered within the brush
          let point = spirepdf.PointF.Create({
            x: (brush.Size.Width - image.Width) / 2,
            y: (brush.Size.Height - image.Height) / 2
          });
          brush.Graphics.DrawImage({ image: image, point: point });

          // Use the brush to draw a rectangle on the page canvas, covering the entire page area
          let rect = spirepdf.RectangleF.Create({ location: spirepdf.PointF.Create({ x: 0, y: 0 }), size: page.Canvas.Size });
          page.Canvas.DrawRectangle({ brush: brush, rectangle: rect });
        }

        // Define the output file name
        const outputFileName = "AddTilingBackgroundImage.pdf";

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
