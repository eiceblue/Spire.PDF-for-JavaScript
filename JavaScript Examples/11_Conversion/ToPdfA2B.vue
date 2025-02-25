<template>
  <span>The following example demonstrates how to convert Pdf to PdfA2B.</span>
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
        let inputFileName = "ToPdfA2B.pdf";
        await wasmModule.FetchFileToVFS(inputFileName,"",`${import.meta.env.BASE_URL}static/data/`);

        // Define the output file name
        const outputFileName = "ToPdfA2B_result.pdf";
  
        //Convert to PDFA file
        let converter = wasmModule.PdfStandardsConverter.Create({filePath: inputFileName});
        //also supports ToPdfA1B ToPdfA1A ToPdfA2A ToPdfA3A ToPdfA3B ToPdfX1A2001
        converter.ToPdfA2B({filePath: outputFileName});

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
