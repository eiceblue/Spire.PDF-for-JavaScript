<template>
  <span>The following example demonstrates how to split a PDF page into multipage. </span>
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
        let inputFileName = 'SampleB_2.pdf';
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);

        //Load the document from disk
        let doc = wasmModule.PdfDocument.Create();
        doc.LoadFromFile({fileName: inputFileName});

        //Get the first page
        let page = doc.Pages.get_Item(0);
        //Create a new Pdf
        let newPdf = wasmModule.PdfDocument.Create();
        //Remove all the margins
        newPdf.PageSettings.Margins.All = 0.0;
        //Set the page size of new Pdf
        newPdf.PageSettings.Width = page.Size.Width;
        newPdf.PageSettings.Height = page.Size.Height / 2;
        //Add a new page
        let newPage = newPdf.Pages.Add();
        // Specify the text layout settings for drawing the page content
        let format = wasmModule.PdfTextLayout.Create();
        format.Break = wasmModule.PdfLayoutBreakType.FitPage;
        format.Layout = wasmModule.PdfLayoutType.Paginate;
        //Draw the page in the new page
        page.CreateTemplate().Draw(newPage, wasmModule.PointF.Create({x:0,y:0}), format);

        // Define the output file name
        const outputFileName = "SplitAPageIntoMultipage_result.pdf";

        // Save the document to the specified path
        newPdf.SaveToFile({fileName: outputFileName});
        newPdf.Close();

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
