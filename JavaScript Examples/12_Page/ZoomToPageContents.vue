<template>
  <span>The following example demonstrates how to zoom page content of PDF document.</span>
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
        let inputFileName = 'PDFTemplate_N.pdf';
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);

        //Load the document from disk
        let doc = wasmModule.PdfDocument.Create();
        doc.LoadFromFile({fileName: inputFileName});

        //Create a newDoc
        let newDoc = wasmModule.PdfDocument.Create();
        for (let i=0;i<doc.Pages.Count;i++) {
            let page = doc.Pages.get_Item(i);

            //Add new page with 'A3' size
            let newPage = newDoc.Pages.Add(wasmModule.PdfPageSize.A3(), wasmModule.PdfMargins.Create(0.0,0.0))
            //Zoom content to the new page
            newPage.Canvas.ScaleTransform(newPage.ActualSize.Width / page.ActualSize.Width, (newPage.ActualSize.Height / page.ActualSize.Height))
            //Draw the page to new page
            newPage.Canvas.DrawTemplate(page.CreateTemplate(), wasmModule.PointF.Create(0.0, 0.0))
      }

        // Define the output file name
        const outputFileName = "ZoomToPageContents_result.pdf";

        // Save the document to the specified path
        newDoc.SaveToFile({fileName: outputFileName});
        newDoc.Close();

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
