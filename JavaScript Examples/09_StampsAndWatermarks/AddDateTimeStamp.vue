<template>
  <span>The following example demonstrates how to add datetime stamp in PDF document</span>
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
        await wasmModule.FetchFileToVFS("ARIALUNI.TTF", "", `${import.meta.env.BASE_URL}static/font/`);

        // Load the input file into the virtual file system (VFS)
        const inputFileName = "PDFTemplate_N.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a new PDF document object.
        let doc = wasmModule.PdfDocument.Create();
        // Load an existing PDF document from a file.
        doc.LoadFromFile({ fileName: inputFileName });

        // Get the first page of the loaded document.
        let page = doc.Pages.get_Item(0);

        // Set the font and brush for the text
        let trueTypeFont = wasmModule.PdfTrueTypeFont.Create({
          fontFile: "ARIALUNI.TTF",
          size: 12,
          style: spirepdf.PdfFontStyle.Regular
        });
        let font = trueTypeFont;
        let brush = spirepdf.PdfSolidBrush.Create({ color: spirepdf.Color.get_Blue() });

        // Generate a string representing the current date and time
        let timeString = spirepdf.DateTime.get_Now().ToString({ format: "MM/dd/yy hh:mm:ss tt " });

        // Create a template with specified dimensions
        let template = spirepdf.PdfTemplate.Create({ width: 140, height: 15 });

        // Define a rectangle to position the template on the page
        let point = spirepdf.PointF.Create({
          x: page.ActualSize.Width - template.Width - 10,
          y: page.ActualSize.Height - template.Height - 10
        });
        let rect = spirepdf.RectangleF.Create({ location: point, size: template.Size });

        // Draw the time string onto the template
        template.Graphics.DrawString({
          s: timeString,
          font: font,
          brush: brush,
          point: spirepdf.PointF.Create({ x: 0, y: 0 })
        });

        // Create a rubber stamp annotation
        let stamp = spirepdf.PdfRubberStampAnnotation.Create({ rectangle: rect });

        // Create a custom appearance for the stamp annotation
        let appearance = spirepdf.PdfAppearance.Create(stamp);
        appearance.Normal = template;

        // Assign the custom appearance to the stamp annotation
        stamp.Appearance = appearance;

        // Add the stamp annotation to the page's annotations widget
        page.AnnotationsWidget.Add(stamp);

        // Define the output file name
        const outputFileName = "AddDateTimeStamp.pdf";

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
