<template>
  <span>The following example demonstrates how to set free text annotation style in PDF document.</span>
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

        // Define and create a free text annotation object
        let rect = wasmModule.RectangleF.Create({ x: 150, y: 120, width: 150, height: 30 });
        let textAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);

        // Specify content for the annotation
        textAnnotation.Text = "\nFree Text Annotation Formatting";

        // Set the formatting for the first free text annotation and add it to the page
        let font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.TimesRoman, size: 10 });
        let border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 1 });
        textAnnotation.Font = font;
        textAnnotation.Border = border;
        textAnnotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Purple() });
        textAnnotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.Circle;
        textAnnotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Green() });
        textAnnotation.Opacity = 0.8;
        page.Annotations.Add(textAnnotation);

        // Define a second rectangle for another free text annotation
        rect = wasmModule.RectangleF.Create({ x: 150, y: 200, width: 150, height: 40 });
        textAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);
        textAnnotation.Text = "\nFree Text Annotation Formatting";
        
        // Set formatting for the second annotation
        border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 1 });
        font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 10 });
        textAnnotation.Font = font;
        textAnnotation.Border = border;
        textAnnotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightGoldenrodYellow() });
        textAnnotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.RClosedArrow;
        textAnnotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightPink() });
        textAnnotation.Opacity = 0.8;
        page.Annotations.Add(textAnnotation);

        // Define a third rectangle for a different free text annotation
        rect = wasmModule.RectangleF.Create({ x: 150, y: 280, width: 280, height: 40 });
        textAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);
        textAnnotation.Text = "\noHow to Set Free Text Annotation Formatting in Pdf file";
        
        // Set formatting for the third annotation
        border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 1 });
        font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 10 });
        textAnnotation.Font = font;
        textAnnotation.Border = border;
        textAnnotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Gray() });
        textAnnotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.Circle;
        textAnnotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightSkyBlue() });
        textAnnotation.Opacity = 0.8;
        page.Annotations.Add(textAnnotation);

        // Define a fourth rectangle for yet another free text annotation
        rect = wasmModule.RectangleF.Create({ x: 150, y: 360, width: 200, height: 40 });
        textAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);
        textAnnotation.Text = "\nFree Text Annotation Formatting";
        
        // Set formatting for the fourth annotation
        border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 1 });
        font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 10 });
        textAnnotation.Font = font;
        textAnnotation.Border = border;
        textAnnotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Pink() });
        textAnnotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.RClosedArrow;
        textAnnotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightGreen() });
        textAnnotation.Opacity = 0.8;
        page.Annotations.Add(textAnnotation);

        // Define the output file name
        const outputFileName = "SetFreeTextAnnotationStyle_result.pdf";

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
