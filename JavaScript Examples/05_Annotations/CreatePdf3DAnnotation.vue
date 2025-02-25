<template>
  <span>The following example demonstrates how to create 3D annotation in PDF document.</span>
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
        let inputFileName = "CreatePdf3DAnnotation.u3d";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        // Create a new page
        let page = doc.Pages.Add();

        // Draw a rectangle on the page to define the canvas area for the 3D file.
        let rt = wasmModule.RectangleF.Create({ x: 0, y: 80, width: 200, height: 200 });

        let annotation = wasmModule.Pdf3DAnnotation.Create({ rectangle: rt, fileName: inputFileName });
        annotation.Activation = wasmModule.Pdf3DActivation.Create();
        annotation.Activation.ActivationMode = wasmModule.Pdf3DActivationMode.PageOpen;

        // Define a 3D view mode and set its parameter
        let View = wasmModule.Pdf3DView.Create();
        let pdfRgbColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Purple() });
        View.Background = wasmModule.Pdf3DBackground.Create({ color: pdfRgbColor });
        View.ViewNodeName = "3DAnnotation";
        View.RenderMode = wasmModule.Pdf3DRendermode.Create({ style: wasmModule.Pdf3DRenderStyle.Solid });
        View.InternalName = "3DAnnotation";
        View.LightingScheme = wasmModule.Pdf3DLighting.Create();
        View.LightingScheme.Style = wasmModule.Pdf3DLightingStyle.Day;

        // Set the 3D view mode for the annotation
        annotation.Views.Add(View);

        // Add the annotation to Pdf
        page.Annotations.Add(annotation);

        // Define the output file name
        const outputFileName = "CreatePdf3DAnnotation_result.pdf";

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
