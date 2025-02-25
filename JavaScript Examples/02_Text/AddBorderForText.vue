<template>
  <span>The following example demonstrates how to add border for text in PDF document. </span>
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

        // Create a pdf instance
        let doc = wasmModule.PdfDocument.Create();
         // Create one page
        let pagebase = doc.Pages.Add();
        
        const text = "Hello World";

        //Create the font to use and set the font style 
        let trueTypeFont = wasmModule.PdfTrueTypeFont.Create({
            fontName: 'Arial Unicode MS',
            emSize: 12,
            style: wasmModule.FontStyle.Regular,
            unicode:true
        });

        //Measure the size of the text
        let size = trueTypeFont.MeasureString({text:text});

         //Create a PdfSolidBrush instance for setting the color of text
        let pdfRGBColor =wasmModule.PdfRGBColor.Create({red: 200, green: 100, blue: 0});
        let pdfBrush = wasmModule.PdfSolidBrush.Create({pdfRGBColor:pdfRGBColor});
        let x = 60;
        let y = 100;

         //Draw the text on page
        pagebase.Canvas.DrawString({s: text, font: trueTypeFont, brush: pdfBrush, x: x, y: y});
  
        //Draw border for text   
        let pen = wasmModule.PdfPen.Create({pdfRgbColor:pdfRGBColor, width:1});
        let rect = wasmModule.RectangleF.Create({x: x, y: y, width: size.Width, height: size.Height});
        pagebase.Canvas.DrawRectangle({pen: pen, rectangle: rect});

        // Define the output file name
        const outputFileName = "AddBorderForText_out.pdf";

        // Save the document to the specified path
        doc.SaveToFile({fileName: outputFileName});
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
