<template>
  <span>The following example demonstrates how to add transparent text in PDF document.</span>
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

        // Create a pdf instance
        let doc = wasmModule.PdfDocument.Create();

        //Create one A4 page
        let page = doc.Pages.Add({size: wasmModule.PdfPageSize.A4(), margins: wasmModule.PdfMargins.Create(0)});
        page.Canvas.Save();

        //Set transparency for page
        let alpha = 0.25;
        page.Canvas.SetTransparency({alphaPen: alpha, alphaBrush: alpha, blendMode: wasmModule.PdfBlendMode.Normal});

        //Create rectangle with specified dimensions
        let rect = wasmModule.RectangleF.Create({x: 50, y: 50, width: 450, height: page.Size.Height});

        //Create transparent text
        let text = "Spire.PDF for .NET,a professional PDF library applied to" +
            " creating, writing, editing, handling and reading PDF files" +
            " without any external dependencies within .NET" +
            "( C#, VB.NET, ASP.NET, .NET Core) application.";
        text += "\n\n\n\n\n";
        text += "Spire.PDF for Java,a PDF Java API that enables" +
            "developers to read, write, convert and print PDF documents" +
            "in Java applications without using Adobe Acrobat.";

        //Create brush from color channel
        let pdfRGBColor = wasmModule.PdfRGBColor.Create({red: 0, green: 255, blue: 0});
        let brush = wasmModule.PdfSolidBrush.Create({pdfRGBColor: pdfRGBColor});

        //Draw the text
        let font = wasmModule.PdfFont.Create(wasmModule.PdfFontFamily.Helvetica, 14);
        page.Canvas.DrawString({s: text, font: font, brush: brush, layoutRectangle: rect});

        page.Canvas.Restore();

        // Define the output file name
        const outputFileName = "AddTransparentText_out.pdf";

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
