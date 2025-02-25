<template>
  <span>The following example demonstrates how to place text around image in PDF document.</span>
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

        const inputImageFile = "logo.png";
        await wasmModule.FetchFileToVFS(inputImageFile, '', `${import.meta.env.BASE_URL}static/data/`);

        // Create a pdf instance
        let doc = wasmModule.PdfDocument.Create();
        
        //Create one page
        let page = doc.Pages.Add();

        //Gets page width
        let pageWidth = page.Canvas.ClientSize.Width;
        let y = 0;

        y = y + 8;

        // Creates a brush
        let brush = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_Black()});

        // Defines a font
        let font1 = wasmModule.PdfTrueTypeFont.Create({
            fontName: 'Arial Unicode MS',
            emSize: 20,
            style: wasmModule.FontStyle.Regular,
            unicode:true
        });

        // Defines a text center alignment format
        let format1 = wasmModule.PdfStringFormat.Create({alignment: wasmModule.PdfTextAlignment.Center});
        format1.CharacterSpacing = 1;

        let text = "Spire.PDF for JavaScript";
        // Draws text at the specified position
        page.Canvas.DrawString({s: text, font: font1, brush: brush, x: pageWidth / 2, y: y, format: format1});
        // Get the size of text
        let size = font1.MeasureString({text: text, format: format1});
        y = y + size.Height + 6;

        // Loads an image
        let image = wasmModule.PdfImage.FromFile(inputImageFile);

        // Draws image at the specified position
        let pointf = wasmModule.PointF.Create({x: pageWidth - image.PhysicalDimension.Width, y: y});
        page.Canvas.DrawImage({image: image, point: pointf});
        let imageLeftSpace = pageWidth - image.PhysicalDimension.Width - 2;
        let imageBottom = image.PhysicalDimension.Height + y;

        let format2 = wasmModule.PdfStringFormat.Create();
        
        // Loads the text around the image
        text = "Spire.PDF for JavaScript is a professional PDF API applied to creating, writing, editing, handling and reading PDF files without any external dependencies."
        +" Using this JavaScript PDF library, you can implement rich capabilities to create PDF files from scratch or process existing PDF documents entirely without installing Adobe Acrobat."
        +"Many rich features can be supported by the JavaScript PDF API, such as PDF text/attachment/image extract, PDF merge/split, section, graph/image drawing and inserting, table creation and processing, and importing data etc."
        +" Besides, Spire.PDF for JavaScript can be applied to easily converting Text, Image and HTML to PDF in high quality.";

        let font2 = wasmModule.PdfTrueTypeFont.Create({fontFile: "/Library/Fonts/ARIALUNI.TTF", size: 16});

        //Set line spacing
        format2.LineSpacing = font2.Size * 1.5;

        let textLayouter = wasmModule.PdfStringLayouter.Create();
        let imageLeftBlockHeight = imageBottom - y;

        // Splits the text around into multiple lines based on the draw area
        let result
            = textLayouter.Layout(text, font2, format2, spirepdf.SizeF.Create({
            width: imageLeftSpace,
            height: imageLeftBlockHeight
        }));
        if (result.ActualSize.Height < imageLeftBlockHeight) {
            imageLeftBlockHeight = imageLeftBlockHeight + result.LineHeight;
            result = textLayouter.Layout(text, font2, format2, wasmModule.SizeF.Create({
                width: imageLeftSpace,
                height: imageLeftBlockHeight
            }));
        }

        // Draws all the lines onto the page
        for (let i = 0; i < result.Lines.length; i++) {
            let line = result.Lines[i];
            page.Canvas.DrawString({s: line.Text, font: font2, brush: brush, x: 0, y: y, format: format2});
            y = y + result.LineHeight;
        }

        // Draw the rest of the text onto the page
        let textWidget = wasmModule.PdfTextWidget.Create({text: result.Remainder, font: font2, brush: brush});
        let textLayout = wasmModule.PdfTextLayout.Create();
        textLayout.Break = wasmModule.PdfLayoutBreakType.FitPage;
        textLayout.Layout = wasmModule.PdfLayoutType.Paginate;
        let point1 = wasmModule.PointF.Create({x: 0, y: y});
        let bounds = wasmModule.RectangleF.Create({location: point1, size: page.Canvas.ClientSize});
        textWidget.StringFormat = format2;
        textWidget.Draw({page: page, layoutRectangle: bounds, format: textLayout});

        // Define the output file name
        const outputFileName = "WrapTextAroundImage_out.pdf";

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
