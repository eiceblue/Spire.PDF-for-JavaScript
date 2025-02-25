<template>
  <span>The following example demonstrates how to set tab order in PDF.</span>
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
        let inputFileName = 'SetTabOrder.pdf';
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);

        // Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        doc.LoadFromFile({fileName: inputFileName});

        // Set using document structure
        doc.FileInfo.IncrementalUpdate = false;
        let page = doc.Pages.get_Item(0);
        // Set the tab order of the page using the structure method
        page.SetTabOrder(wasmModule.TabOrder.Structure); 

        // Define the output file name
        const outputFileName = "SetTabOrder_result.pdf";

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
