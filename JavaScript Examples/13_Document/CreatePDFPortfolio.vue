<template>
  <span>The following example demonstrates how to create PDF portfolio.</span>
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
        let inputFileName = 'Sample.pdf';
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);

        //Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

       // Load an existing PDF document from a file.
       doc.LoadFromFile({fileName: inputFileName});
       
       let files = ["SampleB_2.pdf","SampleB_3.pdf"]
       // Iterate through the files array and add each file to the document's collection
       for (let i = 0; i < files.length; i++) {

        await wasmModule.FetchFileToVFS(files[i], '', `${import.meta.env.BASE_URL}static/data/`);
        // Add the current file to the subfolder
        doc.Collection.Folders.AddFile({filePath: files[i]});
      }

        // Define the output file name
        const outputFileName = "CreatePDFPortfolio_result.pdf";

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
