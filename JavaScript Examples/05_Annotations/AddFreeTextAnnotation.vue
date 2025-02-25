<template>
  <span>The following example demonstrates how to add free text annotation in PDF document.</span>
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
        let inputFileName = "AddFreeTextAnnotation.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        // Load the PDF fle
        doc.LoadFromFile({ fileName: inputFileName });

        // Get the first page
        let page = doc.Pages.get_Item(0);

        // Define a rectangle for the free text annotation
        let rect = wasmModule.RectangleF.Create({ x: 0, y: 300, width: 100, height: 80 });

        // Create a free text annotation and set its properties
        let textAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);
        textAnnotation.Text = "\n  Spire.PDF";

        // Set border style for annotation
        let border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 1 });
        textAnnotation.Border = border;
        textAnnotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Gray() });
        textAnnotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.Slash;

        // Set font properties for the annotation
        let font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.TimesRoman, size: 20 });
        textAnnotation.Font = font;

        // Set color and opacity for the text annotation
        textAnnotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightBlue() });
        textAnnotation.Opacity = 0.8;

        // Add the free text annotation to the page
        page.Annotations.Add(textAnnotation);

        // Define a rectangle for the second free text annotation
        rect = wasmModule.RectangleF.Create({ x: 150, y: 200, width: 150, height: 40 });
        textAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);
        textAnnotation.Text = "\nHigh Fidelity Pdf file Conversion";

        // Set border style and font for the second annotation
        border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 1 });
        font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 10 });
        textAnnotation.Font = font;
        textAnnotation.Border = border;
        textAnnotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightGoldenrodYellow() });
        textAnnotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.RClosedArrow;
        textAnnotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightPink() });
        textAnnotation.Opacity = 0.8;

        // Add the second annotation to the page
        page.Annotations.Add(textAnnotation);

        // Define a rectangle for the third free text annotation
        rect = wasmModule.RectangleF.Create({ x: 150, y: 280, width: 280, height: 40 });
        textAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);
        textAnnotation.Text = "\nEasily Manipulate document and Form fields";

        // Set border style and font for the third annotation
        border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 1 });
        font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 10 });
        textAnnotation.Font = font;
        textAnnotation.Border = border;
        textAnnotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Gray() });
        textAnnotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.Circle;
        textAnnotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightSkyBlue() });
        textAnnotation.Opacity = 0.8;

        // Add the third annotation to the page
        page.Annotations.Add(textAnnotation);

        // Define a rectangle for the fourth free text annotation
        rect = wasmModule.RectangleF.Create({ x: 150, y: 360, width: 200, height: 40 });
        textAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);
        textAnnotation.Text = "\nSecurity features";

        // Set border style and font for the fourth annotation
        border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 1 });
        font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 10 });
        textAnnotation.Font = font;
        textAnnotation.Border = border;
        textAnnotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Pink() });
        textAnnotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.RClosedArrow;
        textAnnotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightGreen() });
        textAnnotation.Opacity = 0.8;

        // Add the fourth annotation to the page
        page.Annotations.Add(textAnnotation);

        // Define a rectangle for the fifth free text annotation
        rect = wasmModule.RectangleF.Create({ x: 150, y: 440, width: 200, height: 40 });
        textAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);
        textAnnotation.Text = "\nExtract data from Pdf documents";

        // Set border style and font for the fifth annotation
        border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 1 });
        font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 10 });
        textAnnotation.Font = font;
        textAnnotation.Border = border;
        textAnnotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_OrangeRed() });
        textAnnotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.RClosedArrow;
        textAnnotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightGoldenrodYellow() });
        textAnnotation.Opacity = 0.8;

        // Add the fifth annotation to the page
        page.Annotations.Add(textAnnotation);

        // Define the output file name
        const outputFileName = "AddFreeTextAnnotation_result.pdf";

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
