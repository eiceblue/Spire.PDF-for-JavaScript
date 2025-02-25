<template>
  <span>The following example demonstrates how to set image size and insert it in PDF document.</span>
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
        // Load the sample image into the virtual file system (VFS)
        const inputImageName = "ChartImage.png";
        await wasmModule.FetchFileToVFS(inputImageName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        // Add a new page
        let page = doc.Pages.Add();

        // Load the image file
        let image = wasmModule.PdfImage.FromFile(inputImageName);

        // Set the width and height of image
        let width = image.Width * 0.75;
        let height = image.Height * 0.75;

        // Define a position to draw image
        let x = (page.Canvas.ClientSize.Width - width) / 2;
        let y = 60;

        // Draw image on page canvas
        page.Canvas.DrawImage({ image: image, x: x, y: y, width: width, height: height });

        // Define the output file name
        const outputFileName = "SetImageSize_result.pdf";

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
