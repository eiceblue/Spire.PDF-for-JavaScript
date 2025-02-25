<template>
  <span>The following example demonstrates how to convert PDF document to images.</span>
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
        let inputFileName = "ToImage.pdf";
        await wasmModule.FetchFileToVFS(inputFileName,"",`${import.meta.env.BASE_URL}static/data/`);
        //Create a document
        let doc = wasmModule.PdfDocument.Create();
        doc.LoadFromFile(inputFileName);
        let outFileName ="";
        //Save to images
        for (let i=0;i<doc.Pages.Count;i++) {
              outFileName = `ToImage-img-${i}.png`;            
              let pdfstream = doc.SaveAsImage({pageIndex: i});
              pdfstream.Save(outFileName);              
          }

        // Read the saved file and convert to a Blob object
        const modifiedFileArray = wasmModule.FS.readFile(outFileName);
        const modifiedFile = new Blob([modifiedFileArray], { type: "application/pdf" });

        // Download the file
        downloadName.value = outFileName;
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
