<template>
  <span>The following example demonstrates how to set PDF document view preference.</span>
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
        // Load the ARIALUNI.TTF font file into the virtual file system (VFS)
        await wasmModule.FetchFileToVFS("ARIALUNI.TTF", "/Library/Fonts/", `${import.meta.env.BASE_URL}static/font/`);

        // Load the sample file into the virtual file system (VFS)
        let inputFileName = 'ViewerPreference.pdf';
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);

        //Create a PDF document
        let doc = wasmModule.PdfDocument.Create();
        doc.LoadFromFile({fileName: inputFileName});

        //Set view reference
        // Center the window of the PDF viewer
        doc.ViewerPreferences.CenterWindow = true
        // Do not display the title of the PDF document in the viewer
        doc.ViewerPreferences.DisplayTitle = false
        // Do not fit the content of the PDF document to the window of the viewer
        doc.ViewerPreferences.FitWindow = false
        // Hide the menu bar of the PDF viewer
        doc.ViewerPreferences.HideMenubar = true
        // Hide the toolbar of the PDF viewer
        doc.ViewerPreferences.HideToolbar = true
        // Display the PDF document as a single page
        doc.ViewerPreferences.PageLayout = wasmModule.PdfPageLayout.SinglePage

        // Define the output file name
        const outputFileName = "ViewerPreference_result.pdf";

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
