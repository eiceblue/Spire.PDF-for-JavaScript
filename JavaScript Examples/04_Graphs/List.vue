<template>
  <span>The following example demonstrates how to work with list.</span>
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

        // Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        // Create margin settings for the PDF page
        let unitCvtr = wasmModule.PdfUnitConvertor.Create();
        let margin = wasmModule.PdfMargins.Create();
        margin.Top = unitCvtr.ConvertUnits(2.54, wasmModule.PdfGraphicsUnit.Centimeter, wasmModule.PdfGraphicsUnit.Point);
        margin.Bottom = margin.Top;
        margin.Left = unitCvtr.ConvertUnits(3.17, wasmModule.PdfGraphicsUnit.Centimeter, wasmModule.PdfGraphicsUnit.Point);
        margin.Right = margin.Left;
        
        // Create a new A4 page with the specified margins
        let page = doc.Pages.Add({ size: wasmModule.PdfPageSize.A4(), margins: margin });

        let y = 10;

        // Title section
        let brush1 = wasmModule.PdfBrushes.get_Black();
        
        // Create a font for the title
        let font1 = wasmModule.PdfTrueTypeFont.Create({
            fontFile: "/Library/Fonts/ARIALUNI.TTF",
            size: 16,
            style: wasmModule.PdfFontStyle.Bold
        });

        // Create a PdfStringFormat object to define text alignment
        let format1 = wasmModule.PdfStringFormat.Create({ alignment: wasmModule.PdfTextAlignment.Center });

        // Draw the title text on the page
        page.Canvas.DrawString({
          s: "Categories List",
          font: font1,
          brush: brush1,
          x: page.Canvas.ClientSize.Width / 2,
          y: y,
          format: format1
        });

        // Update the vertical position for the next element
        y = y + font1.MeasureString({ text: "Categories List", format: format1 }).Height;
        y = y + 5;

        // Define a rectangle for the background gradient
        let location = wasmModule.PointF.Create({ x: 0, y: 0 });
        let rctg = wasmModule.RectangleF.Create({ location: location, size: page.Canvas.ClientSize });
        let pdfrgbColor1 = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Navy() });
        let pdfrgbColor2 = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_OrangeRed() });

        // Create a linear gradient brush for the rectangle
        let brush = wasmModule.PdfLinearGradientBrush.Create({
          rect: rctg,
          color1: pdfrgbColor1,
          color2: pdfrgbColor2,
          mode: wasmModule.PdfLinearGradientMode.Vertical
        });
        
        // Create a font for the list items
        let font = wasmModule.PdfFont.Create({
          fontFamily: wasmModule.PdfFontFamily.Helvetica,
          size: 12,
          style: wasmModule.PdfFontStyle.Bold
        });

        // Define the text to be displayed in the lists
        let formatted = "Beverages\nCondiments\nConfections\nDairy Products\nGrains/Cereals\nMeat/Poultry\nProduce\nSeafood";

        // Create a list using the formatted text
        let list = wasmModule.PdfList.Create({ text: formatted });
        list.Font = font;
        list.Indent = 8;
        list.TextIndent = 5;
        list.Brush = brush;

        // Draw the list on the page
        let layoutWidget1 = new wasmModule.PdfLayoutWidget(list.H);
        let result = layoutWidget1.Draw({ page: page, x: 0, y: y });
        y = result.Bounds.Bottom;

        // Create another sorted list
        let sortedList = wasmModule.PdfSortedList.Create({ text: formatted });
        sortedList.Font = font;
        sortedList.Indent = 8;
        sortedList.TextIndent = 5;
        sortedList.Brush = brush;

        // Draw the sorted list on the page
        let layoutWidget2 = new wasmModule.PdfLayoutWidget(sortedList.H);
        let result2 = layoutWidget2.Draw({ page: page, x: 0, y: y });

        y = result2.Bounds.Bottom;

        // Create a font and marker for ordered lists
        let font2 = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 12 });
        let marker1 = wasmModule.PdfOrderedMarker.Create({ style: wasmModule.PdfNumberStyle.LowerRoman, font: font2 });

        // Create a sorted list with ordered markers
        let list2 = wasmModule.PdfSortedList.Create({ text: formatted });
        list2.Font = font;
        list2.Marker = marker1;
        list2.Indent = 8;
        list2.TextIndent = 5;
        list2.Brush = brush;
        
        // Draw the ordered list on the page
        let layoutWidget3 = new wasmModule.PdfLayoutWidget(list2.H);
        let result3 = layoutWidget3.Draw({ page: page, x: 0, y: y });
        y = result3.Bounds.Bottom;

        // Create another font and marker for a different ordered list
        let font3 = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 12 });
        let marker2 = wasmModule.PdfOrderedMarker.Create({ style: wasmModule.PdfNumberStyle.LowerLatin, font: font2 });

        // Create another sorted list with a different marker style
        let list3 = wasmModule.PdfSortedList.Create({ text: formatted });
        list3.Font = font;
        list3.Marker = marker2;
        list3.Indent = 8;
        list3.TextIndent = 5;
        list3.Brush = brush;
        
        // Draw the last list on the page
        let layoutWidget4 = new wasmModule.PdfLayoutWidget(list3.H);
        layoutWidget4.Draw({ page: page, x: 0, y: y });

        // Define the output file name
        const outputFileName = "List_result.pdf";

        // Save the document to the specified path
        doc.SaveToFile({ fileName: outputFileName });

        // Clean up resources
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
