<template>
  <span>The following example demonstrates how to create multilayer PDF.</span>
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
  
        let imgFileName = 'MultilayerImage.png';
        await wasmModule.FetchFileToVFS(imgFileName, '', `${import.meta.env.BASE_URL}static/data/`);

        //Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        //Creates a page
        let page = doc.Pages.Add();
        //Create text
        let text = "Welcome to evaluate Spire.PDF for javascript !";
        //let format1 = wasmModule.PdfStringFormat.Create(wasmModule.PdfTextAlignment.Left);
        let format1 =  wasmModule. PdfStringFormat.Create({alignment:wasmModule.PdfTextAlignment.Left});
        let brush = wasmModule.PdfSolidBrush.Create({color:wasmModule.Color.get_Black()});
        let font = wasmModule.PdfTrueTypeFont.Create({fontFile: "/Library/Fonts/ARIALUNI.TTF",size: 15,style: wasmModule.PdfFontStyle.Regular});

        let x = 50.0;
        let y = 50.0;
        //Draw text layer
        page.Canvas.DrawString({s:text, font:font, brush:brush,point:wasmModule.PointF.Create({x:x,y:y}), format:format1});
        // Get the size of text
        let size = font.MeasureString({text:"Welcome to  evaluate",format: format1});
        let size2 = font.MeasureString({text:"Spire.PDF for Python",format: format1});

        //Loads an image
        let image = wasmModule.PdfImage.FromFile(imgFileName);
        //Draw image layer
        page.Canvas.DrawImage({image:image,point: wasmModule.PointF.Create({x:x + size.Width,y: y}),size:size2});

        // Define the output file name
        const outputFileName = "CreateMultilayerPDF_result.pdf";

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
