<template>
  <span>The following example demonstrates how to search text and add hyperlink in PDF document. </span>
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

        // Get the first page of the pdf file
        let page = doc.Pages.get_Item(0);

        // Create a PdfTextFinder using the first page
        let finder = wasmModule.PdfTextFinder.Create(page);

        // Set the search parameter to ignore case
        finder.Options.Parameter = wasmModule.TextFindParameter.IgnoreCase;

        // Find all occurrences of "e-iceblue" in the PDF and store them in a list
        let finds = finder.Find("e-iceblue");

        // Define the hyperlink URL
        let url = "http://www.e-iceblue.com";

        // Iterate through each found text fragment
        for (let i = 0; i < finds.length; i++) {
            // Create a PdfUriAnnotation object to add a hyperlink for the searched text
            let find = finds[i];
            let uri = wasmModule.PdfUriAnnotation.Create({rectangle: find.Bounds[0]});
            uri.Uri = url;
            uri.Border = wasmModule.PdfAnnotationBorder.Create({borderWidth: 1});
            uri.Color = wasmModule.PdfRGBColor.Create({color: spirepdf.Color.get_Blue()});

            // Add the annotation to the page's annotation widget
            page.Annotations.Add(uri);
        }
        
        // Define the output file name
        const outputFileName = "SearchTextAndAddHyperlink_out.pdf";

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
