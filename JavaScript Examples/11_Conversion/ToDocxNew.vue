<template>
  <span>The following example demonstrates how to convert PDF document to docx with flow method.</span>
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
        let inputFileName = "ToDocx.pdf";
        await wasmModule.FetchFileToVFS(inputFileName,"",`${import.meta.env.BASE_URL}static/data/`);
       
        //Create a PdfToWordConverter
        let converter = wasmModule.PdfToWordConverter.Create({filePath: inputFileName});

        // Define the output file name
        const outputFileName = "ToDocxNew_result.docx";
        converter.SaveToDocx({outfile: outputFileName});

        // Read the saved file and convert to a Blob object
        const modifiedFileArray = wasmModule.FS.readFile(outputFileName);
        const modifiedFile = new Blob([modifiedFileArray], { type: "application/vnd.openxmlformats-officedocument.wordprocessingml.document" });

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
