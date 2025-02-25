<template>
  <span>The following example demonstrates how to add layers in PDF document.</span>
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

        // Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        let page = doc.Pages.Add();
        // Create a new layer named "red line" with visibility set to "On"
        let layer = doc.Layers.AddLayer({name:"red line",state:wasmModule.PdfVisibility.On});
        // Create a graphics context for drawing on the specified page's canvas using the created layer
        let pcA = layer.CreateGraphics(page.Canvas)
        // Draw a red line on the graphics context using a pen with thickness 2, starting from (100, 350) to (300, 350)
        pcA.DrawLine({pen:wasmModule.PdfPen.Create({brush:wasmModule.PdfBrushes.get_Red(), width:2.0}), point1:spirepdf.PointF.Create({x:100,y:350}), point2:spirepdf.PointF.Create({x:300,y:350})})
        //create a layer named "blue line"
        layer = doc.Layers.AddLayer({name:"blue line"});
        let pcB = layer.CreateGraphics(page.Canvas)
        pcA.DrawLine({pen:wasmModule.PdfPen.Create({brush:wasmModule.PdfBrushes.get_Blue(), width:2.0}), point1:spirepdf.PointF.Create({x:100,y:400}), point2:spirepdf.PointF.Create({x:300,y:400})})
        //create a layer named "green line"
        layer = doc.Layers.AddLayer({name:"green line"});
        let pcC = layer.CreateGraphics(page.Canvas)
        pcA.DrawLine({pen:wasmModule.PdfPen.Create({brush:wasmModule.PdfBrushes.get_Green(), width:2.0}), point1:spirepdf.PointF.Create({x:100,y:450}), point2:spirepdf.PointF.Create({x:300,y:450})})


        // Define the output file name
        const outputFileName = "AddLayers_result.pdf";

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
