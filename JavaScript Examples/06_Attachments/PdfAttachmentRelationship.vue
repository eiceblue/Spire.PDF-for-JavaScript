<template>
  <span>The following example demonstrates how to add addachment and specify the relationship in PDF document.</span>
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
        let inputFileName = "Attachment.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        let attachmentFileName = "E-iceblueLogo.png";
        await wasmModule.FetchFileToVFS(attachmentFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        // Load the PDF fle
        doc.LoadFromFile({ fileName: inputFileName });

        // Define PdfAttachment
        let attachment = wasmModule.PdfAttachment.Create({ fileName: attachmentFileName });

        // Add addachment  attachment, associatedDocument, association
        doc.Attachments.Add({
          attachment: attachment,
          associatedDocument: doc,
          association: wasmModule.PdfAttachmentRelationship.Alternative
        });

        // Define the output file name
        const outputFileName = "PdfAttachmentRelationship_result.pdf";

        // Save the document to the specified path
        doc.SaveToFile({ fileName: outputFileName });

        // Clean up resources
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
