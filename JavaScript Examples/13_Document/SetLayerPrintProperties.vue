<template>
  <span>The following example demonstrates how to set PDF layer print properties.</span>
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
        let inputFileName = 'Multipage.pdf';
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);

        //Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        doc.LoadFromFile({fileName: inputFileName});
        
        // Get the first page of the loaded document.
        let page = doc.Pages.get_Item(0);

        // Create a layer named "red line" within the Pdf document
        let layer = doc.Layers.AddLayer({name: "red line", state: wasmModule.PdfVisibility.On});

        // Set the print state of the layer as "Nerver"
        layer.PrintState = wasmModule.LayerPrintState.Nerver;

        // Draw a red line on the layer using the graphics of the page canvas
        let pcA = layer.CreateGraphics(page.Canvas);
        let pdfRGBColor = wasmModule.PdfRGBColor.Create({red: 255, green: 0, blue: 0});
        let pen = wasmModule.PdfPen.Create({pdfRgbColor: pdfRGBColor, width: 2});
        pcA.DrawLine({
            pen: pen,
            point1: wasmModule.PointF.Create({x: 100, y: 350}),
            point2: wasmModule.PointF.Create({x: 300, y: 350})
        });

        // Define the output file name
        const outputFileName = "SetLayerPrintProperties_result.pdf";

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
