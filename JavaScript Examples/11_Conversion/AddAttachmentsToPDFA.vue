<template>
  <span>The following example demonstrates how to add attachments to Pdf_A1B/Pdf_X1A2001/Pdf_A1A/Pdf_A2A document</span>
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
        // Load the ARIALUNI.TTF font file into the virtual file system (VFS)
        await wasmModule.FetchFileToVFS("ARIALUNI.TTF", "/Library/Fonts/", `${import.meta.env.BASE_URL}static/font/`);
        // Load the input files into the virtual file system (VFS)
        const inputFileName = "SampleB_2.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);
        const inputImageName1 = "SampleB_1.png";
        await wasmModule.FetchFileToVFS(inputImageName1, "", `${import.meta.env.BASE_URL}static/data/`);
        const inputFileName2 = "SampleB_1.pdf";
        await wasmModule.FetchFileToVFS(inputFileName2, "", `${import.meta.env.BASE_URL}static/data/`);

        // Convert the input PDF file to PDF/A-1b standard and save it to the memory stream
        let converter = spirepdf.PdfStandardsConverter.Create({ filePath: inputFileName });
        const toA1B_result = "SampleB_2_to_A1B_result.pdf";
        converter.ToPdfA1B({ filePath: toA1B_result });
        converter.Dispose();

        // Create a new PDF document
        let newDoc = spirepdf.PdfDocument.Create();

        // Load the converted PDF document from the memory stream
        newDoc.LoadFromFile({ fileName: toA1B_result });

        // Read the data of the first attachment file ("SampleB_1.png") into a byte array
        let data = wasmModule.FS.readFile(inputImageName1)
        // Create a PdfAttachment object with the attachment file name and data
        let attach1 = spirepdf.PdfAttachment.Create({ fileName: "attachment1.png", data: data });

        // Read the data of the second attachment file ("SampleB_1.pdf") into a byte array
        let data2 = wasmModule.FS.readFile(inputFileName2)
        // Create a PdfAttachment object with the attachment file name and data
        let attach2 = spirepdf.PdfAttachment.Create({ fileName: "attachment2.pdf", data: data2 });

        // Add the attachments to the new PDF document
        newDoc.Attachments.Add({ attachment: attach1 });
        newDoc.Attachments.Add({ attachment: attach2 });

        // Define the output file name
        const outputFileName = "ToPDFAWithAttachments.pdf";

        // Save the document to the specified path
        newDoc.SaveToFile({ fileName: outputFileName });
        newDoc.Close();

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
