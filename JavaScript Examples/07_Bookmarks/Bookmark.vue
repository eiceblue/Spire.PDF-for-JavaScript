<template>
  <span>The following example demonstrates how to add bookmark to a PDF document.</span>
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
        //Create a PdfAttachment object
        let doc = wasmModule.PdfDocument.Create();

        //Add page
        let page1 = doc.Pages.Add();

        //Define font and color
        let font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 20 });
        let color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_SaddleBrown() });

        //the first node
        let vendorTitle11 = "1.1";

        // Create a destination for the vendor bookmark
        let point = wasmModule.PointF.Create({ x: 150, y: 150 });
        page1.Canvas.DrawString({ s: vendorTitle11, font: font, brush: wasmModule.PdfBrushes.get_Red(), point: point });
        let vendorBookmarkDest11 = wasmModule.PdfDestination.Create({
          page: page1,
          location: wasmModule.PointF.Create({ x: 0, y: 50 })
        });
        // Create a bookmark for the vendor and set its properties
        let vendorBookmark11 = doc.Bookmarks.Add(vendorTitle11);
        vendorBookmark11.Color = color;
        vendorBookmark11.DisplayStyle = wasmModule.PdfTextStyle.Bold;
        vendorBookmark11.Action = wasmModule.PdfGoToAction.Create({ destination: vendorBookmarkDest11 });

        let vendorTitle12 = "1.2";
        let page2 = doc.Pages.Add();
        page2.Canvas.DrawString({ s: vendorTitle12, font: font, brush: wasmModule.PdfBrushes.get_Red(), point: point });
        let vendorBookmarkDest12 = wasmModule.PdfDestination.Create({
          page: page2,
          location: wasmModule.PointF.Create({ x: 0, y: 50 })
        });
        let vendorBookmark12 = vendorBookmark11.Add(vendorTitle12);
        vendorBookmark12.Color = color;
        vendorBookmark12.DisplayStyle = wasmModule.PdfTextStyle.Bold;
        vendorBookmark12.Action = wasmModule.PdfGoToAction.Create({ destination: vendorBookmarkDest12 });

        //the second node
        let vendorTitle21 = "2.1";
        let page3 = doc.Pages.Add();
        page3.Canvas.DrawString({ s: vendorTitle21, font: font, brush: wasmModule.PdfBrushes.get_Red(), point: point });
        // Create a destination for the vendor bookmark
        let vendorBookmarkDest21 = wasmModule.PdfDestination.Create({
          page: page3,
          location: wasmModule.PointF.Create({ x: 0, y: 50 })
        });
        // Create a bookmark for the vendor and set its properties
        let vendorBookmark21 = doc.Bookmarks.Add(vendorTitle21);
        vendorBookmark21.Color = color;
        vendorBookmark21.DisplayStyle = wasmModule.PdfTextStyle.Bold;
        vendorBookmark21.Action = wasmModule.PdfGoToAction.Create({ destination: vendorBookmarkDest21 });

        let vendorTitle22 = "2.2";
        let page4 = doc.Pages.Add();
        page4.Canvas.DrawString({ s: vendorTitle22, font: font, brush: wasmModule.PdfBrushes.get_Red(), point: point });
        let vendorBookmarkDest22 = wasmModule.PdfDestination.Create({
          page: page4,
          location: wasmModule.PointF.Create({ x: 0, y: 50 })
        });
        let vendorBookmark22 = vendorBookmark21.Add(vendorTitle22);
        vendorBookmark22.Color = color;
        vendorBookmark22.DisplayStyle = wasmModule.PdfTextStyle.Bold;
        vendorBookmark22.Action = wasmModule.PdfGoToAction.Create({ destination: vendorBookmarkDest22 });

        // Define the output file name
        const outputFileName = "Bookmark_out.pdf";

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
