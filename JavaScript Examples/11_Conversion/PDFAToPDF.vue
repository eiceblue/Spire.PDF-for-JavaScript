<template>
  <span>The following example demonstrates how to convert PDF/A to PDF document.</span>
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
        let inputFileName = "SamplePDFA.pdf";
        await wasmModule.FetchFileToVFS(inputFileName,"",`${import.meta.env.BASE_URL}static/data/`);
        //Create a document
        let doc = wasmModule.PdfDocument.Create();
	      doc.LoadFromFile(inputFileName);

        // Create a new PDF document to draw content on a new file
        let newDoc = wasmModule.PdfNewDocument.Create();
        newDoc.CompressionLevel = wasmModule.PdfCompressionLevel.None;

        // Iterate through each page in the original document
        //foreach (PdfPageBase page in doc.Pages)
        for (let i = 0; i < doc.Pages.Count; i++) {
            let page = doc.Pages.get_Item(i);
            // Get the size of the current page
            let size = page.Size;

            // Add a new page to the new document with the same size and no margins
            let p = newDoc.Pages.Add({size: size, margins: wasmModule.PdfMargins.Create()});

            // Draw the contents of the original page onto the new page page
            let template = page.CreateTemplate();
            let layoutWidget = new wasmModule.PdfLayoutWidget(template.H);
            layoutWidget.Draw({page: p, x: 0, y: 0});
            //page.CreateTemplate().Draw({page: p, x: 0, y: 0});
        }

        // Define the output file name
        const outputFileName = "PDFAToPdf_result.pdf";

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
