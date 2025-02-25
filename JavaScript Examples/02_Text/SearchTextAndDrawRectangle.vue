<template>
  <span>The following example demonstrates how to search text and draw rectangle in PDF document.</span>
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
        doc.LoadFromFile({fileName: inputFileName});

        // Get the first page of the pdf file
        let page = doc.Pages.get_Item(0);

        // Create a PdfTextFinder object for searching text within the first page
        let finder = wasmModule.PdfTextFinder.Create(page);
        finder.Options.Parameter = wasmModule.TextFindParameter.IgnoreCase;

        // Find occurrences of the specified text within the first page
        let finds = finder.Find("Spire.PDF");

        // Iterate through each found text fragment
        for (let i = 0; i < finds.length; i++) {
            let find = finds[i];
            // Draw a rectangle with red pen brush, width
            let pen = wasmModule.PdfPen.Create({brush: wasmModule.PdfBrushes.get_Red(), width: 0.9});
            page.Canvas.DrawRectangle({pen: pen, rectangle: find.Bounds[0]});
        }

        // Define the output file name
        const outputFileName = "SearchTextAndDrawRectangle_out.pdf";

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
