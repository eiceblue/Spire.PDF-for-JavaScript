<template>
  <span>The following example demonstrates how to get all attachments of PDF document.</span>
  <el-button @click="startProcessing">Start</el-button>
  <a v-if="downloadUrl" :href="downloadUrl" :download="downloadName">
    Click here to download the generated file
  </a>
</template>

<script>
import { ref } from "vue";
import JSZip from "jszip";

export default {
  setup() {
    const downloadUrl = ref(null);
    const downloadName = ref("");

    const startProcessing = async () => {
      wasmModule = window.wasmModule;
      if (wasmModule) {
        // Load the sample file into the virtual file system (VFS)
        let inputFileName = "Template_Pdf_2.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        // Load the PDF fle
        doc.LoadFromFile({ fileName: inputFileName });

        // Get a collection of attachments on the PDF document
        let collection = doc.Attachments;

        let outputDirectoryName = "attachmentFiles/";
        FS.mkdirTree(outputDirectoryName);

        // Save all the attachments to the files
        for (let i = 0; i < collection.Count; i++) {
          let attachment = collection.get_Item(i);
          let embeddedFileSpecification = new wasmModule.PdfEmbeddedFileSpecification(attachment.H);
          wasmModule.FS.writeFile(outputDirectoryName+embeddedFileSpecification.FileName, embeddedFileSpecification.Data);
        }

        // Clean up resources
        doc.Close();

        const zip = new JSZip();
        let items = await FS.readdir(outputDirectoryName);
        items = items.filter((item) => item !== "."
        
        && item !== "..");
        for (const item of items) {
          const itemPath = `${outputDirectoryName}/${item}`;
          const fileData = await FS.readFile(itemPath);
          zip.file(item, fileData);
        }

        const zipBlob = await zip.generateAsync({ type: "blob" });
        const zipDownloadUrl = URL.createObjectURL(zipBlob);
        const zipDownloadName = `attachmentFiles.zip`;
        downloadName.value = zipDownloadName;
        downloadUrl.value = zipDownloadUrl;
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
