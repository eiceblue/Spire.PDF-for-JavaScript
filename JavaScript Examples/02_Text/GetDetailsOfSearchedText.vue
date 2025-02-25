<template>
  <span>The following example demonstrates how to get details of the searched text. </span>
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
        let results = finder.Find("Spire.PDF");

        // Create a StringBuilder object to store the details of the searched text
        let builder = "";

        // Iterate through each found text fragment
        for (let i = 0; i < results.length; i++) {
            let find = results[i];
            builder = builder + "==================================================================================" + "\r\n";
            // Append the matched text and the detail of matched text to the StringBuilder
            builder = builder + "Match Text: " + find.Text + "\r\n";
            builder = builder + "Size: " + find.Sizes[0].Width + "," + find.Sizes[0].Height + "\r\n";
            builder = builder + "Position: " + find.Positions[0].X + "," + find.Positions[0].Y + "\r\n";
            builder = builder + "The line that contains the searched text: " + find.LineText + "\r\n";
        }


        // Define the output file name
        const outputFileName = "GetDetailsOfSearchedText_out.txt";
        wasmModule.FS.writeFile(outputFileName, builder);
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
