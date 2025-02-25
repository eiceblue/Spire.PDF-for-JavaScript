<template>
  <span>The following example demonstrates how to find text in specified rectangle. </span>
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
        let inputFileName = 'FindTextInDefineArea.pdf';
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);

        // Create a pdf instance
        let doc = wasmModule.PdfDocument.Create();

        //Load a pdf file
        doc.LoadFromFile({fileName: inputFileName});

        // Define a rectangle to specify the search area
        let rctg = wasmModule.RectangleF.Create({x: 0, y: 0, width: 300, height: 300});

        //Get the first page
        let pdfPageBase = doc.Pages.get_Item(0);

        // Create a PdfTextFinder object for the first page
        let finder = wasmModule.PdfTextFinder.Create(pdfPageBase);
        finder.Options.Parameter = wasmModule.TextFindParameter.WholeWord;
        finder.Options.Area = rctg;

        //Find text in the rectangle
        let finds = finder.Find("Spire");

        //Highlight the found text

        for (let j = 0; j < finds.length; j++) {
            finds[j].HighLight({color: wasmModule.Color.get_Green()});
        }
        // Define the output file name
        const outputFileName = "FindTextInDefineArea_out.pdf";

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
