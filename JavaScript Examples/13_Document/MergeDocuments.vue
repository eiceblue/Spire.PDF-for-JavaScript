<template>
  <span>The following example demonstrates how to merge multiple PDF documents into one.</span>
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
        let inputFileName1 = 'MergePdfsTemplate_1.pdf';
        await wasmModule.FetchFileToVFS(inputFileName1, '', `${import.meta.env.BASE_URL}static/data/`);

        let inputFileName2 = 'MergePdfsTemplate_2.pdf';
        await wasmModule.FetchFileToVFS(inputFileName2, '', `${import.meta.env.BASE_URL}static/data/`);

        let inputFileName3 = 'MergePdfsTemplate_3.pdf';
        await wasmModule.FetchFileToVFS(inputFileName3, '', `${import.meta.env.BASE_URL}static/data/`);
        //Pdf document list
        let files = [inputFileName1, inputFileName2, inputFileName3]
        //Open pdf documents
        let docs = Array.from({length: files.length}, () => null);
        let i = 0;
        while (i < files.length) {
            docs[i] = wasmModule.PdfDocument.Create();
            docs[i].LoadFromFile({fileName: files[i]});
            i++;
        }
        //Append document
        docs[0].AppendPage({doc:docs[1]})
        //Import page
        for (let i = 0; i < docs[2].Pages.Count; i += 2) {
            docs[0].InsertPage({ldDoc:docs[2], pageIndex:i});
        }

        // Define the output file name
        const outputFileName = "MergeDocuments_result.pdf";

        // Save the document to the specified path
        docs[0].SaveToFile({fileName: outputFileName});
        docs[0].Close();
        
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
