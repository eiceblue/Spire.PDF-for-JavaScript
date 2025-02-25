<template>
  <span>The following example shows how to launch pdf file in new window</span>
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
        const inputFileName = "DocumentsLinks.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a new PDF document object
        let doc = wasmModule.PdfDocument.Create();
        // Load an existing PDF document from VFS
        doc.LoadFromFile({ fileName: inputFileName });

        let collection = null;
        let test = ["Spire.PDF"];

        // Iterate through each page of the PDF document
        for (let pageIndex = 0; pageIndex < doc.Pages.Count; pageIndex++) {
          let page = doc.Pages.get_Item(pageIndex);
          // Iterate through each keyword in the 'test' array
          for (let i = 0; i < test.length; i++) {
            // Create a PdfTextFinder to search for the keyword in the current page.
            let finder = spirepdf.PdfTextFinder.Create(page);
            finder.Options.Parameter = spirepdf.TextFindParameter.WholeWord;

            // Find all occurrences of the keyword on the current page.
            collection = finder.Find(test[i]);

            // Iterate through each found text fragment.
            for (let m = 0; m < collection.length; m++) {
              let find = collection[m];
              // Create a PdfLaunchAction to launch a file when the annotation is clicked.
              const inputFileName = "Sample.pdf";
              wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);
              let launchAction = spirepdf.PdfLaunchAction.Create({
                fileName: inputFileName,
                path: spirepdf.PdfFilePathType.Relative
              });

              // Set the launch action to open the file in a new window.
              launchAction.IsNewWindow = true;

              // Get the position and size of the found text fragment
              let rect = spirepdf.RectangleF.Create({
                x: find.Positions[0].X,
                y: find.Positions[0].Y,
                width: find.Sizes[0].Width,
                height: find.Sizes[0].Height
              });

              // Create a PdfActionAnnotation with the launch action and the annotation rectangle.
              let annotation = spirepdf.PdfActionAnnotation.Create(rect, launchAction);

              // Add the annotation to the current page.
              page.Annotations.Add(annotation);
            }
          }
        }

        // Define the output file name
        const outputFileName = "LaunchFileInNewWindow.pdf";

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
