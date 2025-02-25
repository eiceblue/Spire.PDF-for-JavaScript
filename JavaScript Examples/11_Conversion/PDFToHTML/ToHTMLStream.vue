<template>
  <span>The following example demonstrates how to convert PDF document to HTML stream.</span>
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
        let inputFileName = "SampleB_1.pdf";
        await wasmModule.FetchFileToVFS(inputFileName,"",`${import.meta.env.BASE_URL}static/data/`);
        //Create a document
        let doc = wasmModule.PdfDocument.Create();
      	doc.LoadFromFile(inputFileName);

        let outputName = "ToHtmlStream_result.html";

        // Create a new memory stream
        let ms = wasmModule.Stream.CreateByFile(outputName);

        // Save the PDF document to an HTML stream
        doc.SaveToStream({stream: ms, fileformat: wasmModule.FileFormat.HTML});

        // Write the content of the memory stream to an HTML file
        wasmModule.FS.writeFile(outputName, ms.ToArray());
        ms.Close();
        doc.Close();

        // Read the saved file and convert to a Blob object
        const modifiedFileArray = wasmModule.FS.readFile(outputName);
        const modifiedFile = new Blob([modifiedFileArray], { type: "text/html" });

        // Download the file
        downloadName.value = outputName;
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
