<template>
  <span>The following example demonstrates how to set line break. </span>
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
        let page = doc.Pages.Add({size: wasmModule.PdfPageSize.A4(), margins: wasmModule.PdfMargins.Create(40)});
        // Create brush from color channel
        let brush = wasmModule.PdfSolidBrush.Create({color:wasmModule.Color.get_Black()});

        // Create text
        let text = "Spire.PDF for JavaScript" +
            "\n" +
            "A professional PDF library applied to" +
            " creating, writing, editing, handling and reading PDF files" +
            " without any external dependencies.";

        text += "\n\rSpire.PDF for Java" +
            "\n" +
            "A PDF Java API that enables developers to read, " +
            "write, convert and print PDF documents" +
            "in Java applications without using Adobe Acrobat.";
        text += "\n\r";
        text += "Welcome to evaluate Spire.PDF!";

        // Create rectangle with specified dimensions
        let rect = wasmModule.RectangleF.Create({x:50, y:50, width: page.Size.Width - 150,height: page.Size.Height});
        let  font=wasmModule.PdfFont.Create({fontFamily:wasmModule.PdfFontFamily.Helvetica,size:13});
        // Draw the text s, font, brush, layoutRectangle
        page.Canvas.DrawString({s:text, font:font, brush:brush,layoutRectangle: rect});

        // Define the output file name
        const outputFileName = "SetLineBreak_out.pdf";

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
