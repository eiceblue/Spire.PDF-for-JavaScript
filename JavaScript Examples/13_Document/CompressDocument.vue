<template>
  <span>The following example demonstrates how to compress a PDF document.</span>
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

        let compressor = wasmModule.PdfCompressor.Create({filePath: inputFileName});
        // Enable compression of PDF content
        compressor.Options.CompressContents = true;
        // Configure text compression options
        compressor.Options.TextCompressionOptions.UnembedFonts = true;
        compressor.Options.TextCompressionOptions.CompressFonts = true;
        // Configure image compression options
        compressor.Options.ImageCompressionOptions.ResizeImages = true;
        compressor.Options.ImageCompressionOptions.CompressImage = true;
        compressor.Options.ImageCompressionOptions.ImageQuality = wasmModule.ImageQuality.High;

        // Define the output file name
        const outputFileName = "CompressDocument_result.pdf";

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
