<template>
  <span>The following example demonstrates how to replace all searched text in PDF document. </span>
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

        // Load the sample file into the virtual file system (VFS)
        let inputFileName = 'SearchReplaceTemplate.pdf';
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);

        // Create a pdf instance
        let doc = wasmModule.PdfDocument.Create();

        //Load a pdf file
        doc.LoadFromFile({fileName: inputFileName});

        let page = doc.Pages.get_Item(0);

        // Create a PdfTextFinder object for searching text within the page
        let finder = wasmModule.PdfTextFinder.Create(page);
        finder.Options.Parameter = wasmModule.TextFindParameter.IgnoreCase;

        // Find occurrences of the specified text within the page
        let finds = finder.Find("Spire.PDF for JavaScript");

        let newText = "E-iceblue Spire.PDF";

        // Creates a brush
        let brush = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_DarkBlue()});

        let font2 = wasmModule.PdfTrueTypeFont.Create({
            fontName: 'Arial Unicode MS',
            emSize: 9,
            style: wasmModule.FontStyle.Regular,
            unicode:true
        });

        let rec;

        // Iterate through each found text fragment
        for (let i = 0; i < finds.length; i++) {
            let find = finds[i];
            // Gets the bound of the found text in page
            rec = find.Bounds[0];

            page.Canvas.DrawRectangle({brush: wasmModule.PdfBrushes.get_White(), rectangle: rec});
            // Draws new text as defined font and color
            page.Canvas.DrawString({s: newText, font: font2, brush: brush, layoutRectangle: rec});

            // This method can directly replace old text with newText,but it just can set the background color, can not set font/forecolor
            // find.ApplyRecoverString(newText, Color.Gray);
        }

        // Define the output file name
        const outputFileName = "ReplaceAllSearchedText_out.pdf";

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
