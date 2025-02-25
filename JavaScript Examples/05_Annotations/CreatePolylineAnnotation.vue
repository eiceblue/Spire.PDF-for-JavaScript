<template>
  <span>The following example demonstrates how to create polyline annotation in PDF document.</span>
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

        // Create a polyline annotation
        let point1 = wasmModule.PointF.Create({ x: 0, y: 60 });
        let point2 = wasmModule.PointF.Create({ x: 30, y: 45 });
        let point3 = wasmModule.PointF.Create({ x: 60, y: 90 });
        let point4 = wasmModule.PointF.Create({ x: 90, y: 80 });
        let points = [point1, point2, point3, point4];
        let polyline = wasmModule.PdfPolyLineAnnotation.Create(page, points);

        // Set properties of polyline
        polyline.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_PaleVioletRed() });
        polyline.Text = "This is a polygon annotation";
        polyline.Author = "E-ICEBLUE";
        polyline.Subject = "polygon annotation demo";
        polyline.Name = "Test Annotation";
        polyline.Border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 1 });
        polyline.ModifiedDate = wasmModule.DateTime.get_Now();
        
        // Add the annotation into page
        page.Annotations.Add(polyline);

        // Define the output file name
        const outputFileName = "CreatePolylineAnnotation_result.pdf";

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
