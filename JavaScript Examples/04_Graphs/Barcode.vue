<template>
  <span>The following example demonstrates how to draw barcode in PDF document.</span>
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

        // Set the margin
        let unitCvtr = wasmModule.PdfUnitConvertor.Create();
        let margin = wasmModule.PdfMargins.Create({ margin: 0 });
        margin.Top = unitCvtr.ConvertUnits(2.54, wasmModule.PdfGraphicsUnit.Centimeter, wasmModule.PdfGraphicsUnit.Point);
        margin.Bottom = margin.Top;
        margin.Left = unitCvtr.ConvertUnits(3.17, wasmModule.PdfGraphicsUnit.Centimeter, wasmModule.PdfGraphicsUnit.Point);
        margin.Right = margin.Left;

        // Add PdfSection and set layout for it
        let section = doc.Sections.Add();
        section.PageSettings.Margins = margin;
        section.PageSettings.Size = wasmModule.PdfPageSize.A4();

        // Add a new page
        let page = section.Pages.Add();
        let y = 10;

        // Create a font and set style for it
        let font1 = wasmModule.PdfTrueTypeFont.Create({
            fontFile: "/Library/Fonts/ARIALUNI.TTF",
            size: 12,
            style: wasmModule.PdfFontStyle.Bold
        });

        let text = wasmModule.PdfTextWidget.Create();
        text.Font = font1;
        text.Text = "Codabar:";
        let format = wasmModule.PdfTextLayout.Create();
        let location = wasmModule.PointF.Create({ x: 0, y: y });
        let result = text.Draw({ page: page, location: location, format: format });
        page = result.Page;
        y = result.Bounds.Bottom + 2;

        // Draw Codabar
        let barcode1 = wasmModule.PdfCodabarBarcode.Create({ text: "00:12-3456/7890" });
        barcode1.BarcodeToTextGapHeight = 1;
        barcode1.TextDisplayLocation = wasmModule.TextLocation.Bottom;
        barcode1.TextColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Blue() });
        location = wasmModule.PointF.Create({ x: 0, y: y });
        barcode1.Draw({ page: page, location: location });
        y = barcode1.Bounds.Bottom + 5;

        // Draw Code11Barcode
        text.Text = "Code11:";
        result = text.Draw({ page: page, location: wasmModule.PointF.Create({ x: 0, y: y }), format: format });
        page = result.Page;
        y = result.Bounds.Bottom + 2;

        let barcode2 = wasmModule.PdfCode11Barcode.Create({ text: "123-4567890" });
        barcode2.BarcodeToTextGapHeight = 1;
        barcode2.TextDisplayLocation = wasmModule.TextLocation.Bottom;
        barcode2.TextColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Blue() });
        location = wasmModule.PointF.Create({ x: 0, y: y });
        barcode2.Draw({ page: page, location: location });
        y = barcode2.Bounds.Bottom + 5;

        // Draw Code32
        text.Text = "Code32:";
        result = text.Draw({ page: page, location: wasmModule.PointF.Create({ x: 0, y: y }), format: format });
        page = result.Page;
        y = result.Bounds.Bottom + 2;

        // Create PdfCode32Barcode object and set style for it
        let barcode5 = wasmModule.PdfCode32Barcode.Create({ text: "16273849" });
        barcode5.BarcodeToTextGapHeight = 1;
        barcode5.TextDisplayLocation = wasmModule.TextLocation.Bottom;
        barcode5.TextColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Blue() });
        barcode5.Draw({ page: page, location: wasmModule.PointF.Create({ x: 0, y: y }) });// new PointF(0, y));
        y = barcode5.Bounds.Bottom + 5;

        page = section.Pages.Add();
        y = 10;

        // Draw Code39
        text.Text = "Code39:";
        result = text.Draw({ page: page, location: wasmModule.PointF.Create({ x: 0, y: y }), format: format });
        page = result.Page;
        y = result.Bounds.Bottom + 2;

        // Create PdfCode39Barcode object and set style for it
        let barcode6 = wasmModule.PdfCode39Barcode.Create({ text: "16-273849" });
        barcode6.BarcodeToTextGapHeight = 1;
        barcode6.TextDisplayLocation = wasmModule.TextLocation.Bottom;
        barcode6.TextColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Blue() });
        barcode6.Draw({ page: page, location: wasmModule.PointF.Create({ x: 0, y: y }) });// new PointF(0, y));
        y = barcode6.Bounds.Bottom + 5;

        // Draw Code39-E
        text.Text = "Code39-E:";
        result = text.Draw({ page: page, location: wasmModule.PointF.Create({ x: 0, y: y }), format: format });
        page = result.Page;
        y = result.Bounds.Bottom + 2;

        // Create PdfCode39ExtendedBarcode object and set style for it
        let barcode7 = wasmModule.PdfCode39ExtendedBarcode.Create({ text: "16-273849" });
        barcode7.BarcodeToTextGapHeight = 1;
        barcode7.TextDisplayLocation = wasmModule.TextLocation.Bottom;
        barcode7.TextColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Blue() });
        barcode7.Draw({ page: page, location: wasmModule.PointF.Create({ x: 0, y: y }) });// new PointF(0, y));
        y = barcode7.Bounds.Bottom + 5;

        // Draw Code93
        text.Text = "Code93:";
        result = text.Draw({ page: page, location: wasmModule.PointF.Create({ x: 0, y: y }), format: format });
        page = result.Page;
        y = result.Bounds.Bottom + 2;

        // Create PdfCode93Barcode object and set style for it
        let barcode8 = wasmModule.PdfCode93Barcode.Create({ text: "16-273849" });
        barcode8.BarcodeToTextGapHeight = 1;
        barcode8.TextDisplayLocation = wasmModule.TextLocation.Bottom;
        barcode8.QuietZone.Bottom = 5;
        barcode8.TextColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Blue() });
        barcode8.Draw({ page: page, location: wasmModule.PointF.Create({ x: 0, y: y }) });// new PointF(0, y));
        y = barcode8.Bounds.Bottom + 5;

        // Draw Code93-E
        text.Text = "Code93-E:";
        result = text.Draw({ page: page, location: wasmModule.PointF.Create({ x: 0, y: y }), format: format });
        page = result.Page;
        y = result.Bounds.Bottom + 2;

        // Create PdfCode93ExtendedBarcode object and set style for it
        let barcode9 = wasmModule.PdfCode93ExtendedBarcode.Create({ text: "16-273849" });
        barcode9.BarcodeToTextGapHeight = 1;
        barcode9.TextDisplayLocation = wasmModule.TextLocation.Bottom;
        barcode9.TextColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Blue() });
        barcode9.Draw({ page: page, location: wasmModule.PointF.Create({ x: 0, y: y }) });// new PointF(0, y));
        y = barcode9.Bounds.Bottom + 5;

        // Define the output file name
        const outputFileName = "Barcode_result.pdf";

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
