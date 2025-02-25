<template>
  <span>The following example demonstrates how to replace image in PDF document. </span>
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

        let inputFileName  = "ReplaceImage.pdf";
        await wasmModule.FetchFileToVFS(inputFileName , '', `${import.meta.env.BASE_URL}static/data/`);
 
        const inputImageName = "ChartImage.png";
        await wasmModule.FetchFileToVFS(inputImageName , '', `${import.meta.env.BASE_URL}static/data/`);

        // Create a pdf instance
        let doc = wasmModule.PdfDocument.Create();
        
        //Load a pdf file
        doc.LoadFromFile({fileName: inputFileName});
        // Get the first page
        let page = doc.Pages.get_Item(0);

        //Create PdfImageHelper object to get Image from page
        let helper = wasmModule.PdfImageHelper.Create();
        let images = helper.GetImagesInfo(page);

        let newImage = wasmModule.PdfImage.FromFile(inputImageName);

        //Replace the first image on the page. imageInfo, newImage
        helper.ReplaceImage(images[0], newImage);

        // Define the output file name
        const outputFileName = "ReplaceImage_out.pdf";

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
