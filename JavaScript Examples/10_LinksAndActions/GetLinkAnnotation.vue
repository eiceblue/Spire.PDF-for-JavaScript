<template>
  <span>The following example demonstrates how to get information of link annotations in PDF document</span>
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
        let annotations = page.Annotations;

        //Create StringBuilder to save
        let content = "";

        //Verify whether widgetCollection is not null or not
        if (annotations.Count > 0) {
          //traverse the PdfAnnotationCollection
          for (let i = 0; i < annotations.Count; i++) {
            //if it is PdfTextWebLinkAnnotationWidget
            if (annotations.get_Item(i) instanceof spirepdf.PdfTextWebLinkAnnotationWidget) {
              //Get the link annotation
              let WebLinkAnnotation = annotations.get_Item(i);
              let url = WebLinkAnnotation.Url;
              //Add strings to StringBuilder
              content = content + "The url of link annotation is " + url + "\r\n";
              content = content + "The text of link annotation is " + WebLinkAnnotation.Text + "\r\n";
            }
          }
        }
        doc.Close();
        // Read the saved file and convert to a Blob object
        const outputFileName = "GetLinkAnnotation.txt";
        const modifiedFile = new Blob([content], { type: "application/txt" });

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
