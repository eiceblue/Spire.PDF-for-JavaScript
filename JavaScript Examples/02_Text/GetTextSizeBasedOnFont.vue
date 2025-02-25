<template>
  <span>The following example demonstrates how to get the text size based on the font.</span>
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

        let stringB = "";
        let text = "Spire.PDF";

        //Create an instance for PdfFont
        let font1 = new wasmModule.PdfFont.Create({fontFamily: wasmModule.PdfFontFamily.TimesRoman, size: 12});

        //Get text size based on font name and size
        let sizeF1 = font1.MeasureString(text);
        stringB = "1. The width is: " + sizeF1.Width + ", the height is: " + sizeF1.Height;

        let font2 = wasmModule.PdfTrueTypeFont.Create({
            fontName: 'Arial Unicode MS',
            emSize: 12,
            style: wasmModule.FontStyle.Regular,
            unicode:true
        });

        //Get text size based on font name and size
        let sizeF2 = font2.MeasureString({text: text});
        stringB = stringB + "2. The width is: " + sizeF2.Width + ", the height is: " + sizeF2.Height;

        // Define the output file name
        const outputFileName = "GetTextSizeBasedOnFont_out.txt";
        wasmModule.FS.writeFile(outputFileName, stringB);

        // Read the saved file and convert to a Blob object
        const modifiedFileArray = wasmModule.FS.readFile(outputFileName);
        const modifiedFile = new Blob([modifiedFileArray], { type:  "text/plain" });

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
