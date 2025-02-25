<template>
  <span>The following example demonstrates how to invisible free text annotation in PDF document.</span>
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
        let inputFileName = "Template_Pdf_4.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        // Load the PDF fle
        doc.LoadFromFile({ fileName: inputFileName });

        // Get the first page
        let page = doc.Pages.get_Item(0);

        // Define a rectangle with specific coordinates and dimensions
        let rect = wasmModule.RectangleF.Create({ x: 100, y: 120, width: 150, height: 30 });

        // Create a new free text annotation using the defined rectangle
        let FreetextAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);

        // Set the text content of the free text annotation
        FreetextAnnotation.Text = "Invisible Free Text Annotation";

        // Specify the font for the free text annotation
        let font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.TimesRoman, size: 10 });

        // Specify the border width for the free text annotation
        let border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 1 });

        // Set various properties for the free text annotation
        FreetextAnnotation.Font = font;
        FreetextAnnotation.Border = border;
        FreetextAnnotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Purple() });
        FreetextAnnotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.Circle;
        FreetextAnnotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Green() });
        FreetextAnnotation.Opacity = 0.8;

        // Set flags to make the free text annotation invisible
        FreetextAnnotation.Flags = wasmModule.PdfAnnotationFlags.NoView;

        // Add the free text annotation to the page's annotations collection
        page.AnnotationsWidget.Add(FreetextAnnotation);

        // Define a new rectangle for the next free text annotation
        rect = wasmModule.RectangleF.Create({ x: 100, y: 180, width: 150, height: 30 });

        // Create another free text annotation using the new rectangle
        FreetextAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);

        // Set properties for the second free text annotation
        FreetextAnnotation.Text = "Show Free Text Annotation";
        FreetextAnnotation.Font = font;
        FreetextAnnotation.Border = border;
        FreetextAnnotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightPink() });
        FreetextAnnotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.Circle;
        FreetextAnnotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightGreen() });
        FreetextAnnotation.Opacity = 0.8;

        // Add the second free text annotation to the page's annotations collection
        page.AnnotationsWidget.Add(FreetextAnnotation);

        // Define the output file name
        const outputFileName = "InvisibleFreeTextAnnotation_result.pdf";

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
