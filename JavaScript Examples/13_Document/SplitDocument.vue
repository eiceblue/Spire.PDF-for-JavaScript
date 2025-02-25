<template>
  <span>The following example demonstrates how to split a PDF document into multiple PDF documents.</span>
  <el-button @click="startProcessing">Start</el-button>
   <a v-if="downloadUrl0" :href="downloadUrl0" :download="downloadName0">
    Click here to download the generated file0
  </a>
  <a v-if="downloadUrl1" :href="downloadUrl1" :download="downloadName1">
    Click here to download the generated file1
  </a>
   <a v-if="downloadUrl2" :href="downloadUrl2" :download="downloadName2">
    Click here to download the generated file2
  </a>
  <a v-if="downloadUrl3" :href="downloadUrl3" :download="downloadName3">
    Click here to download the generated file3
  </a>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    const downloadUrl0 = ref(null);
    const downloadName0 = ref("");

    const downloadUrl1 = ref(null);
    const downloadName1 = ref("");

    const downloadUrl2 = ref(null);
    const downloadName2 = ref("");

    const downloadUrl3 = ref(null);
    const downloadName3 = ref("");

    const startProcessing = async () => {
      wasmModule = window.wasmModule;
      if (wasmModule) {

        // Load the sample file into the virtual file system (VFS)
        let inputFileName = 'SplitDocument.pdf';
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);

        //Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        doc.LoadFromFile({fileName: inputFileName});

        const outFileName = "SplitDocument_result-{0}.pdf";
        
        //Split document
        doc.Split(outFileName)

        for(let count = 0; count < doc.Pages.Count; count++) {
            const outFileName = "SplitDocument_result-"+count+".pdf";
            // Read the saved file and convert to a Blob object
            const modifiedFileArray = wasmModule.FS.readFile(outFileName);
            const modifiedFile = new Blob([modifiedFileArray], { type: "application/pdf" });

            // Download the file
            if (count === 0) {
            downloadUrl0.value = URL.createObjectURL(modifiedFile);
            downloadName0.value = outFileName;
            } 
            else if (count === 1) {
            downloadUrl1.value = URL.createObjectURL(modifiedFile);
            downloadName1.value = outFileName;
            } 
            else if (count === 2) {
            downloadUrl2.value = URL.createObjectURL(modifiedFile);
            downloadName2.value = outFileName;
          }
            else if (count === 3) {
            downloadUrl3.value = URL.createObjectURL(modifiedFile);
            downloadName3.value = outFileName;
          }
        }

      }
    };

    return {
      startProcessing,
      downloadName0,
      downloadUrl0,
      downloadName1,
      downloadUrl1,
      downloadName2,
      downloadUrl2,
      downloadName3,
      downloadUrl3,
    };
  },
};
</script>
