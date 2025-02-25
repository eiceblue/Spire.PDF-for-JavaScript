<template>
  <span>The following example demonstrates how to compress PDF document.</span>
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
        let inputFileName = 'CompressDocument.pdf';
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);

        // Create a new instance of PdfCompressor with the specified input PDF file path
        let compressor = wasmModule.PdfCompressor.Create({filePath: inputFileName});

        // Set compression options for the compressor
        compressor.Options.ImageCompressionOptions.ResizeImages = true;
        compressor.Options.ImageCompressionOptions.ImageQuality = wasmModule.ImageQuality.Low;

        // Define the output file name
        const outputFileName = "CompressDocumentSecondApproach_result.pdf";

        // Save the document to the specified path
        compressor.CompressToFile(outputFileName);

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
