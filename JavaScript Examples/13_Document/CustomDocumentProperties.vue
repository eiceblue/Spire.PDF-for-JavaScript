<template>
  <span>The following example demonstrates how to add custom document properties.</span>
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
        let inputFileName = 'SampleB_2.pdf';
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);

        //Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        // Load an existing PDF document from a file.
        doc.LoadFromFile({fileName: inputFileName});
        //Custom document properties
        doc.DocumentInformation.SetCustomProperty("Company", "E-iceblue")
        doc.DocumentInformation.SetCustomProperty("Component", "Spire.PDF for .NET")
        doc.DocumentInformation.SetCustomProperty("Name", "Daisy")
        doc.DocumentInformation.SetCustomProperty("Team", "SalesTeam")
        // Define the output file name
        const outputFileName = "CustomDocumentProperties_result.pdf";

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
