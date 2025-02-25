<template>
  <span>The following example demonstrates how to update the link annotation in PDF document</span>
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
        const inputFileName = "LinkAnnotation.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a new PDF document object.
        let doc = wasmModule.PdfDocument.Create();
        // Load an existing PDF document from a file.
        doc.LoadFromFile({ fileName: inputFileName });

        // Get the first page of the loaded document.
        let page = doc.Pages.get_Item(0);

        //Get the annotation collection
        let annotations = page.AnnotationsWidget;

        //Verify whether widgetCollection is not null or not
        if (annotations.Count > 0) {
          //traverse the PdfAnnotationCollection
          for (let i = 0; i < page.Annotations.Count; i++) {
            //if it is PdfTextWebLinkAnnotationWidget
            if (page.Annotations.get_Item(i) instanceof spirepdf.PdfTextWebLinkAnnotationWidget) {
              //Get the link annotation
              let annotation = page.Annotations.get_Item(i);
              //Change the url
              annotation.Url = "http://www.e-iceblue.com/Introduce/pdf-for-net-introduce.html";
            }
          }
        }

        // Define the output file name
        const outputFileName = "ExtractAndUpdateLink.pdf";

        // Save the document to the specified path
        doc.SaveToFile({ fileName: outputFileName });
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
