<template>
  <span>The following example demonstrates how to set PDF page orientation according to image size.</span>
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
        let inputFileName = 'scenery.jpg';
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);

        //Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        //Add a section
        let section = doc.Sections.Add()
        
        //Load a image
        let image = wasmModule.PdfImage.FromFile(inputFileName)
        //Check whether the width of the image file is greater than default page width or not
        if (image.PhysicalDimension.Width > section.PageSettings.Size.Width) {
            //Set the orientation as landscape
            section.PageSettings.Orientation = wasmModule.PdfPageOrientation.Landscape;
        } else {
            section.PageSettings.Orientation = wasmModule.PdfPageOrientation.Portrait;
        }
        //Add a new page with orientation Landscape
        let page = section.Pages.Add();
        //Draw the image on the page
        page.Canvas.DrawImage({image:image, point:wasmModule.PointF.Create({x:0,y:0})});

        // Define the output file name
        const outputFileName = "SetPageOrientation_result.pdf";

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
