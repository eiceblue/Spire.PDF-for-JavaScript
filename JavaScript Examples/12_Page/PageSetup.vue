<template>
  <span>The following example demonstrates how to work with page setup in PDF document.</span>
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
        let txtFile = "Summary_of_Science.txt"
        let inputArialFont = "arial.ttf";
        const inputFileName3 = "Wikipedia_Science.png";
        await wasmModule.FetchFileToVFS(inputFileName3,"",`${import.meta.env.BASE_URL}static/data/`);
        await wasmModule.FetchFileToVFS(txtFile,"",`${import.meta.env.BASE_URL}static/data/`);
        await wasmModule.FetchFileToVFS(inputArialFont,"",`${import.meta.env.BASE_URL}static/data/`);
        //Create a PPT document
        let doc = wasmModule.PdfDocument.Create();

        let contentByte = wasmModule.FS.readFile(txtFile);
        const decoder = new TextDecoder('utf-8');
        let contentText = decoder.decode(contentByte);
	      //Set the margin
        let unitCvtr = wasmModule.PdfUnitConvertor.Create();
        let margin = wasmModule.PdfMargins.Create();
        margin.Top = unitCvtr.ConvertUnits(2.54, wasmModule.PdfGraphicsUnit.Centimeter, wasmModule.PdfGraphicsUnit.Point);
        margin.Bottom = margin.Top;
        margin.Left = unitCvtr.ConvertUnits(3.17, wasmModule.PdfGraphicsUnit.Centimeter, wasmModule.PdfGraphicsUnit.Point);
        margin.Right = margin.Left;

        //Create one page
        let page = doc.Pages.Add({size: wasmModule.PdfPageSize.A4(), margins: margin});
        page.BackgroundColor = wasmModule.Color.get_Chocolate();

        //Draw content on the page
        DrawPage(page,inputArialFont,inputFileName3,contentText);

        page = doc.Pages.Add({size: wasmModule.PdfPageSize.A4(), margins: margin});
        page.BackgroundColor = wasmModule.Color.get_Coral();
        DrawPage(page,inputArialFont,inputFileName3,contentText);

        page = doc.Pages.Add(wasmModule.PdfPageSize.A3(), margin, wasmModule.PdfPageRotateAngle.RotateAngle180, wasmModule.PdfPageOrientation.Landscape);
        page.BackgroundColor = wasmModule.Color.get_LightPink();
        DrawPage(page,inputArialFont,inputFileName3,contentText);

        //create section
        let section = doc.Sections.Add();
        page = section.Pages.Add();
        section.PageSettings.Size = wasmModule.PdfPageSize.A4();
        section.PageSettings.Margins = margin;
        DrawPage(page,inputArialFont,inputFileName3,contentText);

        //Set background color
        page = section.Pages.Add();
        page.BackgroundColor = wasmModule.Color.get_LightSkyBlue();
        DrawPage(page,inputArialFont,inputFileName3,contentText);

        // Landscape
        section = doc.Sections.Add();
        section.PageSettings.Orientation = wasmModule.PdfPageOrientation.Landscape;
        page = section.Pages.Add();
        section.PageSettings.Size = wasmModule.PdfPageSize.A4();
        section.PageSettings.Margins = margin;
        DrawPage(page,inputArialFont,inputFileName3,contentText);

        // Rotate 90
        section = doc.Sections.Add();
        page = section.Pages.Add();
        section.PageSettings.Size = wasmModule.PdfPageSize.A4();
        section.PageSettings.Margins = margin;
        section.PageSettings.Rotate = wasmModule.PdfPageRotateAngle.RotateAngle90;
        DrawPage(page,inputArialFont,inputFileName3,contentText);

        //Rotate 180
        section = doc.Sections.Add();
        page = section.Pages.Add();
        section.PageSettings.Size = wasmModule.PdfPageSize.A4();
        section.PageSettings.Margins = margin;
        section.PageSettings.Rotate = wasmModule.PdfPageRotateAngle.RotateAngle180;
        DrawPage(page,inputArialFont,inputFileName3,contentText);

        // Define the output file name
        const outputFileName = "PageSetup_out.pdf";

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

      function DrawPage(page,inputArialFont,inputFileName3,contentText) {
    let pageWidth = page.Canvas.ClientSize.Width;
    let y = 0.0;
    //Title
    y = y + 5;
    let brush2 = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_Black()});


    let font2 = wasmModule.PdfTrueTypeFont.Create({
        fontFile: inputArialFont,
        size: 16,
        style: wasmModule.PdfFontStyle.Bold
    });
    let format2 = wasmModule.PdfStringFormat.Create({alignment: wasmModule.PdfTextAlignment.Center});
    format2.CharacterSpacing = 1.0;
    let text = "Summary of Science";
    page.Canvas.DrawString({s: text, font: font2, brush: brush2, x: pageWidth / 2, y: y, format: format2});
    let size = font2.MeasureString({text: text, format: format2});
    y = y + size.Height + 6;

    //Icon
    let image = wasmModule.PdfImage.FromFile(inputFileName3);

    page.Canvas.DrawImage({
        image: image, point: wasmModule.PointF.Create({
            x:
                pageWidth - image.PhysicalDimension.Width, y: y
        })
    });
    let imageLeftSpace = pageWidth - image.PhysicalDimension.Width - 2;
    let imageBottom = image.PhysicalDimension.Height + y;
    // Reference content
    let font3 = wasmModule.PdfTrueTypeFont.Create({
        fontFile: inputArialFont,
        size: 9,
        style: wasmModule.PdfFontStyle.Regular
    });
    let format3 = wasmModule.PdfStringFormat.Create();
    format3.ParagraphIndent = font3.Size * 2;
    format3.MeasureTrailingSpaces = true;
    format3.LineSpacing = font3.Size * 1.5;
    let text1 = "(All text and picture from ";
    let text2 = "Wikipedia";
    let text3 = ", the free encyclopedia)";
    page.Canvas.DrawString({s: text1, font: font3, brush: brush2, x: 0, y: y, format: format3});
    size = font3.MeasureString({text: text1, format: format3});
    let x1 = size.Width;
    format3.ParagraphIndent = 0;
    let font4 = wasmModule.PdfTrueTypeFont.Create({
        fontFile: inputArialFont,
        size: 9,
        style: wasmModule.PdfFontStyle.Underline
    });
    let brush3 = wasmModule.PdfBrushes.get_Blue();
    page.Canvas.DrawString({s: text2, font: font4, brush: brush3, x: x1, y: y, format: format3});
    size = font4.MeasureString({text: text2, format: format3});
    x1 = x1 + size.Width;
    page.Canvas.DrawString({s: text3, font: font3, brush: brush2, x: x1, y: y, format: format3});
    y = y + size.Height;
    //Content
    let format4 = wasmModule.PdfStringFormat.Create();

    let font5 = wasmModule.PdfTrueTypeFont.Create({
        fontFile: inputArialFont,
        size: 10,
        style: wasmModule.PdfFontStyle.Regular
    });

    format4.LineSpacing = font5.Size * 1.5;
    let textLayouter = wasmModule.PdfStringLayouter.Create();
    let imageLeftBlockHeight = imageBottom - y;
    let result = textLayouter.Layout(contentText, font5, format4, wasmModule.SizeF.Create({
        width: imageLeftSpace, height: imageLeftBlockHeight
    }));

    if (result.ActualSize.Height < imageBottom - y) {
        imageLeftBlockHeight = imageLeftBlockHeight + result.LineHeight;
        result = textLayouter.Layout(contentText, font5, format4, wasmModule.SizeF.Create({
            width: imageLeftSpace, height: imageLeftBlockHeight
        }));
    }

    let count = result.Lines.length;
    for (let i = 0; i < count; i++) {
        let line = result.Lines[i];
        //Draw text
        page.Canvas.DrawString({s: line.Text, font: font5, brush: brush2, x: 0, y: y, format: format4});
        y = y + result.LineHeight;
    }

    let textWidget = wasmModule.PdfTextWidget.Create({text: result.Remainder, font: font5, brush: brush2});
    let textLayout = wasmModule.PdfTextLayout.Create();
    textLayout.Break = wasmModule.PdfLayoutBreakType.FitPage;
    textLayout.Layout = wasmModule.PdfLayoutType.Paginate;
    let bounds = wasmModule.RectangleF.Create({
        location: wasmModule.PointF.Create({x: 0, y: y}),
        size: page.Canvas.ClientSize
    });
    textWidget.StringFormat = format4;
    let newPage = new wasmModule.PdfNewPage(page.H);
    let layoutWidget = new wasmModule.PdfLayoutWidget(textWidget.H);
    layoutWidget.Draw({page: newPage, layoutRectangle: bounds, format: textLayout});
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
