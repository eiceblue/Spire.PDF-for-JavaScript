<template>
  <span>The following example demonstrates how to get Pdf page information.</span>
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
        let inputFileName = "GetPageInfo.pdf";
        await wasmModule.FetchFileToVFS(inputFileName,"",`${import.meta.env.BASE_URL}static/data/`);
        //Create a PPT document
        let doc = wasmModule.PdfDocument.Create();
	      doc.LoadFromFile(inputFileName);

        // Define the output file name
        const outputFileName = "GetPageInfo_out.txt";

        let page = doc.Pages.get_Item(0)

        //Get the size of page MediaBox based on "point"
        let MediaBoxWidth = page.MediaBox.Width
        let MediaBoxHeight = page.MediaBox.Height
        let MediaBoxX = page.MediaBox.X
        let MediaBoxY = page.MediaBox.Y
        //Get the size of page BleedBox based on "point"
        let BleedBoxWidth = page.BleedBox.Width
        let BleedBoxHeight = page.BleedBox.Height
        let BleedBoxX = page.BleedBox.X
        let BleedBoxY = page.BleedBox.Y
        //Get the size of page CropBox based on "point"
        let CropBoxWidth = page.CropBox.Width
        let CropBoxHeight = page.CropBox.Height
        let CropBoxX = page.CropBox.X
        let CropBoxY = page.CropBox.Y
        //Get the size of page ArtBox based on "point"
        let ArtBoxWidth = page.ArtBox.Width
        let ArtBoxHeight = page.ArtBox.Height
        let ArtBoxX = page.ArtBox.X
        let ArtBoxY = page.ArtBox.Y
        //Get the size of page TrimBox based on "point"
        let TrimBoxWidth = page.TrimBox.Width
        let TrimBoxHeight = page.TrimBox.Height
        let TrimBoxX = page.TrimBox.X
        let TrimBoxY = page.TrimBox.Y
        //Get the actual size of page
        let actualSizeW = page.ActualSize.Width
        let actualSizeH = page.ActualSize.Height
        //Gets the rotation angle of the current page
        let rotationAngle = page.Rotation
        let rotation = rotationAngle.toString();
        //Create StringBuilder to save
        let content = "";
        //Add page information string to StringBuilder
        content += "MediaBox width: " + MediaBoxWidth.toString() + "pt, height: " + MediaBoxHeight.toString() +
            "pt, RectangleF X: " + MediaBoxX.toString() + "pt, RectangleF Y: " + MediaBoxY.toString() + "pt." + "\n";
        content += "MediaBox width: " + BleedBoxWidth.toString() + "pt, height: " + BleedBoxHeight.toString() +
            "pt, RectangleF X: " + BleedBoxX.toString() + "pt, RectangleF Y: " + BleedBoxY.toString() + "pt." + "\n";
        content += "MediaBox width: " + CropBoxWidth.toString() + "pt, height: " + CropBoxHeight.toString() +
            "pt, RectangleF X: " + CropBoxX.toString() + "pt, RectangleF Y: " + CropBoxY.toString() + "pt." + "\n";
        content += "MediaBox width: " + ArtBoxWidth.toString() + "pt, height: " + ArtBoxHeight.toString() +
            "pt, RectangleF X: " + ArtBoxX.toString() + "pt, RectangleF Y: " + ArtBoxY.toString() + "pt." + "\n";
        content += "MediaBox width: " + TrimBoxWidth.toString() + "pt, height: " + TrimBoxHeight.toString() +
            "pt, RectangleF X: " + TrimBoxX.toString() + "pt, RectangleF Y: " + TrimBoxY.toString() + "pt." + "\n";
        content += "The actual size of the current page width: " + actualSizeW.toString() + "\n";
        content += "The actual size of the current page height: " + actualSizeH.toString() + "\n";
        content += "The rotation angle of the current page: " + rotation;

        wasmModule.FS.writeFile(outputFileName, content);
        doc.Close();


        // Read the saved file and convert to a Blob object
        const modifiedFileArray = wasmModule.FS.readFile(outputFileName);
        const modifiedFile = new Blob([modifiedFileArray], { type: "text/plain" });

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
