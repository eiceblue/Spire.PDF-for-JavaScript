<template>
  <span>The following example demonstrates how to get particular annotation infomation from PDF document.</span>
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

        // Load the PDF fle
        doc.LoadFromFile({ fileName: inputFileName });

        let page = doc.Pages.get_Item(0);

        // Get the annotation collection from the document
        let annotations = page.Annotations;

        // Get particular annotation information from the document
        let content = "";
        if (annotations.get_Item(0) instanceof wasmModule.PdfTextAnnotationWidget) {
          let textAnnotation = annotations.get_Item(0);
          let styledAnnotationWidget = new wasmModule.PdfStyledAnnotationWidget(textAnnotation.H);
          content = content + "Annotation text: " + styledAnnotationWidget.Text + "\r\n";
          let annotBase = new wasmModule.PdfAnnotation(textAnnotation.H);
          content = content + "Annotation ModifiedDate: " + annotBase.ModifiedDate.ToString() + "\r\n";
          let markUpAnnotationWidget = new wasmModule.PdfMarkUpAnnotationWidget(textAnnotation.H);
          content = content + "Annotation author: " + markUpAnnotationWidget.Author + "\r\n";
          content = content + "Annotation Name: " + annotBase.Name + "\r\n";
        }

        // Define the output file name
        const outputFileName = "GetParticularAnnotationInfo_result.txt";

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
