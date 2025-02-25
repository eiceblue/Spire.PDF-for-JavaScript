<template>
  <span>The following example demonstrates how to get the page number of PDF bookmark.</span>
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
        let inputFileName = "Template_Pdf_1.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a new PdfDocument object
        let doc = wasmModule.PdfDocument.Create();

        // Load an existing PDF document from a file.
        doc.LoadFromFile({ fileName: inputFileName });

        //Get bookmarks collections of the PDF file.
        let bookmarks = doc.Bookmarks;

        //Get the page number of the first bookmark.
        let bookmark = bookmarks.get_Item(0);
        let pageNumber = doc.Pages.IndexOf(bookmark.Destination.Page) + 1;

        //Save to file.
        let showPageNumber = pageNumber.toString();

        let content = "The page number of the first bookmark is: " + showPageNumber

        // Define the output file name
        const outputFileName = "GetPdfBookmarkPageNumber_result.txt";

        // Save the document to the specified path
        wasmModule.FS.writeFile(outputFileName, content.toString());
        doc.Close();

        // Read the saved file and convert to a Blob object
        const modifiedFileArray = wasmModule.FS.readFile(outputFileName);
        const modifiedFile = new Blob([modifiedFileArray], {type: "text/plain"});

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
