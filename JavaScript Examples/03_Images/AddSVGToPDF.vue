<template>
  <span>The following example demonstrates how to add SVG to an existing PDF file.</span>
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
        let inputName1 = "AddSVGToPDF.pdf";
        await wasmModule.FetchFileToVFS(inputName1, '', `${import.meta.env.BASE_URL}static/data/`);

        // Load the sample file into the virtual file system (VFS)
        let inputName2 = "AddSVGToPDF.svg";
        await wasmModule.FetchFileToVFS(inputName2, '', `${import.meta.env.BASE_URL}static/data/`);

        let existingPDF = wasmModule.PdfDocument.Create();
        existingPDF.LoadFromFile({fileName: inputName1});

        // Create a pdf instance
        let doc = wasmModule.PdfDocument.Create();
        doc.LoadFromSvg({fileName: inputName2});

        //Create template
        let template = doc.Pages.get_Item(0).CreateTemplate();

        //Draw template on existing PDF
        let point = wasmModule.PointF.Create({x: 50, y: 250});
        let size = wasmModule.SizeF.Create({width: 200, height: 200});
        existingPDF.Pages.get_Item(0).Canvas.DrawTemplate({template: template, location: point, size: size});

        // Define the output file name
        const outputFileName = "AddSVGToPDF_out.pdf";

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
