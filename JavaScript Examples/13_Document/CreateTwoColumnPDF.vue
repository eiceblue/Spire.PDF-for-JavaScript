<template>
  <span>The following example demonstrates how to create a PDF document that has two columns.</span>
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

        //Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

         // Creates a new page
        let page = doc.Pages.Add()
        let s1 = "Spire.PDF is a professional PDF component applied to creating, writing, " + "editing, handling and reading PDF files without any external dependencies within " + ".NET application. Using this .NET PDF library, you can implement rich capabilities " + "to create PDF files from scratch or process existing PDF documents entirely through " + "C#/VB.NET without installing Adobe Acrobat.";
        let s2 = "Many rich features can be supported by the API, such as security setting " + "(including digital signature), PDF text/attachment/image extract, PDF merge/split " + ", metadata update, section, graph/image drawing and inserting, table creation and " + "processing, and importing data etc.Besides, Spire.PDF for .NET can be applied to easily " + "converting Text, Image and HTML to PDF with C#/VB.NET in high quality.";
        // Get width and height of page
        let pageWidth = page.GetClientSize().Width;
        let pageHeight = page.GetClientSize().Height;
        let format =  wasmModule. PdfStringFormat.Create({alignment:wasmModule. PdfTextAlignment.Justify});
        let brush = wasmModule.PdfBrushes.get_Black()
        let font = wasmModule.PdfFont.Create({fontFamily:wasmModule.PdfFontFamily.TimesRoman, size:20});

        // Draw text
        let rect1 = wasmModule.RectangleF.Create({x: 0, y: 20, width: pageWidth / 2 - 8, height: pageHeight});
        let rect2 = wasmModule.RectangleF.Create({x: pageWidth / 2 + 8, y: 20, width: pageWidth / 2 - 8, height: pageHeight});
        page.Canvas.DrawString({s:s1, font:font, brush:brush,layoutRectangle:rect1, format:format});//s, font, brush, layoutRectangle, format
        page.Canvas.DrawString({s:s2, font:font, brush:brush,layoutRectangle:rect2, format:format});

        // Define the output file name
        const outputFileName = "CreateTwoColumnPDF_result.pdf";

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
