<template>
  <span>The following example demonstrates how to extract text from specific area.</span>
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
        let inputFileName = 'ExtractTextFromSpecificArea.pdf';
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);

        // Create a pdf instance
        let doc = wasmModule.PdfDocument.Create();

        //Load a pdf file
        doc.LoadFromFile({fileName: inputFileName});

        // Get the first page
        let page = doc.Pages.get_Item(0);

        // Define options for text extraction
        let options = wasmModule.PdfTextExtractOptions.Create();
        options.ExtractArea = wasmModule.RectangleF.Create({x:80, y:180, width:500,height: 200});

        // Create a PdfTextExtractor object and extract text using the specified options
        let pdfTextExtractor = wasmModule.PdfTextExtractor.Create(page);
        let text = pdfTextExtractor.ExtractText(options);

        // Define the output file name
        const outputFileName = "ExtractTextFromSpecificArea_out.txt";
        wasmModule.FS.writeFile(outputFileName, text);
        doc.Close();

        // Read the saved file and convert to a Blob object
        const modifiedFileArray = wasmModule.FS.readFile(outputFileName);
        const modifiedFile = new Blob([modifiedFileArray], { type: "text/plain"});

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
