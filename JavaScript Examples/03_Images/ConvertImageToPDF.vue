<template>
  <span>The following example demonstrates how to convert image file to PDF document. </span>
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
 
        let inputFileName  = "bg.png";
        await wasmModule.FetchFileToVFS(inputFileName , '', `${import.meta.env.BASE_URL}static/data/`);

        // Create a pdf instance
        let doc = wasmModule.PdfDocument.Create();
        let section = doc.Sections.Add();
        let page = section.Pages.Add();
        let image = wasmModule.PdfImage.FromFile(inputFileName);

        //Set image display location and size in PDF
        //Calculate rate
        let widthFitRate = image.PhysicalDimension.Width / page.Canvas.ClientSize.Width;
        let heightFitRate = image.PhysicalDimension.Height / page.Canvas.ClientSize.Height;
        let fitRate = Math.max(widthFitRate, heightFitRate);

        //Calculate the size of image
        let fitWidth = image.PhysicalDimension.Width / fitRate;
        let fitHeight = image.PhysicalDimension.Height / fitRate;

        //Draw image
        page.Canvas.DrawImage({image: image, x: 0, y: 30, width: fitWidth, height: fitHeight});

        // Define the output file name
        const outputFileName = "ConvertImageToPDF_out.pdf";

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
