<template>
  <span>The following example shows how to set properties for stamp</span>
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
        const inputFileName = "TextStamp.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a new PDF document object.
        let doc = wasmModule.PdfDocument.Create();
        // Load an existing PDF document from a file.
        doc.LoadFromFile({ fileName: inputFileName });

        // Get the first page of the loaded document.
        let page = doc.Pages.get_Item(0);

        // Traverse through each annotation widget on the page.
        for (let i = 0; i < page.Annotations.Count; i++) {
          // Check if the current annotation is a rubber stamp annotation.
          if (page.Annotations.get_Item(i) instanceof spirepdf.PdfRubberStampAnnotationWidget) {
            // Cast the annotation to a PdfRubberStampAnnotationWidget.
            let stamp = page.Annotations.get_Item(i);

            // Set properties for the rubber stamp annotation.
            stamp.Author = "TestUser";
            stamp.Subject = "E-iceblue";
            stamp.CreationDate = spirepdf.DateTime.get_Now();
            stamp.ModifiedDate = spirepdf.DateTime.get_Now();
          }
        }

        // Define the output file name
        const outputFileName = "SetPropertiesForStamp.pdf";

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
