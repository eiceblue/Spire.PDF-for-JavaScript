<template>
  <span>The following example demonstrates how to convert text to PDF document.</span>
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
        let inputFileName = "TextToPdf.txt";
        await wasmModule.FetchFileToVFS(inputFileName,"",`${import.meta.env.BASE_URL}static/data/`);

        // Get text from .txt file.
        let textByte = wasmModule.FS.readFile(inputFileName);
        const decoder = new TextDecoder('utf-8');
        let text = decoder.decode(textByte);
        //Create a  document
        let doc = wasmModule.PdfDocument.Create();
        // Add a section to the document.
        let section = doc.Sections.Add();

        // Add a page to the section.
        let page = section.Pages.Add();

        // Create a PdfFont object using the Helvetica font with a size of 11.
        let font = wasmModule.PdfFont.Create({fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 11});

        // Create a PdfStringFormat object for text formatting.
        let format = wasmModule.PdfStringFormat.Create();
        format.LineSpacing = 20;

        // Create a PdfBrush object for text color.
        let brush = wasmModule.PdfBrushes.get_Black();

        // Create a PdfTextLayout object for text layout options.
        let textLayout = wasmModule.PdfTextLayout.Create();
        textLayout.Break = wasmModule.PdfLayoutBreakType.FitPage;
        textLayout.Layout = wasmModule.PdfLayoutType.Paginate;

        // Define the bounds of the text widget on the page.
        let bounds = wasmModule.RectangleF.Create({
            location: wasmModule.PointF.Create({x: 10, y: 20}),
            size: page.Canvas.ClientSize
        });

        // Create a PdfTextWidget object with the specified text, font, and brush.
        let textWidget = wasmModule.PdfTextWidget.Create({text: text, font: font, brush: brush});
        textWidget.StringFormat = format;

        // Draw the text widget on the page using the specified bounds and text layout options.
        let layoutWidget = new wasmModule.PdfLayoutWidget(textWidget.H);
        layoutWidget.Draw({page: page, layoutRectangle: bounds, format: textLayout});

        // Define the output file name
        const outputFileName = "TextToPdf_result.pdf";

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
