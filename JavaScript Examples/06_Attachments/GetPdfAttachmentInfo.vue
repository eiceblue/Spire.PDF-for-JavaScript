<template>
  <span>The following example demonstrates how to get Pdf attachment information.</span>
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
        let inputFileName = "Template_Pdf_2.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        // Load the PDF fle
        doc.LoadFromFile({ fileName: inputFileName });

        // Get a collection of attachments on the PDF document
        let collection = doc.Attachments;

        // Get the first attachment
        let attachment = collection.get_Item(0);
        let embeddedFileSpecification = new wasmModule.PdfEmbeddedFileSpecification(attachment.H);

        // Get the information of the first attachment
        let content = "";
        content = content + "Filename: " + embeddedFileSpecification.FileName + "\r\n";
        content = content + "Description: " + embeddedFileSpecification.Description + "\r\n";
        content = content + "Creation Date: " + embeddedFileSpecification.CreationDate.ToString() + "\r\n";
        content = content + "Modification Date: " + embeddedFileSpecification.ModificationDate.ToString() + "\r\n";

        // Define the output file name
        const outputFileName = "GetPdfAttachmentInfo_result.txt";

        // Clean up resources
        doc.Close();

        // Save the content to the specified path
        wasmModule.FS.writeFile(outputFileName, content);

        // Read the saved file and convert to a Blob object
        const modifiedFileArray = wasmModule.FS.readFile(outputFileName);
        const modifiedFile = new Blob([modifiedFileArray], { type: "text/plain" });

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
