<template>
  <span>The following example demonstrates how to add image watermark in PDF document</span>
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
        const inputImageName1 = "E-logo.png";
        await wasmModule.FetchFileToVFS(inputImageName1, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a new PDF document object.
        let doc = wasmModule.PdfDocument.Create();
        // Load an existing PDF document from a file.
        doc.LoadFromFile({ fileName: inputFileName });

        // Load an image from a file.
        let image = spirepdf.PdfImage.FromFile(inputImageName1);

        // Get the first page from the document.
        let page = doc.Pages.get_Item(0);

        // Specify the position on the page to insert the image.
        let position = spirepdf.PointF.Create({ x: 160, y: 260 });

        // Save the current state of the canvas and set transparency for the image.
        page.Canvas.Save();
        page.Canvas.SetTransparency({ alphaPen: 0.5, alphaBrush: 0.5, blendMode: spirepdf.PdfBlendMode.Multiply });

        // Draw the image on the page using the specified position.
        page.Canvas.DrawImage({ image: image, point: position });

        // Restore the previous state of the canvas.
        page.Canvas.Restore();

        // Define the output file name
        const outputFileName = "ImageWatermarkSecond.pdf";

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
