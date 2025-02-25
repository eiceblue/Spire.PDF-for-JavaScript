<template>
  <span>The following example demonstrates how to set transparency in PDF document.</span>
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

        // Load the sample image into the virtual file system (VFS)
        let inputFileName = "ChartImage.png";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        // Add a new section to the document
        let section = doc.Sections.Add();

        // Load an image from the specified file
        let image = wasmModule.PdfImage.FromFile(inputFileName);
        let imageWidth = image.PhysicalDimension.Width / 2;
        let imageHeight = image.PhysicalDimension.Height / 2;

        // Define an array of blend modes for transparency effects
        let models = ["Normal", "Multiply", "Screen", "Overlay", "Darken", "Lighten", "ColorDodge", "ColorBurn", "HardLight"
          , "SoftLight", "Difference", "Exclusion", "Hue", "Saturation", "Color", "Luminosity"];

        // Loop through each blend mode to create a new page for each one
        for (let i = 0; i < models.length; i++) {
          let mode = models[i];
          // Add a new page to the section
          let page = section.Pages.Add();
          let pageWidth = page.Canvas.ClientSize.Width;
          let y = 0;

          // Title section
          y = y + 15;
          let brush = wasmModule.PdfSolidBrush.Create({ color: wasmModule.Color.get_OrangeRed() });
          let font = wasmModule.PdfTrueTypeFont.Create({
            fontFile: "/Library/Fonts/ARIALUNI.TTF",
            size: 16,
            style: wasmModule.PdfFontStyle.Bold
          });

          // Create a PdfStringFormat object to center-align the title
          let format = wasmModule.PdfStringFormat.Create({ alignment: wasmModule.PdfTextAlignment.Center });
          let text = "Transparency Blend Mode:" + mode;

          // Draw the title text on the page
          page.Canvas.DrawString({ s: text, font: font, brush: brush, x: pageWidth / 2, y: y, format: format });

          // Measure the size of the title text for positioning
          let size = font.MeasureString({ text: text, format: format });
          y = y + size.Height + 25;

          // Draw the image on the page
          page.Canvas.DrawImage({ image: image, x: 0, y: y, width: imageWidth, height: imageHeight });
          page.Canvas.Save();
          let d = (page.Canvas.ClientSize.Width - imageWidth) / 5;
          let x = d;
          y = y + d / 2 + 40;

          // Draw the imageij multiiple times with varying transparency
          for (let j = 0; j < 5; j++) {
            let alpha = 1.0 / 6 * (5 - j);

            // Set transparency for the canvas
            page.Canvas.SetTransparency({alphaPen: alpha, alphaBrush: alpha, blendMode: wasmModule.PdfBlendMode.fromValue(i)});

            // Draw the image with the current transparency settings
            page.Canvas.DrawImage({ image: image, x: x, y: y, width: imageWidth, height: imageHeight });
            x = x + d;
            y = y + d / 2 + 40;
          }
          // Restore the canvas to the previous state
          page.Canvas.Restore();
        }

        // Define the output file name
        const outputFileName = "Transparency_result.pdf";

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
