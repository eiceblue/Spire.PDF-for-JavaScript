<template>
  <span>The following example demonstrates how to get zoom factor of pdf document.</span>
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
        let inputFileName = 'GetZoomFactor.pdf';
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);

        //Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        doc.LoadFromFile({fileName: inputFileName});
        let action = wasmModule.PdfGoToAction.Create({destination:doc.AfterOpenAction})
        // Get the zoom factor value from the destination of the action
        let zoomvalue = action.Destination.Zoom
        let result = "The zoom factor of the document is "+ (zoomvalue*100).toFixed(2).toString() +"%."+"\n";

        // Define the output file name
        const outputFileName = "GetZoomFactor_result.txt";

        // Save result file
        wasmModule.FS.writeFile(outputFileName,  result);
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
