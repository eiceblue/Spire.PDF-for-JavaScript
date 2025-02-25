<template>
  <span>The following example demonstrates how to set inherit zoom property for bookmarks.</span>
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
        function SetBookmarkAction(bookmark) {
          // Get the destination of the bookmark
          let dest = bookmark.Destination;

          // Set the destination mode to "Location" and the zoom level to 0
          dest.Mode = wasmModule.PdfDestinationMode.Location;
          dest.Zoom = 0;

          // Iterate through each child bookmark recursively
          for (let i = 0; i < bookmark.Count; i++) {
            let childbookmark = bookmark.get_Item(i);
            SetBookmarkAction(childbookmark);
          }
        }
        // Load the sample file into the virtual file system (VFS)
        let inputFileName = "SetInheritZoomForBookmarks.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a new PdfDocument object
        let doc = wasmModule.PdfDocument.Create();
        // Load an existing PDF document from a file.
        doc.LoadFromFile({ fileName: inputFileName });

        // Get the bookmarks collection of the PDF file
        let bookmarks = doc.Bookmarks;

        // Iterate through each bookmark in the collection
        for (let i = 0; i < bookmarks.Count; i++) {
          // Set inherit zoom for the current bookmark
          let bookmark = bookmarks.get_Item(i);
          SetBookmarkAction(bookmark);
        }

        // Define the output file name
        const outputFileName ="SetInheritZoomForBookmarks_result.pdf";

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
