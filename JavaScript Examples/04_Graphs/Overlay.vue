<template>
  <span>The following example demonstrates how to draw a page to another page.</span>
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
        let inputFileName1 = "Overlay1.pdf";
        await wasmModule.FetchFileToVFS(inputFileName1, "", `${import.meta.env.BASE_URL}static/data/`);

        let inputFileName2 = "Overlay2.pdf";
        await wasmModule.FetchFileToVFS(inputFileName2, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a PDF document
        let doc1 = wasmModule.PdfDocument.Create();

        // Load the first PDF fle
        doc1.LoadFromFile({ fileName: inputFileName1 });

        // Create a PDF document
        let doc2 = wasmModule.PdfDocument.Create();

        // Load the second PDF fle
        doc2.LoadFromFile({ fileName: inputFileName2 });

        // Create page template form the first page of doc1
        let template = doc1.Pages.get_Item(0).CreateTemplate();

        // Iterate each page in doc2
        for (let i = 0; i < doc2.Pages.Count; i++) {
          let page = doc2.Pages.get_Item(i);
          // Set transparency for page
          page.Canvas.SetTransparency({ alphaPen: 0.25, alphaBrush: 0.25, blendMode: wasmModule.PdfBlendMode.Overlay });

          // Draw template
          let point = wasmModule.PointF.Create({ x: 0, y: 0 });
          page.Canvas.DrawTemplate({ template: template, location: point });
        }

        // Define the output file name
        const outputFileName = "Overlay_result.pdf";

        // Save the document to the specified path
        doc2.SaveToFile({ fileName: outputFileName });

        // Clean up resources
        doc1.Close();
        doc2.Close();

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
