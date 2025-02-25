<template>
  <span>The following example demonstrates how to draw text with gradient color.</span>
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
         // Create one page
        let page = doc.Pages.Add();
        
        //Create a rectangle
        let pointf=  wasmModule.PointF.Create({x:0,y:0});
        let sizef= wasmModule.SizeF.Create({width:300,height:100});
        let rectf = wasmModule.RectangleF.Create({location:pointf,size:sizef});

        //Create a brush with gradient
        let  color1= wasmModule.PdfRGBColor.Create({color:wasmModule.Color.get_Red()});
        let  color2= wasmModule.PdfRGBColor.Create({color:wasmModule.Color.get_Blue()});
        let horizontal= wasmModule.PdfLinearGradientMode.Horizontal;
        let brush = wasmModule.PdfLinearGradientBrush.Create({rect:rectf,color1:color1,color2:color2,mode:horizontal});

        //Create a true type font with size 20f, underline style
        let font = wasmModule.PdfFont.Create(wasmModule.PdfFontFamily.Helvetica, 20);

        //Draw text
        let textpoint=  wasmModule.PointF.Create({x:0,y:0});
        page.Canvas.DrawString({s:"Welcome to E-iceblue!", font:font, brush:brush, point:textpoint});

        // Define the output file name
        const outputFileName = "DrawTextWithGradient_out.pdf";

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
