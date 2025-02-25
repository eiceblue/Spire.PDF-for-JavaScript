<template>
  <span>The following example demonstrates how to extract text and images of PDF document.</span>
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
        let inputFileName = "Extraction.pdf";
        await wasmModule.FetchFileToVFS(inputFileName,"",`${import.meta.env.BASE_URL}static/data/`);
        //Create a PPT document
        let doc = wasmModule.PdfDocument.Create();
	      doc.LoadFromFile(inputFileName);

        //Create a buffer to store the extracted text
        let sbuffer = "";

        //Iterate through each page in the document
        for (let i = 0; i < doc.Pages.Count; i++) {
            let page = doc.Pages.get_Item(i);

            //Create a PdfTextExtractor object for the page
            let pdfTextExtractor = wasmModule.PdfTextExtractor.Create(page)

            //Create PdfTextExtractOptions object
            let pdfTextExtractOptions = wasmModule.PdfTextExtractOptions.Create();
            pdfTextExtractOptions.IsExtractAllText = true;

            //Extract the text from the page
            sbuffer += pdfTextExtractor.ExtractText(pdfTextExtractOptions) + "\n";
        }

        // Define the output file name
        const outputFileName = "Extraction_out.txt";

        wasmModule.FS.writeFile(outputFileName, sbuffer);
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
