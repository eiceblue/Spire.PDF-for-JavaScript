<template>
  <span>The following example shows how to replace text in the page of pdf document.</span>
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
        let inputFileName = 'SearchReplaceTemplate.pdf';
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);

        // Create a pdf instance
        let doc = wasmModule.PdfDocument.Create();

        //Load a pdf file
        doc.LoadFromFile({fileName: inputFileName});

        // Get the first page of the pdf file
        let page = doc.Pages.get_Item(0);

        // Create a PdfTextReplacer using the first page
        let replacer = wasmModule.PdfTextReplacer.Create(page);

        // Replace all occurrences of "Spire.PDF" with "E-iceblue" in this page
        replacer.ReplaceAllText({oldText: "Spire.PDF", newText: "E-iceblue"});

        // Replace the first occurrence of "Adobe Acrobat" with "PDF editors"
        replacer.ReplaceText("Adobe Acrobat", "PDF editors");

        // Define the output file name
        const outputFileName = "ReplaceTextSecond_out.pdf";

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
