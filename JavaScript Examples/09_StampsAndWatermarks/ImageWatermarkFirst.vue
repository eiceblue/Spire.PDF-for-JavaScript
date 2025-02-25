<template>
  <span>The following example demonstrates how to add image watermark in PDF document</span>
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
        const inputFileName = "ImageWaterMark.pdf";

        // Load the input file into the virtual file system (VFS)
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);
        const inputImageName1 = "Background.png";
        await wasmModule.FetchFileToVFS(inputImageName1, "", `${import.meta.env.BASE_URL}static/data/`);


        // Create a new PDF document object.
        let doc = wasmModule.PdfDocument.Create();
        // Load an existing PDF document from VFS
        doc.LoadFromFile({ fileName: inputFileName });

        // Get the first page of the loaded document
        let page = doc.Pages.get_Item(0);

        // Load the image from a file
        let stream = spirepdf.Stream.CreateByFile(inputImageName1);

        // Set the loaded image as the background image of the page
        page.BackgroundImage = stream;

        // Define the output file name
        const outputFileName = "ImageWatermarkFirst.pdf";

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
