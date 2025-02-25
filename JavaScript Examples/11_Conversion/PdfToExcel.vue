<template>
  <span>The following example demonstrates how to convert PDF document to Excel document.</span>
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
        let inputFileName = "PdfToExcel.pdf";
        await wasmModule.FetchFileToVFS(inputFileName,"",`${import.meta.env.BASE_URL}static/data/`);
        //Create a document
        let doc = wasmModule.PdfDocument.Create();
      	doc.LoadFromFile(inputFileName);

        // Define the output file name
        const outputFileName = "PdfToExcel_out.xlsx";

        // Save the document to the specified path
        doc.SaveToFile({fileName: outputFileName,fileFormat: wasmModule.FileFormat.XLSX});
        doc.Close();

        // Read the saved file and convert to a Blob object
        const modifiedFileArray = wasmModule.FS.readFile(outputFileName);
        const modifiedFile = new Blob([modifiedFileArray], { type: "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet" });

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
