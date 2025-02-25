<template>
  <span>The following example demonstrates how to expand specific bookmarks of PDF document.</span>
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
        let inputFileName = "ExpandSpecificBookmarks.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a new PdfDocument object
        let doc = wasmModule.PdfDocument.Create();

        // Load an existing PDF document from a file.
        doc.LoadFromFile({ fileName: inputFileName });

        //Set BookMarkExpandOrCollapse as "true" for the first bookmarks and "false" for the first level of the second bookmarks
        doc.Bookmarks.get_Item(0).ExpandBookmark = true;

        let collections = doc.Bookmarks.get_Item(1);
        let collections1 = collections.get_Item(0);
        collections1.ExpandBookmark = false;

        // Define the output file name
        const outputFileName = "ExpandSpecificBookmarks_result.pdf";

        // Save the document to the specified path
        doc.SaveToFile({ fileName: outputFileName });
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
