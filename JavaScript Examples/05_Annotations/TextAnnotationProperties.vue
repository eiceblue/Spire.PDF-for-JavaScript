<template>
  <span>The following example demonstrates how to copy textAnnotation properties to an other annotation.</span>
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
        let inputFileName = "FreeTextAnnotation.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        // Load the PDF fle
        doc.LoadFromFile({ fileName: inputFileName });

        // Get the first page
        let firstPage = doc.Pages.get_Item(0);

        // Create a new PDF document object to store the copied annotations
        let newPdf = wasmModule.PdfDocument.Create();

        // Iterate through each annotation in the first page's annotation list
        for (let i = 0; i < firstPage.Annotations.Count; i++) {
          let annotation = firstPage.Annotations.get_Item(i);
          // Check if the annotation is a free text annotation
          if (annotation instanceof wasmModule.PdfFreeTextAnnotationWidget) {
            // Convert the annotation to a free text annotation object
            let textAnnotation = firstPage.Annotations.get_Item(i);

            // Retrieve the rectangle bounds of the annotation
            let rect = textAnnotation.Bounds;

            // Retrieve the text content of the annotation
            let annotBase = new wasmModule.PdfAnnotation(textAnnotation.H);
            let text = annotBase.Text;

            // Create a new page in the new PDF document with the same size as the first page
            let newPage = newPdf.Pages.Add({ size: firstPage.Size });

            // Create a new free text annotation with the same rectangle bounds
            let newAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);

            // Set the text content of the new annotation
            newAnnotation.Text = text;

            // Copy the callout lines information from the original annotation
            newAnnotation.CalloutLines = textAnnotation.CalloutLines;

            // Copy the line ending style from the original annotation
            newAnnotation.LineEndingStyle = textAnnotation.LineEndingStyle;

            // Set the annotation intent to indicate it's a free text callout
            newAnnotation.AnnotationIntent = wasmModule.PdfAnnotationIntent.FreeTextCallout;

            // Copy the rectangular differences information from the original annotation
            newAnnotation.RectangleDifferences = textAnnotation.RectangularDifferenceArray;

            // Set the color of the new annotation to match the original annotation
            newAnnotation.Color = textAnnotation.Color;

            // Add the new annotation to the annotations widget of the new page
            newPage.Annotations.Add(newAnnotation);
          }
        }

        // Define the output file name
        const outputFileName = "TextAnnotationProperties_result.pdf";

        // Save the document to the specified path
        newPdf.SaveToFile({ fileName: outputFileName });

        // Clean up resources
        doc.Close();
        newPdf.Close();

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
