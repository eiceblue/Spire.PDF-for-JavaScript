<template>
  <span>The following example demonstrates how to work with text layout in PDF document.</span>
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
        const inputImageFile = "Wikipedia_Science.png";
        await wasmModule.FetchFileToVFS(inputImageFile, '', `${import.meta.env.BASE_URL}static/data/`);

        // Create a pdf instance
        let doc = wasmModule.PdfDocument.Create();

        //Create one page
        let page = doc.Pages.Add();
        let pageWidth = page.Canvas.ClientSize.Width;
        let y = 0;

        //Page header
        let pdfrgbColor = wasmModule.PdfRGBColor.Create({color: wasmModule.Color.get_LightGray()});
        let pen1 = wasmModule.PdfPen.Create({pdfRgbColor: pdfrgbColor, width: 1});
        let brush1 = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_LightGray()});

        let font1 = wasmModule.PdfFont.Create(wasmModule.PdfFontFamily.Helvetica, 20);
        let format1 = wasmModule.PdfStringFormat.Create({alignment: wasmModule.PdfTextAlignment.Right});
        let text = "Demo of Spire.Pdf";

        //Draw text
        page.Canvas.DrawString({s: text, font: font1, brush: brush1, x: pageWidth - 2, y: y, format: format1});

        //Measure the size of text
        let size = font1.MeasureString({text: text, format: format1});
        y = y + size.Height + 1;

        //Draw the text of header
        page.Canvas.DrawLine({pen: pen1, x1: 0, y1: y, x2: pageWidth, y2: y});

        //Title
        y = y + 25;
        let brush2 = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_Black()});
        let format2 = wasmModule.PdfStringFormat.Create({alignment: wasmModule.PdfTextAlignment.Center});
        format2.CharacterSpacing = 1;
        text = "Summary of Science";
        page.Canvas.DrawString({s: text, font: font1, brush: brush2, x: pageWidth / 2, y: y, format: format2});
        size = font1.MeasureString({text: text, format: format2});
        y = y + size.Height + 16;

        //Icon
        let image = wasmModule.PdfImage.FromFile(inputImageFile);
        let point = wasmModule.PointF.Create({x: pageWidth - image.PhysicalDimension.Width, y: y});
        page.Canvas.DrawImage({image: image, point: point});
        let imageLeftSpace = pageWidth - image.PhysicalDimension.Width - 2;
        let imageBottom = image.PhysicalDimension.Height + y;

        //Refenrence content
        let font2 = wasmModule.PdfFont.Create(wasmModule.PdfFontFamily.Helvetica, 12);
        //Define string format
        let format3 = wasmModule.PdfStringFormat.Create();
        format3.ParagraphIndent = font2.Size * 2;
        format3.MeasureTrailingSpaces = true;
        format3.LineSpacing = font2.Size * 1.5;
        let text1 = "(All text and picture from ";
        let text2 = "Wikipedia";
        let text3 = ", the free encyclopedia)";

        //Draw text
        page.Canvas.DrawString({s: text1, font: font2, brush: brush2, x: 0, y: y, format: format3});

        //Meature the size of text
        size = font2.MeasureString({text: text1, format: format3});
        let x1 = size.Width;
        format3.ParagraphIndent = 0;
    
        let brush3 = wasmModule.PdfBrushes.get_Blue();
        page.Canvas.DrawString({s: text2, font: font2, brush: brush3, x: x1, y: y, format: format3});

        //Measure the size of text
        size = font2.MeasureString({text: text2, format: format3});
        x1 = x1 + size.Width;

        //Draw text
        page.Canvas.DrawString({s: text3, font: font2, brush: brush2, x: x1, y: y, format: format3});
        y = y + size.Height;

        //Content
        //Define the format of string
        let format4 = wasmModule.PdfStringFormat.Create();
        text = "Science (from the Latin scientia, meaning \"knowledge\") is an enterprise that builds and organizes knowledge in the form of testable explanations and predictions about the natural world. An older meaning still in use today is that of Aristotle, for whom scientific knowledge was a body of reliable knowledge that can be logically and rationally explained."
        +"Since classical antiquity science as a type of knowledge was closely linked to philosophy, the way of life dedicated to discovering such knowledge. And into early modern times the two words, \"science\" and \"philosophy\","
        +" were sometimes used interchangeably in the English language. By the 17th century, \"natural philosophy\" (which is today called \"natural science\") "
        +"could be considered separately from \"philosophy\" in general. But \"science\" continued to also be used in a broad sense denoting "
        +"reliable knowledge about a topic, in the same way it is still used in modern terms such as library science or political science."
        +"The more narrow sense of \"science\" that is common today developed as a part of science became a distinct enterprise "
        +"of defining \"laws of nature\", based on early examples such as Kepler's laws, Galileo's laws, and Newton's laws of motion. In this period it became more common to refer to natural philosophy as \"natural science\". "
        +"Over the course of the 19th century, the word \"science\" became increasingly associated with the disciplined study of the natural world including physics, chemistry, geology and biology. "
        +"This sometimes left the study of human thought and society in a linguistic limbo, which was resolved by classifying these areas of academic study as social science. Similarly, several other major areas of disciplined study and knowledge exist today under the general rubric of \"science\", such as formal science and applied science.";

        //Define the font style
        format4.LineSpacing = font2.Size * 1.5;

        let textLayouter = wasmModule.PdfStringLayouter.Create();
        let imageLeftBlockHeight = imageBottom - y;
        let result = textLayouter.Layout(text, font2, format4, wasmModule.SizeF.Create({
            width: imageLeftSpace,
            height: imageLeftBlockHeight
        }));

        if (result.ActualSize.Height < imageBottom - y) {
            imageLeftBlockHeight = imageLeftBlockHeight + result.LineHeight;
            result = textLayouter.Layout(text, font2, format4, wasmModule.SizeF.Create({
                width: imageLeftSpace,
                height: imageLeftBlockHeight
            }));
        }
        let count = result.Lines.length;
        for (let i = 0; i < count; i++) {
            let line = result.Lines[i];
            //Draw text
            page.Canvas.DrawString({s: line.Text, font: font2, brush: brush2, x: 0, y: y, format: format4});
            y = y + result.LineHeight + 2;
        }

        let textWidget = wasmModule.PdfTextWidget.Create({text: result.Remainder, font: font2, brush: brush2});
        let textLayout = wasmModule.PdfTextLayout.Create();
        textLayout.Break = wasmModule.PdfLayoutBreakType.FitPage;
        textLayout.Layout = wasmModule.PdfLayoutType.Paginate;
        let point1 = wasmModule.PointF.Create({x: 0, y: y});
        let bounds = wasmModule.RectangleF.Create({location: point1, size: page.Canvas.ClientSize});
        textWidget.StringFormat = format4;
        textWidget.Draw({page: page, layoutRectangle: bounds, format: textLayout});

        // Define the output file name
        const outputFileName = "TextLayout_out.pdf";

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
