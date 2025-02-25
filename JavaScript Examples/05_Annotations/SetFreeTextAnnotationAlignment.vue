<template>
  <span>The following example demonstrates how to set FreeTextAnnotation alignment.</span>
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

        // Define a rectangle with specific coordinates and dimensions
        let rect = wasmModule.RectangleF.Create({ x: 0, y: 300, width: 200, height: 80 });

        // Create a new PdfFreeTextAnnotation object and pass the rectangle as a parameter
        let textAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);

        // Set the text content of the free text annotation
        textAnnotation.Text = "\n  Spire.PDF";

        // Create a new PdfAnnotationBorder object with a specified width
        let border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 1 });

        // Create a new PdfFont object using the Times Roman font family and a font size of 20
        let font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.TimesRoman, size: 20 });

        // Assign the created font to the font property of the free text annotation
        textAnnotation.Font = font;

        // Assign the created border to the border property of the free text annotation
        textAnnotation.Border = border;

        // Set the border color of the free text annotation to gray
        textAnnotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Gray() });

        // Set the line ending style of the free text annotation to slash
        textAnnotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.Slash;

        // Set the background color of the free text annotation to light blue
        textAnnotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightBlue() });

        // Set the opacity of the free text annotation to 0.8
        textAnnotation.Opacity = 0.8;

        // Set the alignment of the text within the free text annotation to center
        textAnnotation.TextAlignment = wasmModule.PdfAnnotationTextAlignment.Center;

        // Add the free text annotation to the annotations collection of the page
        page.Annotations.Add(textAnnotation);

        // Define the output file name
        const outputFileName = "SetFreeTextAnnotationAlignment_result.pdf";

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
