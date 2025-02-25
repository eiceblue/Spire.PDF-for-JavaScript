<template>
  <span>The following example demonstrates how to convert particular PDF page to PNG.</span>
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
 
        let inputFileName  = "PageToImage.pdf";
        await wasmModule.FetchFileToVFS(inputFileName , '', `${import.meta.env.BASE_URL}static/data/`);

        // Create a pdf instance
        let doc = wasmModule.PdfDocument.Create();

        //Load the PDF document from the specified file
        doc.LoadFromFile({fileName: inputFileName});

        //Convert a particular page to emf
        //Set page index and image name
        let pageIndex = 0;
        let pdfstream = doc.SaveAsImage({pageIndex: pageIndex});

        // Define the output file name
        const outputFileName = "PageToPNG_out.pdf";

        pdfstream.Save(outputFileName);
        pdfstream.Dispose();
        doc.Dispose();

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
