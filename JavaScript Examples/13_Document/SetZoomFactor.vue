<template>
  <span>The following example demonstrates how to set zoom factor.</span>
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
        let inputFileName = 'SetZoomFactor.pdf';
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);

        //Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        doc.LoadFromFile({fileName: inputFileName});
        
        //Get the first page
        let page = doc.Pages.get_Item(0);
        //Set pdf destination
        let dest = wasmModule.PdfDestination.Create({page:page});
        dest.Mode = wasmModule.PdfDestinationMode.Location;
        dest.Location = wasmModule.PointF.Create(-40.0, -40.0);
        dest.Zoom = 0.6;
        //Set action
        let gotoAction = wasmModule.PdfGoToAction.Create({destination:dest});
        doc.AfterOpenAction = gotoAction;

        // Define the output file name
        const outputFileName = "SetZoomFactor_result.pdf";

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
