<template>
  <span>The following example demonstrates how to create polygon annotation in PDF document.</span>
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

        // Create a new page
        let page = doc.Pages.Add();

        // Initialize an instance of PdfPolygonAnnotation, specifying all vertex coordinates which can form a complete shape
        let point1 = wasmModule.PointF.Create({ x: 0, y: 30 });
        let point2 = wasmModule.PointF.Create({ x: 30, y: 15 });
        let point3 = wasmModule.PointF.Create({ x: 60, y: 30 });
        let point4 = wasmModule.PointF.Create({ x: 45, y: 50 });
        let point5 = wasmModule.PointF.Create({ x: 15, y: 50 });
        let point6 = wasmModule.PointF.Create({ x: 0, y: 30 });
        let points = [point1, point2, point3, point4, point5, point6];
        let polygon = wasmModule.PdfPolygonAnnotation.Create(page, points);

        // Set the border color, text, border effect and other properties of polygon annotation
        polygon.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_PaleVioletRed() });
        polygon.Text = "This is a polygon annotation";
        polygon.Author = "E-ICEBLUE";
        polygon.Subject = "polygon annotation demo";
        polygon.BorderEffect = wasmModule.PdfBorderEffect.BigCloud;
        polygon.ModifiedDate = wasmModule.DateTime.get_Now();

        // Add the annotation to Pdf page
        page.Annotations.Add(polygon);

        // Define the output file name
        const outputFileName = "CreatePdfPolygonAnnotation_result.pdf";

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
