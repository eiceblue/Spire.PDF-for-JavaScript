<template>
  <span>The following example demonstrates how to modify line annotations in PDF document.</span>
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
        let inputFileName = "PdfLineAnnotation.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        // Load the PDF fle
        doc.LoadFromFile({ fileName: inputFileName });

        let page = doc.Pages.get_Item(0);

        //Get the annotation collection from the document.
        let annotation = page.Annotations.get_Item(0);

        // Check if the retrieved annotation is a PdfLineAnnotationWidget
        if (annotation instanceof wasmModule.PdfLineAnnotationWidget) {
          // Cast the annotation as a PdfLineAnnotationWidget to access its specific properties and methods
          let lineAnn = annotation;

          // Set the author property of the line annotation to "Author_test"
          lineAnn.Author = "Author_test";

          // Set the subject property of the line annotation to "Subject_test"
          lineAnn.Subject = "Subject_test";
        }

        // Define the output file name
        const outputFileName = "ModifyLineAnnotation_result.pdf";

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
