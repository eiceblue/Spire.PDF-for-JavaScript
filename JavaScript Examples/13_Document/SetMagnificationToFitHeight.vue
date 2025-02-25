<template>
  <span>The following example demonstrates how to set magnification of Pdf to fit height.</span>
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
        let inputFileName = 'SampleB_1.pdf';
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);

        //Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        doc.LoadFromFile({fileName: inputFileName});
        
        // Get the first page
        let page = doc.Pages.get_Item(0);

        // Create a PdfDestination with specific page, location
        let dest = wasmModule.PdfDestination.Create({page: page, location: wasmModule.PointF.Create({x: -40, y: -40})});

        // Set the Magnification to Fit-Height
        dest.Mode = wasmModule.PdfDestinationMode.FitV;

        //Create GoToAction with dest
        let gotoaction = wasmModule.PdfGoToAction.Create({destination: dest});

        // Set open action
        doc.AfterOpenAction = gotoaction;
        doc.ViewerPreferences.PageMode = wasmModule.PdfPageMode.UseOutlines;

        // Define the output file name
        const outputFileName = "SetMagnificationToFitHeight_result.pdf";

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
