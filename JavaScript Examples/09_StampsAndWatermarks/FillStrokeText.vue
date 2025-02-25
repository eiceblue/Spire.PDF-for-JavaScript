<template>
  <span>The following example demonstrates how to fill stroke text in PDF document</span>
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
        const inputFileName = "PDFTemplate_N.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a new PDF document object
        let doc = wasmModule.PdfDocument.Create();
        // Load an existing PDF document from VFS
        doc.LoadFromFile({ fileName: inputFileName });

        // Get the first page of the loaded document.
        let page = doc.Pages.get_Item(0);

        // Define a PDF pen with gray color
        let pen = spirepdf.PdfPen.Create({ color: spirepdf.Color.get_Gray() });

        // Save the current graphics state
        let state = page.Canvas.Save();

        // Rotate the page canvas by -20 degrees
        page.Canvas.RotateTransform({ angle: -20 });

        // Create a PdfStringFormat object and set the character spacing to 5f
        let format = spirepdf.PdfStringFormat.Create();
        format.CharacterSpacing = 5;

        // Draw the string "E-ICEBLUE" on the page using a specified font, pen, position, and format
        let font = spirepdf.PdfFont.Create({ fontFamily: spirepdf.PdfFontFamily.Helvetica, size: 45 });
        page.Canvas.DrawString({ s: "E-ICEBLUE", font: font, pen: pen, x: 0, y: 500, format: format });

        // Restore the graphics state to its previous state
        page.Canvas.Restore({ state: state });

        // Define the output file name
        const outputFileName = "FillStrokeText.pdf";

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
