<template>
  <span>The following example demonstrates how to set PDF document properties and PDF file information.</span>
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
        let inputFileName = 'Properties.pdf';
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);

        //Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        doc.LoadFromFile({fileName: inputFileName});
        
        //Set document info
        doc.DocumentInformation.Author = "E-iceblue"
        doc.DocumentInformation.Creator = "E-iceblue"
        doc.DocumentInformation.Keywords = "pdf, demo, document information"
        doc.DocumentInformation.Producer = "Spire.Pdf"
        doc.DocumentInformation.Subject = "Demo of Spire.Pdf"
        doc.DocumentInformation.Title = "Document Information"
        
        //File info
        doc.FileInfo.CrossReferenceType = wasmModule.PdfCrossReferenceType.CrossReferenceStream
        doc.FileInfo.IncrementalUpdate = false

        // Define the output file name
        const outputFileName = "Properties_result.pdf";

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
