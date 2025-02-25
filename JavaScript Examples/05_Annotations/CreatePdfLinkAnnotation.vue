<template>
  <span>The following example demonstrates how to create link annotation in PDF document.</span>
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
        let inputFileName = "Template_Pdf_3.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        // Create a new page
        let page = doc.Pages.Add();

        // Define the rectangle for the file link annotation
        let rect = wasmModule.RectangleF.Create({ x: 0, y: 40, width: 250, height: 35 });

        // Create a file link annotation with the specified rectangle and input file name
        let link = wasmModule.PdfFileLinkAnnotation.Create(rect, inputFileName);
        
        // Add the file link annotation to the page's annotation collection
        page.Annotations.Add(link);

        // Create a free text annotation using the same rectangle
        let text = wasmModule.PdfFreeTextAnnotation.Create(rect);
        text.Text = "Click here! This is a link annotation.";

        // Define the font for the free text annotation
        let font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 15 });
        text.Font = font;

        // Add the free text annotation to the page's annotation collection
        page.Annotations.Add(text);

        // Define the output file name
        const outputFileName = "CreatePdfLinkAnnotation_result.pdf";

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
