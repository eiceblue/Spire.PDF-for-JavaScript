<template>
  <span>The following example demonstrates how to get the document properties of PDF document.</span>
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
        let inputFileName = 'PDFTemplate-Az.pdf';
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);

        //Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        doc.LoadFromFile({fileName: inputFileName});
        
        let docInfo = doc.DocumentInformation;
        let outContent = "";
        outContent+="Author:"+docInfo.Author+"\n";
        outContent+="Creation Date: "+ docInfo.CreationDate.ToString()+"\n";
        outContent+="Keywords: "+ docInfo.Keywords+"\n";
        outContent+="Modify Date: "+ docInfo.ModificationDate.ToString()+"\n";
        outContent+="Subject: "+ docInfo.Subject+"\n";
        outContent+="Title: "+ docInfo.Title+"\n";

        // Define the output file name
        const outputFileName = "GetDocumentProperties_result.txt";

        // Save result file
        wasmModule.FS.writeFile(outputFileName,  outContent);
        doc.Close();

        // Read the saved file and convert to a Blob object
        const modifiedFileArray = wasmModule.FS.readFile(outputFileName);
        const modifiedFile = new Blob([modifiedFileArray], { type: "text/plain" });

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
