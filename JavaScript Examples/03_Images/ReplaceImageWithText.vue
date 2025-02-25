<template>
  <span>The following example demonstrates how to replace image with text.</span>
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
        // Load the sample file into the virtual file system (VFS)
        let inputFileName = "ReplaceImage.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        // Load the PDF fle
        doc.LoadFromFile({ fileName: inputFileName });

        // Get the first page
        let page = doc.Pages.get_Item(0);

        // Create PdfImageHelper object to get Image from page
        let helper = wasmModule.PdfImageHelper.Create();
        let images = helper.GetImagesInfo(page);

        // Get width and height of image
        let width = images[0].Bounds.Width;
        let height = images[0].Bounds.Height;

        //Get location of image
        let xPos = images[0].Bounds.X;
        let yPos = images[0].Bounds.Y;

        // Remove the image
        helper.DeleteImage({ imageInfo: images[0] });

        // Define a rectangle location,size
        let point = wasmModule.PointF.Create({ x: xPos, y: yPos });
        let size = wasmModule.SizeF.Create({ width: width, height: height });
        let rect = wasmModule.RectangleF.Create({ location: point, size: size });

        // Define string format
        let format = wasmModule.PdfStringFormat.Create();
        format.Alignment = wasmModule.PdfTextAlignment.Center;
        format.LineAlignment = wasmModule.PdfVerticalAlignment.Middle;

        let font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 18 });

        // Draw the string at the location
        page.Canvas.DrawString({
          s: "ReplacedText",
          font: font,
          brush: wasmModule.PdfBrushes.get_Purple(),
          layoutRectangle: rect,
          format: format
        });

        // Define the output file name
        const outputFileName = "ReplaceImageWithText_result.pdf";

        // Save the document to the specified path
        doc.SaveToFile({ fileName: outputFileName });

        // Clean up resources
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
