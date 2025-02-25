<template>
  <span>The following example demonstrates how to get the size of PDF pages.</span>
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
        let inputFileName = "Sample.pdf";
        await wasmModule.FetchFileToVFS(inputFileName,"",`${import.meta.env.BASE_URL}static/data/`);
        //Create a PPT document
        let doc = wasmModule.PdfDocument.Create();
	      doc.LoadFromFile(inputFileName);

        let page = doc.Pages.get_Item(0);

        //Get the width of page based on "point"
        let pointWidth = page.Size.Width;
        //Get the height of page
        let pointHeight = page.Size.Height;
        //Create PdfUnitConvertor to convert the unit
        let unitCvtr = wasmModule.PdfUnitConvertor.Create();
        //Convert the size with "pixel"
        let pixelWidth = unitCvtr.ConvertUnits(
            pointWidth, wasmModule.PdfGraphicsUnit.Point, wasmModule.PdfGraphicsUnit.Pixel);
        let pixelHeight = unitCvtr.ConvertUnits(
            pointHeight, wasmModule.PdfGraphicsUnit.Point, wasmModule.PdfGraphicsUnit.Pixel);
        //Convert the size with "inch"
        let inchWidth = unitCvtr.ConvertUnits(
            pointWidth, wasmModule.PdfGraphicsUnit.Point, wasmModule.PdfGraphicsUnit.Inch);
        let inchHeight = unitCvtr.ConvertUnits(
            pointHeight, wasmModule.PdfGraphicsUnit.Point, wasmModule.PdfGraphicsUnit.Inch);
        //Convert the size with "centimeter"
        let centimeterWidth = unitCvtr.ConvertUnits(
            pointWidth, wasmModule.PdfGraphicsUnit.Point, wasmModule.PdfGraphicsUnit.Centimeter);
        let centimeterHeight = unitCvtr.ConvertUnits(
            pointHeight, wasmModule.PdfGraphicsUnit.Point, wasmModule.PdfGraphicsUnit.Centimeter);
        //Create StringBuilder to save
        let content = "";
        //Add pointSize string to StringBuilder
        content += "The page size of the file is (width: " +
            pointWidth.toString() + "pt, height: " + pointHeight.toString() + "pt)." + "\n";
        content += "The page size of the file is (width: " +
            pixelWidth.toString() + "pixel, height: " + pixelHeight.toString() + "pixel)." + "\n";
        content += "The page size of the file is (width: " +
            inchWidth.toString() + "inch, height: " + inchHeight.toString() + "inch)." + "\n";
        content += "The page size of the file is (width: " +
            centimeterWidth.toString() + "cm, height: " + centimeterHeight.toString() + "cm.)";

          
        // Define the output file name
        const outputFileName = "GetPageSize_out.txt";
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
