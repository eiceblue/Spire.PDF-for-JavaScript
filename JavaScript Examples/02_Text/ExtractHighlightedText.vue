<template>
  <span>The following example demonstrates how to extract highlighted text.</span>
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
        let inputFileName = 'ExtractHighlightedText.pdf';
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);

        // Create a pdf instance
        let doc = wasmModule.PdfDocument.Create();

        //Load a pdf file
        doc.LoadFromFile({fileName: inputFileName});    
        let page = doc.Pages.get_Item(0);

        let textMarkupAnnotation;
        let outContent = "Extracted hightlighted text:";
        let pdfTextExtractor = spirepdf.PdfTextExtractor.Create(page);

        //Get PdfTextMarkupAnnotationWidget objects
        for (let i = 0; i < page.Annotations.Count; i++) {
            if (page.Annotations.get_Item(i) instanceof wasmModule.PdfTextMarkupAnnotationWidget) {
                textMarkupAnnotation = page.Annotations.get_Item(i);
                //Get the highlighted text
                let pdfTextExtractOptions = wasmModule.PdfTextExtractOptions.Create();
                pdfTextExtractOptions.ExtractArea = textMarkupAnnotation.Bounds;
                outContent = outContent + pdfTextExtractor.ExtractText(pdfTextExtractOptions);

                //Get the highlighted color
                let color = textMarkupAnnotation.TextMarkupColor;
            }
        }
        // Define the output file name
        const outputFileName = "ExtractHighlightedText_out.txt";
        wasmModule.FS.writeFile(outputFileName, outContent);
        doc.Close();

        // Read the saved file and convert to a Blob object
        const modifiedFileArray = wasmModule.FS.readFile(outputFileName);
        const modifiedFile = new Blob([modifiedFileArray], { type: "text/plain"});

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
