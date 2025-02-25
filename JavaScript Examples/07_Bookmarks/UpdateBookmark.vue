<template>
  <span>The following example demonstrates how to update bookmarks of PDF document.</span>
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
        // Function to edit the child bookmarks of a parent bookmark recursively
        function EditChildBookmark(parentBookmark) {
          // Iterate through each child bookmark of the parent bookmark
          for (let i = 0; i < parentBookmark.Count; i++) {
            let childBookmark = parentBookmark.get_Item(i);
            // Set the color of the child bookmark to Blue
            childBookmark.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Blue() });

            // Set the outline text style of the child bookmark to Regular
            childBookmark.DisplayStyle = wasmModule.PdfTextStyle.Regular;

            // Recursively call EditChild2Bookmark to edit the child's child bookmarks
            EditChild2Bookmark(childBookmark);
          }
        }

        // Function to edit the child's child bookmarks recursively
        function EditChild2Bookmark(childBookmark) {
          // Iterate through each child bookmark of the child bookmark
          for (let i = 0; i < childBookmark.Count; i++) {
            let child2Bookmark = childBookmark.get_Item(i);
            // Set the color of the child's child bookmark to LightSalmon
            child2Bookmark.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightSalmon() });

            // Set the outline text style of the child's child bookmark to Italic
            child2Bookmark.DisplayStyle = wasmModule.PdfTextStyle.Italic;
          }
        }

        // Load the sample file into the virtual file system (VFS)
        let inputFileName = "UpdateBookmark.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a new PdfDocument object
        let doc = wasmModule.PdfDocument.Create();

        // Load an existing PDF document from a file.
        doc.LoadFromFile({ fileName: inputFileName });

        // Get the first bookmark from the document
        let bookmark = doc.Bookmarks.get_Item(0);

        // Change the title of the bookmark
        bookmark.Title = "Modified BookMark";

        // Set the color of the bookmark to Black
        bookmark.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Black() });

        // Set the outline text style of the bookmark to Bold
        bookmark.DisplayStyle = wasmModule.PdfTextStyle.Bold;

        // Edit the child bookmarks of the parent bookmark
        EditChildBookmark(bookmark);

        // Define the output file name
        const outputFileName ="UpdateBookmark_result.pdf";

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
