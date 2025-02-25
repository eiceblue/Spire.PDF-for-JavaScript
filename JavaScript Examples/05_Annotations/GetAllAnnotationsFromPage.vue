<template>
  <span>The following example demonstrates how to get all annotations from page.</span>
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

        //Get all annotations from the first page
        let annotations = doc.Pages.get_Item(0).Annotations;

        let content = "";

        for (let i = 0; i < annotations.Count; i++) {
          // A text annotation will attach a popup annotation since they are father-son relationship.
          // The annotation information exists in the text annotation, so here we mask the blank popup annotation.
          if (annotations.get_Item(i) instanceof wasmModule.PdfPopupAnnotationWidget)
            continue;
          let popupAnnotationWidget = annotations.get_Item(i);
          content = content + "Annotation information: " + "\r\n";
          let annotBase = new wasmModule.PdfAnnotation(popupAnnotationWidget.H);
          content = content + "Text: " + annotBase.Text + "\r\n";
          let modifiedDate = annotBase.ModifiedDate.ToString() + "\r\n";
          content = content +  "ModifiedDate: "+modifiedDate + "\r\n";
        }

        // Define the output file name
        const outputFileName = "GetAllAnnotationsFromPage_result.txt";

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
