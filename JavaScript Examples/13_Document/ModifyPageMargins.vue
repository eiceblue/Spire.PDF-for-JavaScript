<template>
  <span>The following example demonstrates how to modify page margins of PDF document.</span>
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
        let inputFileName = 'Multipage.pdf';
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);

        //Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        doc.LoadFromFile({fileName: inputFileName});

        //Creates a new pdf document
        let newDoc = wasmModule.PdfDocument.Create();
        
        //Defines the page margins of the new document
        let top = 50.0
        let bottom = 50.0
        let left = 50.0
        let right = 50.0
        for (let i=0;i<doc.Pages.Count;i++)
        {
            let page = doc.Pages.get_Item(i);
            //Adds a new page to the new document and set the page size to be the same as the source document
            let newPage = newDoc.Pages.Add(page.Size, wasmModule.PdfMargins.Create(0.0));
            newPage.Canvas.ScaleTransform((page.ActualSize.Width - left - right) / page.ActualSize.Width, (page.ActualSize.Height - top - bottom) / page.ActualSize.Height);
            newPage.Canvas.DrawTemplate(page.CreateTemplate(), wasmModule.PointF.Create(left, top));
        }

        // Define the output file name
        const outputFileName = "ModifyPageMargins_result.pdf";

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
