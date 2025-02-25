<template>
  <span>The following example demonstrates how to convert all pages of PDF to PNG. </span>
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
        let inputFileName  = "ConvertAllPagesToPNG.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);

        // Create a pdf instance
        let doc = wasmModule.PdfDocument.Create();
        doc.LoadFromFile({fileName: inputFileName});
        
        // Iterate through each page
        for (let i = 0; i < doc.Pages.Count; i++) {
            const outputFileName = "ConvertAllPagesToPNG_" + i + ".png";
            let stream = doc.SaveAsImage({pageIndex: i});
            stream.Save(outputFileName);
            stream.Dispose();
            
            // Read the saved file and convert to a Blob object
            const modifiedFileArray = wasmModule.FS.readFile(outputFileName);
            const modifiedFile = new Blob([modifiedFileArray],  {type: 'image/png'});

            // Download the file
            downloadName.value = outputFileName;
            downloadUrl.value = URL.createObjectURL(modifiedFile);
        }
      
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
