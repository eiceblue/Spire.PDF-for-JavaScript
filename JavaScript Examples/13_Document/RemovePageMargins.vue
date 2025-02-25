<template>
  <span>The following example demonstrates how to remove the margins of PDF pages.</span>
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
        let inputFileName = 'PDFTemplate-Az.pdf';
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);

        //Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        doc.LoadFromFile({fileName: inputFileName});
        
        //Creates a new pdf document
        let newDoc = wasmModule.PdfDocument.Create();
        //Get page margins of source pdf page
        let margins = doc.PageSettings.Margins
        let top = margins.Left
        let bottom = margins.Bottom
        let left = margins.Left
        let right = margins.Right

        for (let i=0;i<doc.Pages.Count;i++)
        {
            let page = doc.Pages.get_Item(i);

            //Adds a new page to the new document
            let newPage = newDoc.Pages.Add(wasmModule.SizeF.Create(page.Size.Width - left - right, page.Size.Height - top - bottom), wasmModule.PdfMargins.Create(0.0))
            //Draws the content of the source page onto the new document page
            newPage.Canvas.DrawTemplate(page.CreateTemplate(), wasmModule.PointF.Create(-left, -top))
        }

        // Define the output file name
        const outputFileName = "RemovePageMargins_result.pdf";

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
