<template>
  <span>The following example demonstrates how to get all bookmarks of PDF document.</span>
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

        function GetBookmarks(bookmarks, result) {
          //Declare a new StringBuilder content
          let content = "";

          //Get Pdf bookmarks information.
          if (bookmarks.Count > 0) {
            content = content + "Pdf bookmarks:" + "\r\n";
            //foreach (PdfBookmark parentBookmark in bookmarks)
            for (let i = 0; i < bookmarks.Count; i++) {
              let parentBookmark = bookmarks.get_Item(i);
              //Get the title.
              content = content + parentBookmark.Title + "\r\n";
              //Get the text style.
              let textStyle = parentBookmark.DisplayStyle.toString();
              content = content + textStyle + "\r\n";
              let childContent = GetChildBookmark(parentBookmark);
              content = content + childContent;
            }
          }
          //Save to file.
          wasmModule.FS.writeFile(result, content.toString());
        }


        function GetChildBookmark(parentBookmark) {
          let childContent = "";
          if (parentBookmark.Count > 0) {
            for (let i = 0; i < parentBookmark.Count; i++) {
              let childBookmark = parentBookmark.get_Item(i);
              //Get the title.
              childContent = childContent + childBookmark.Title + "\r\n";
              //Get the text style.
              let textStyle = childBookmark.DisplayStyle.toString();
              childContent = childContent + textStyle + "\r\n";
              childContent = childContent + GetChildBookmark(childBookmark);
            }
          }
          return childContent;
        }

        // Load the sample file into the virtual file system (VFS)
        let inputFileName = "Template_Pdf_1.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a new PdfDocument object
        let doc = wasmModule.PdfDocument.Create();

        // Load an existing PDF document from a file.
        doc.LoadFromFile({ fileName: inputFileName });

        //Get bookmarks collection of the Pdf file.
        let bookmarks = doc.Bookmarks;

        // Define the output file name
        const outputFileName = "GetPdfBookmarks_result.txt";

        //Get Pdf Bookmarks.
        GetBookmarks(bookmarks, outputFileName);

        //Close
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
