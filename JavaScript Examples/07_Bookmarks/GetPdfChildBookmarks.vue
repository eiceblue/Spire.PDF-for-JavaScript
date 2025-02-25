<template>
  <span>The following example demonstrates how to get child bookmarks of PDF document.</span>
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
        function GetChildBookmarks(bookmarks, result) {
          //Declare a new StringBuilder content
          let content = "";
          //Get Pdf child Bookmarks information.
          for (let i = 0; i < bookmarks.Count; i++) {
            let parentBookmark = bookmarks.get_Item(i);

            if (parentBookmark.Count > 0) {
              content = content + "Child Bookmarks:" + "\r\n";
              for (let j = 0; j < parentBookmark.Count; j++) {
                let childBookmark = parentBookmark.get_Item(j);
                //Get the title
                content = content + childBookmark.Title + "\r\n";
                //Get the color.
                let color = childBookmark.Color.R + "," + childBookmark.Color.G + "," + childBookmark.Color.B;
                content = content + "color: "+color + "\r\n";
                //Get the text style.
                let textStyle = "";
                if (childBookmark.DisplayStyle == spirepdf.PdfTextStyle.Bold) {
                  textStyle = "Bold" + "\r\n";
                } else
                  if (childBookmark.DisplayStyle == spirepdf.PdfTextStyle.Italic) {
                    textStyle = "Italic" + "\r\n";
                  }
                if (childBookmark.DisplayStyle == spirepdf.PdfTextStyle.Regular) {
                  textStyle = "Regular" + "\r\n";
                }

                content = content + textStyle;
              }
            }
          }
          wasmModule.FS.writeFile(result, content.toString());
        }
        
        // Load the sample file into the virtual file system (VFS)
        let inputFileName = "Template_Pdf_1.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a new PdfDocument object
        let doc = wasmModule.PdfDocument.Create();

        // Load an existing PDF document from a file.
        doc.LoadFromFile({ fileName: inputFileName });

        //Get bookmarks collections of the PDF file.
        let bookmarks = doc.Bookmarks;

        // Define the output file name
        const outputFileName = "GetPdfChildBookmarks_result.txt";

        //Get Pdf child Bookmarks.
        GetChildBookmarks(bookmarks, outputFileName);

        // Close
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
