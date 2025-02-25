<template>
  <span>The following example demonstrates how to set free text annotation subject in PDF files.</span>
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

        // Initialize a PdfFreeTextAnnotation
        let rect = wasmModule.RectangleF.Create({ x: 150, y: 120, width: 150, height: 30 });
        let textAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);

        // Specify content
        textAnnotation.Text = "\nSet free text annotation subject";

        // Set subject
        textAnnotation.Subject = "SubjectTest";

        // Create a new PdfFont object using the Times Roman font family and a font size of 20
        let font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.TimesRoman, size: 10 });

        // Create a new PdfAnnotationBorder object with a specified width
        let border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 1 });

        // Assign the created font to the font property of the free text annotation
        textAnnotation.Font = font;

        // Assign the created border to the border property of the free text annotation
        textAnnotation.Border = border;

        // Set the border color of the free text annotation to gray
        textAnnotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Purple() });

        // Set the line ending style of the free text annotation to slash
        textAnnotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.Circle;

        // Set the background color of the free text annotation to light blue
        textAnnotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Green() });

        // Set the opacity of the free text annotation to 0.8
        textAnnotation.Opacity = 0.8;
        page.Annotations.Add(textAnnotation);

        // Define the output file name
        const outputFileName = "SetFreeTextAnnotationSubject_result.pdf";

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
