<template>
  <span>The following example demonstrates how to embed sound file in PDF document</span>
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

        // Load the input file into the virtual file system (VFS)
        const inputFileName = "EmbedSoundFile.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);
        const inputFileName1 = "Music.wav";
        await wasmModule.FetchFileToVFS(inputFileName1, "", `${import.meta.env.BASE_URL}static/data/`);

        
        // Create a new PDF document object.
        let doc = wasmModule.PdfDocument.Create();
        // Load an existing PDF document from a file.
        doc.LoadFromFile({ fileName: inputFileName });

        // Get the first page of the loaded document.
        let page = doc.Pages.get_Item(0);

        // Create a sound action with the specified sound file.
        let soundAction = spirepdf.PdfSoundAction.Create(inputFileName1);

        // Set properties for the sound action.
        soundAction.Sound.Bits = 1;
        soundAction.Sound.Channels = spirepdf.PdfSoundChannels.Stereo;
        soundAction.Sound.Encoding = spirepdf.PdfSoundEncoding.Signed;
        soundAction.Volume = 0.8;
        soundAction.Repeat = true;

        // Set the sound action to be executed when the PDF document is opened.
        doc.AfterOpenAction = soundAction;

        // Define the output file name
        const outputFileName = "EmbedSoundFile.pdf";

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
