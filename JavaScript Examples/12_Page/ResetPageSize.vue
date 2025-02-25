<template>
  <span>The following example demonstrates how to reset page size of PDF document.</span>
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
        let inputFileName = "ResetPageSize.pdf";
        await wasmModule.FetchFileToVFS(inputFileName,"",`${import.meta.env.BASE_URL}static/data/`);
        //Create a PPT document
        let doc = wasmModule.PdfDocument.Create();
      	doc.LoadFromFile(inputFileName);

        //Set the margins
        let margins = wasmModule.PdfMargins.Create();
        //Create a new pdf document
        let newDoc = wasmModule.PdfDocument.Create();
        let scale = 0.8;
        for (let i=0;i<doc.Pages.Count;i++) {
            let page = doc.Pages.get_Item(i);
            //Use same scale to set width and height
            let width = page.Size.Width * scale;
            let height = page.Size.Height * scale;
            //Add new page with expected width and height
            let newPage = newDoc.Pages.Add({size:wasmModule.SizeF.Create({width:width,height: height}),margins: margins});
            newPage.Canvas.ScaleTransform(scale, scale);
            //Copy content of original page into new page
            newPage.Canvas.DrawTemplate({template:page.CreateTemplate(), location:wasmModule.PointF.Create()});
        }

        // Define the output file name
        const outputFileName = "ResetPageSize_out.pdf";

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
