<template>
  <span>The following example demonstrates how to delete image in PDF document.</span>
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
 
        let inputFileName  = "DeleteImage.pdf";
        await wasmModule.FetchFileToVFS(inputFileName , '', `${import.meta.env.BASE_URL}static/data/`);

        // Create a pdf instance
        let doc = wasmModule.PdfDocument.Create();

        //Load the PDF document from the specified file
        doc.LoadFromFile({fileName: inputFileName});

        // Get the first page of the PDF document
        let page = doc.Pages.get_Item(0);

        // Create a PdfImageHelper instance for working with images in the PDF
        let helper = wasmModule.PdfImageHelper.Create();
        // Get information about the images on the page
        let images = helper.GetImagesInfo(page);

        // Delete the first image on the page
        helper.DeleteImage({imageInfo: images[0]});

        // Define the output file name
        const outputFileName = "DeleteImageFirstApproach_out.pdf";

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
