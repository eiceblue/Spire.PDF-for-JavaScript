<template>
  <span>The following example demonstrates how to setting properties of word file when converting PDF to Word</span>
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

        // Load the input file into the virtual file system (VFS)
        const inputFileName = "ConvertToWordSettingProperties.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create an instance of the PdfToDocConverter class with the specified input file path
        let converter = spirepdf.PdfToDocConverter.Create({ filePath: inputFileName });

        // Set various properties for the converted Word document
        // Set the title of the Word document to "PDFTODOCX"
        converter.DocxOptions.Title = "PDFTODOCX";

        // Set the subject of the Word document to "Set document properties."
        converter.DocxOptions.Subject = "Set document properties.";

        // Set the tags of the Word document to "Test Tags"
        converter.DocxOptions.Tags = "Test Tags";

        // Set the categories of the Word document to "PDF"
        converter.DocxOptions.Categories = "PDF";

        // Set the comments of the Word document to "This document is just for testing the properties"
        converter.DocxOptions.Commments = "This document is just for testing the properties";

        // Set the authors of the Word document to "E-iceblue Support Team"
        converter.DocxOptions.Authors = "E-iceblue Support Team";

        // Set the last saved by of the Word document to "E-iceblue Support Team"
        converter.DocxOptions.LastSavedBy = "E-iceblue Support Team";

        // Set the revision version of the Word document to 8
        converter.DocxOptions.Revision = 8;

        // Set the version of the Word document to "csharp V4.0"
        converter.DocxOptions.Version = "csharp V4.0";

        // Set the program name of the Word document to "Spire.Pdf for .NET"
        converter.DocxOptions.ProgramName = "Spire.Pdf for .NET";

        // Set the company of the Word document to "E-iceblue"
        converter.DocxOptions.Company = "E-iceblue";

        // Set the manager of the Word document to "E-iceblue"
        converter.DocxOptions.Manager = "E-iceblue";

        // Define the output file name
        const outputFileName = "ConvertToWordSettingProperties.docx";
        converter.SaveToDocx({fileName: outputFileName});
       
        // Read the saved file and convert to a Blob object
        const modifiedFileArray = wasmModule.FS.readFile(outputFileName);
        const modifiedFile = new Blob([modifiedFileArray], { type: "application/msword" });

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
