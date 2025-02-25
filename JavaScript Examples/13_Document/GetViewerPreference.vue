<template>
  <span>The following example demonstrates how to get viewer preference of PDF document.</span>
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
        let outContent = "";
        let viewer = doc.ViewerPreferences
        outContent+="Whether the documents window position is in the center: "+"\n";
        outContent+="CenterWindow: " + viewer.CenterWindow.toString()+"\n";
        outContent+="Document displaying mode, i.e. show thumbnails, full-screen, show attachment panel: "+"\n";
        outContent+="PageMode: " + viewer.PageMode.toString()+"\n";
        outContent+="The page layout, i.e. single page, one column: "+"\n";
        outContent+="PageLayout: " + viewer.PageLayout.toString()+"\n";
        outContent+="Whether window's title bar should display document title: "+"\n";
        outContent+="DisplayTitle: " + viewer.DisplayTitle.toString()+"\n";
        outContent+="Whether to resize the document's window to fit the size of the firstdisplayed page: "+"\n";
        outContent+="FitWindow: " + viewer.FitWindow.toString()+"\n";
        outContent+="Whether to hide menu bar of the viewer application: "+"\n";
        outContent+="HideMenubar: " + viewer.HideMenubar.toString()+"\n";
        outContent+="Whether to hide tool bar of the viewer application: "+"\n";
        outContent+="HideToolbar: " + viewer.HideToolbar.toString()+"\n";
        outContent+="Whether to hide UI elements like scroll bars and leave only the page contents displayed: "+"\n";
        outContent+="HideWindowUI: " + viewer.HideWindowUI.toString()+"\n";

        // Define the output file name
        const outputFileName = "GetViewerPreference_result.txt";

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
