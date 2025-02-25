<template>
  <span>The following example demonstrates how to extract text from particular page. </span>
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
        let inputFileName = 'ExtractTextFromParticularPage.pdf';
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);

        // Create a pdf instance
        let doc = wasmModule.PdfDocument.Create();

        //Load a pdf file
        doc.LoadFromFile({fileName: inputFileName});

        // Get the first page
        let page = doc.Pages.get_Item(0);

        // Extract text from page keeping white space
        let options = wasmModule.PdfTextExtractOptions.Create();
        options.IsExtractAllText = true; //false->Extract text from page without keeping white space
        let pdfTextExtractor = wasmModule.PdfTextExtractor.Create(page);
        let text = pdfTextExtractor.ExtractText(options);

        // Define the output file name
        const outputFileName = "ExtractTextFromParticularPage_out.txt";
        wasmModule.FS.writeFile(outputFileName, text);
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
