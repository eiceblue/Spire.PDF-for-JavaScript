<template>
  <span>The following example demonstrates how to add launch action in PDF document</span>
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
        // Load the ARIALUNI.TTF font file into the virtual file system (VFS)
        await wasmModule.FetchFileToVFS("ARIALUNI.TTF", "/Library/Fonts/", `${import.meta.env.BASE_URL}static/font/`);

        // Load the input file into the virtual file system (VFS)
        const inputFileName = "text.txt";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a new PdfDocument object
        let doc = wasmModule.PdfDocument.Create();
        
        // Add a new page to the document and assign it to the 'page' variable
        let page = doc.Pages.Add();

        // Create a PDF Launch Action that will open a text file
        let launchAction = spirepdf.PdfLaunchAction.Create({ fileName: inputFileName });

        // Create a PDF Action Annotation with the PDF Launch Action
        let text = "Click here to open file";
        let trueTypeFont1 = wasmModule.PdfTrueTypeFont.Create({
            fontFile: "ARIALUNI.TTF",
            size: 13,
            style: spirepdf.PdfFontStyle.Regular
        });
        let font = trueTypeFont1;

        let rect = spirepdf.RectangleF.Create({ x: 50, y: 50, width: 230, height: 15 });
        page.Canvas.DrawString({ s: text, font: font, brush: spirepdf.PdfBrushes.get_ForestGreen(), layoutRectangle: rect });
        let annotation = spirepdf.PdfActionAnnotation.Create(rect, launchAction);

        // Add the PDF Action Annotation to the page
        page.Annotations.Add(annotation);

        // Define the output file name
        const outputFileName = "AddPdfLaunchAction.pdf";

        // Save the document to the specified path
        doc.SaveToFile({ fileName: outputFileName });
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
