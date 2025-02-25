<template>
  <span>The following example demonstrates how to expand bookmarks of PDF document.</span>
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
        function ForeachBookmark(collections, expand)
         {
          // Check if the collection is empty, and return if it is
          if (collections.Count == 0)
            return;

          // Iterate through each bookmark in the collection
          for (let i = 0; i < collections.Count; i++) {
            let bookmark = collections.get_Item(i);
            // Recursively call the function to process child bookmarks
            ForeachBookmark(bookmark, expand);

            // Set the ExpandBookmark property of the current bookmark to the specified flag
            bookmark.ExpandBookmark = expand;
          }
        }
        // Load the sample file into the virtual file system (VFS)
        let inputFileName = "Template_Pdf_1.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a new PdfDocument object
        let doc = wasmModule.PdfDocument.Create();

        // Load an existing PDF document from a file.
        doc.LoadFromFile({ fileName: inputFileName });

        // Set true to expand the bookmarks
        ForeachBookmark(doc.Bookmarks, true);

        // Define the output file name
        const outputFileName = "ExpandBookmarks_result.pdf";

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
