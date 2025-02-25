<template>
  <span>The following example demonstrates how to find and highlight text in PDF document.</span>
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
        let inputFileName = 'FindAndHighlightText.pdf';
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);

        // Create a pdf instance
        let doc = wasmModule.PdfDocument.Create();

        //Load a pdf file
        doc.LoadFromFile({fileName: inputFileName});

        for (let i = 0; i < doc.Pages.Count; i++) {
            let page = doc.Pages.get_Item(i);
            // Create a PdfTextFinder object for the current page
            let finder = wasmModule.PdfTextFinder.Create(page);
            finder.Options.Parameter = wasmModule.TextFindParameter.WholeWord;
            // Find the occurrences of the specified text
            let finds = finder.Find("science");
            //let finds = finder.FindAllText();

            //Highlight the found text
            for (let j = 0; j < finds.length; j++) {
                finds[j].HighLight();
            }
        }

        // Define the output file name
        const outputFileName = "FindAndHighlightText_out.pdf";

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
