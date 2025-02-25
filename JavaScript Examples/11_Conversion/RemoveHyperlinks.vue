<template>
  <span>The following example demonstrates how to remove hyperlinks in PDF document.</span>
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
        let inputFileName = "RemoveHyperlinks.pdf";
        await wasmModule.FetchFileToVFS(inputFileName,"",`${import.meta.env.BASE_URL}static/data/`);
        //Create a  document
        let doc = wasmModule.PdfDocument.Create();
	      doc.LoadFromFile(inputFileName);

        // Get the first page
        let page = doc.Pages.get_Item(0);
        // Get the annotation collection
        let widgetCollection = page.AnnotationsWidget;
        // Verify whether widgetCollection is null or not
        if (widgetCollection.Count > 0) {
            for (let i = 0; i < widgetCollection.Count; i++) {
                let annotation = widgetCollection.get_Item(i)
                // Get the TextWebLink Annotation
                if (annotation instanceof wasmModule.PdfTextWebLinkAnnotationWidget) {
                    let link = annotation;
                    // Remove the TextWebLink annotation
                    widgetCollection.Remove(link);
                }
            }
        }

        // Define the output file name
        const outputFileName = "RemoveHyperlinks_out.pdf";

        // Save the document to the specified path
        doc.SaveToFile({fileName: outputFileName});
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
