<template>
  <span>The following example demonstrates how to create line annotation in PDF document.</span>
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

        // Get the first page
        let page = doc.Pages.Add();

        // Create a line annotation
        let linePoints = [100, 650, 180, 650];
        let lineAnnotation = wasmModule.PdfLineAnnotation.Create({
          linePoints: linePoints,
          text: "This is the first line annotation"
        });

        // Set the line border
        lineAnnotation.lineBorder.BorderStyle = wasmModule.PdfBorderStyle.Solid;
        lineAnnotation.lineBorder.BorderWidth = 1;

        // Set the line intent
        lineAnnotation.LineIntent = wasmModule.PdfLineIntent.LineDimension;

        // Set the line style
        lineAnnotation.BeginLineStyle = wasmModule.PdfLineEndingStyle.Butt;
        lineAnnotation.EndLineStyle = wasmModule.PdfLineEndingStyle.Diamond;

        // Set the line flag
        lineAnnotation.Flags = wasmModule.PdfAnnotationFlags.Default;

        // Set the line color
        lineAnnotation.InnerLineColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Green() });
        lineAnnotation.BackColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Green() });

        // Set the leader line
        lineAnnotation.LeaderLineExt = 0;
        lineAnnotation.LeaderLine = 0;

        // Add the line annotation to the page
        page.Annotations.Add(lineAnnotation);

        // Create another line annotation
        linePoints = [100, 550, 280, 550];
        lineAnnotation = wasmModule.PdfLineAnnotation.Create({
          linePoints: linePoints,
          text: "This is the second line annotation"
        });
        lineAnnotation.lineBorder.BorderStyle = wasmModule.PdfBorderStyle.Underline;
        lineAnnotation.lineBorder.BorderWidth = 2;
        lineAnnotation.LineIntent = wasmModule.PdfLineIntent.LineArrow;
        lineAnnotation.BeginLineStyle = wasmModule.PdfLineEndingStyle.Circle;
        lineAnnotation.EndLineStyle = wasmModule.PdfLineEndingStyle.Diamond;
        lineAnnotation.Flags = wasmModule.PdfAnnotationFlags.Default;
        lineAnnotation.InnerLineColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Pink() });
        lineAnnotation.BackColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Pink() });
        lineAnnotation.LeaderLineExt = 0;
        lineAnnotation.LeaderLine = 0;
        page.Annotations.Add(lineAnnotation);

        // Create another line annotation
        linePoints = [100, 450, 280, 450];
        lineAnnotation = wasmModule.PdfLineAnnotation.Create({
          linePoints: linePoints,
          text: "This is the third line annotation"
        });
        lineAnnotation.lineBorder.BorderStyle = wasmModule.PdfBorderStyle.Beveled;
        lineAnnotation.lineBorder.BorderWidth = 2;
        lineAnnotation.LineIntent = wasmModule.PdfLineIntent.LineDimension;
        lineAnnotation.BeginLineStyle = wasmModule.PdfLineEndingStyle.None;
        lineAnnotation.EndLineStyle = wasmModule.PdfLineEndingStyle.None;
        lineAnnotation.Flags = wasmModule.PdfAnnotationFlags.Default;
        lineAnnotation.InnerLineColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Blue() });
        lineAnnotation.BackColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Blue() });
        lineAnnotation.LeaderLineExt = 1;
        lineAnnotation.LeaderLine = 1;
        page.Annotations.Add(lineAnnotation);

        // Define the output file name
        const outputFileName = "CreatePdfLineAnnotation_result.pdf";

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
