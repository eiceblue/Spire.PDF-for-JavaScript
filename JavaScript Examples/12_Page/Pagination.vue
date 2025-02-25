<template>
  <span>The following example demonstrates how to paginate pages in PDF document.</span>
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

        let inputArialFont = "arial.ttf";
        await wasmModule.FetchFileToVFS(inputArialFont,"",`${import.meta.env.BASE_URL}static/data/`);
        const inputImageFile = "Header.png";
        await wasmModule.FetchFileToVFS(inputImageFile,"",`${import.meta.env.BASE_URL}static/data/`);
        const inputImageFile1 = "Footer.png";
        await wasmModule.FetchFileToVFS(inputImageFile1,"",`${import.meta.env.BASE_URL}static/data/`);
        const inputFileName2 = "SciencePersonificationBoston.jpg";
        await wasmModule.FetchFileToVFS(inputFileName2,"",`${import.meta.env.BASE_URL}static/data/`);
        let contentTxtFile = "Summary_of_Science.txt";
        await wasmModule.FetchFileToVFS(contentTxtFile,"",`${import.meta.env.BASE_URL}static/data/`);

        let contentByte = wasmModule.FS.readFile(contentTxtFile);
        const decoder = new TextDecoder('utf-8');
        let content = decoder.decode(contentByte);
        //Create a PPT document
        let doc = wasmModule.PdfDocument.Create();
        // Set the margin
        let unitCvtr = wasmModule.PdfUnitConvertor.Create();
        let margin = wasmModule.PdfMargins.Create();
        margin.Top = unitCvtr.ConvertUnits(
            2.54, wasmModule.PdfGraphicsUnit.Centimeter, wasmModule.PdfGraphicsUnit.Point);
        margin.Bottom = margin.Top;
        margin.Left = unitCvtr.ConvertUnits(
            3.17, wasmModule.PdfGraphicsUnit.Centimeter, wasmModule.PdfGraphicsUnit.Point);
        margin.Right = margin.Left;

        let font_regular = wasmModule.PdfTrueTypeFont.Create({fontFile:inputArialFont,size: 8,style: wasmModule.PdfFontStyle.Regular});
        let font_bold = wasmModule.PdfTrueTypeFont.Create({fontFile:inputArialFont,size: 8,style: wasmModule.PdfFontStyle.Bold});

        DrawCover(doc.Sections.Add(), margin,font_regular,font_bold,inputImageFile,inputImageFile1,inputFileName2);
        DrawContent(doc.Sections.Add(), margin,font_regular,inputImageFile,inputImageFile1,content);
        DrawPageNumber(doc.Sections.get_Item(1), margin,
            1, doc.Sections.get_Item(1).Pages.Count,font_regular);

        // Define the output file name
        const outputFileName = "Pagination_out.pdf";

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

      function DrawPageHeaderAndFooter(page, margin, isCover,font_regular,inputImageFile,inputImageFile1) {
    page.Canvas.SetTransparency({alpha:0.5});
    //Icon
    let headerImage = wasmModule.PdfImage.FromFile(inputImageFile);
    let footerImage = wasmModule.PdfImage.FromFile(inputImageFile1);
    page.Canvas.DrawImage(headerImage, wasmModule.PointF.Create({x:0.0,y: 0.0}));
    page.Canvas.DrawImage(footerImage, wasmModule.PointF.Create({x:
        0.0, y:page.Canvas.ClientSize.Height - footerImage.PhysicalDimension.Height}));
    if (isCover) {
        page.Canvas.SetTransparency({alpha:1.0});
    }

    let brush = wasmModule.PdfBrushes.get_Black();
    let pen = wasmModule.PdfPen.Create({brush:brush, width:0.75});//brush, width
    let format = wasmModule.PdfStringFormat.Create({alignment:wasmModule.PdfTextAlignment.Right});
    format.MeasureTrailingSpaces = true;
    let space = font_regular.Height * 0.75;
    let x = margin.Left;
    let width = page.Canvas.ClientSize.Width - margin.Left - margin.Right;
    let y = margin.Top - space;
    page.Canvas.DrawLine({pen:pen,x1: x,y1: y, x2:x + width,y2: y});
    y = y - 1 - font_regular.Height;
    page.Canvas.DrawString({s:"Demo of Spire.Pdf", font:font_regular, brush:brush, x:x + width, y:y, format:format});
    page.Canvas.SetTransparency({alpha:1.0});
}


      function DrawCover(section, margin,font_regular,font_bold,inputImageFile,inputImageFile1,inputFileName2) {
    section.PageSettings.Size = wasmModule.PdfPageSize.A4();
    section.PageSettings.Margins.All = 0.0;
    let page = section.Pages.Add();
    DrawPageHeaderAndFooter(page, margin, true,font_regular,inputImageFile,inputImageFile1);
    // Reference    content
    let brush1 = wasmModule.PdfBrushes.get_LightGray();
    let brush2 = wasmModule.PdfBrushes.get_Blue();

    let format = wasmModule.PdfStringFormat.Create();
    format.MeasureTrailingSpaces = true;
    let text1 = "(All text and picture from ";
    let text2 = "Wikipedia";
    let text3 = ", the free encyclopedia)";
    let x = 0.0;
    let y = 10.0;
    x = x + margin.Left;
    y = y + margin.Top;
    page.Canvas.DrawString({s:text1, font:font_regular, brush:brush1, x:x, y:y, format:format});
    x = x + font_regular.MeasureString(text1, format).Width;
    page.Canvas.DrawString({s:text2, font:font_regular, brush:brush2, x:x, y:y, format:format});
    x = x + font_regular.MeasureString(text2, format).Width;
    page.Canvas.DrawString({s:text3, font:font_regular, brush:brush1, x:x, y:y, format:format});
    //Cover
    let brush3 = wasmModule.PdfBrushes.get_Black();
    let brush4 = wasmModule.PdfSolidBrush.Create({pdfRGBColor:wasmModule.PdfRGBColor.Create({red:0xf9,green: 0xf9,blue: 0xf9})});

    let image = wasmModule.PdfImage.FromFile(inputFileName2);

    let text = "Personification of \"Science\" in front of the Boston Public Library";
    let r = image.PhysicalDimension.Height / image.Height;
    let pen = wasmModule.PdfPen.Create({brush:brush1,width: r});
    let size = font_regular.MeasureString({text:text,width: image.PhysicalDimension.Width - 2});
    let template = wasmModule.PdfTemplate.Create({width:image.PhysicalDimension.Width + 4 * r + 4,
        height: image.PhysicalDimension.Height + 4 * r + 7 + size.Height});
    template.Graphics.DrawRectangle({
        pen: pen,
        brush: brush4,
        x: 0,
        y: 0,
        width: template.Width,
        height: template.Height
    });
    x = y = r + 2;
    template.Graphics.DrawRectangle(
        {brush: brush1, x:x,y: y,width: image.PhysicalDimension.Width + 2 * r,height: image.PhysicalDimension.Height + 2 * r});
    x = y = x + r;
    template.Graphics.DrawImage({image:image, x:x,y: y});
    x = x - 1;
    y = y + image.PhysicalDimension.Height + r + 2;
    template.Graphics.DrawString(
        {s: text,font: font_regular,brush: brush3,layoutRectangle: wasmModule.RectangleF.Create({location:wasmModule.PointF.Create({x:x,y: y}), size:size})});
    let x1 = (page.Canvas.ClientSize.Width - template.Width) / 2;
    let y1 = (page.Canvas.ClientSize.Height - margin.Top - margin.Bottom) *  (1 - 0.618) - template.Height / 2 + margin.Top;
    let graphicsWidget=new wasmModule.PdfGraphicsWidget(template.H);
    graphicsWidget.Draw({graphics:page.Canvas,x: x1,y: y1});
    // Title
    format.Alignment = wasmModule.PdfTextAlignment.Center;

    x = page.Canvas.ClientSize.Width / 2;
    y = y1 + template.Height + 10;
    page.Canvas.DrawString({s:"Science History and Etymology", font:font_bold, brush:brush3, x:x, y:y, format:format});
}

function DrawContent(section, margin,font_regular,inputImageFile,inputImageFile1,content) {
    section.PageSettings.Size = wasmModule.PdfPageSize.A4();

    section.PageSettings.Margins.All = 0.0;
    let page = section.Pages.Add();
    DrawPageHeaderAndFooter(page, margin, false,font_regular,inputImageFile,inputImageFile1);
    let x = margin.Left;
    let y = margin.Top + 8;
    let width = page.Canvas.ClientSize.Width - margin.Left - margin.Right;

    let brush1 = wasmModule.PdfBrushes.get_Black();
    let pen1 = wasmModule.PdfPen.Create({brush:brush1,width: 0.75});
    page.Canvas.DrawString({s:"Science History and Etymology", font:font_regular, brush:brush1, x:x, y:y});
    y = y + font_regular.MeasureString({text:"Science History and Etymology"}).Height + 6;
    page.Canvas.DrawLine({pen:pen1,x1:x, y1:y, x2:page.Canvas.ClientSize.Width - margin.Right, y2:y});
    y = y + 1.75;
  
    let lines = content.split('\n');

    let format1 = wasmModule.PdfStringFormat.Create();
    format1.MeasureTrailingSpaces = true;
    format1.LineSpacing = font_regular.Height * 1.5;
    format1.ParagraphIndent = font_regular.MeasureString({text:"\t",format: format1}).Width;
    y = y + font_regular.Height * 0.5;
    let size = font_regular.MeasureString({text:lines[0],width: width,format: format1});
    page.Canvas.DrawString({s:lines[0], font:font_regular, brush:brush1, layoutRectangle:wasmModule.RectangleF.Create(wasmModule.PointF.Create(x, y), size), format:format1});
    y = y + size.Height;
    let format2 = wasmModule.PdfStringFormat.Create();
    format2.LineSpacing = font_regular.Height * 1.4;
    format2.MeasureTrailingSpaces = true;
    size = font_regular.MeasureString({text:lines[1],width: width, format:format2});
    page.Canvas.DrawString({s:lines[1], font:font_regular, brush:brush1, layoutRectangle:wasmModule.RectangleF.Create(wasmModule.PointF.Create(x, y), size), format:format2});
    y = y + size.Height;
    y = y + font_regular.Height * 0.75;
    let indent = font_regular.MeasureString({text:"\t\t", format:format2}).Width;
    let x1 = x + indent;
    size = font_regular.MeasureString({text:lines[2],width: width - indent,format: format2});
    page.Canvas.DrawString({s:lines[2], font:font_regular, brush:brush1, layoutRectangle:wasmModule.RectangleF.Create(wasmModule.PointF.Create(x, y), size), format:format2});
    y = y + size.Height + font_regular.Height * 0.75;

    let textLayouter = wasmModule.PdfStringLayouter.Create();
    let result = textLayouter.Layout(content, font_regular, format2, wasmModule. SizeF.Create({width:width,height: 999999}));
    let count=result.Lines.length;
    for (let i=0;i<count;i++)
    {
        let line=result.Lines[i];

        if (line.intLineType &wasmModule. LineType.FirstParagraphLine.value === wasmModule. LineType.FirstParagraphLine.value) {
            y = y + font_regular.Height * 0.75
        }
        if (y > (page.Canvas.ClientSize.Height - margin.Bottom - result.LineHeight)){
            page = section.Pages.Add();
            DrawPageHeaderAndFooter(page, margin, false,font_regular,inputImageFile,inputImageFile1);
            y = margin.Top;
        }

        //Draw text
        page.Canvas.DrawString({s:line.Text, font:font_regular, brush:brush1,x: x,y: y, format:format2});
        y = y + result.LineHeight + 2;
    }
}

function DrawPageNumber(section, margin, startNumber, pageCount,font_regular) {
    let count=section.Pages.Count;
    for (let i=0;i<count;i++) {
        let page = section.Pages.get_Item(i);

        page.Canvas.SetTransparency({alpha:0.5});
        let brush = wasmModule.PdfBrushes.get_Black();
        let pen = wasmModule.PdfPen.Create({brush:brush,width: 0.75});
        let format = wasmModule.PdfStringFormat.Create({alignment:wasmModule.PdfTextAlignment.Right});
        format.MeasureTrailingSpaces = true;
        let space = font_regular.Height * 0.75;
        let x = margin.Left;
        let width = page.Canvas.ClientSize.Width - margin.Left - margin.Right;
        let y = page.Canvas.ClientSize.Height - margin.Bottom + space;
        page.Canvas.DrawLine({pen:pen,x1:x, y1:y, x2:x + width, y2:y});
        y = y + 1;
        let numberLabel = `${startNumber} of ${pageCount}`;
        startNumber += 1;
        page.Canvas.DrawString({s:numberLabel, font:font_regular, brush:brush, x:x + width, y:y, format:format});
        page.Canvas.SetTransparency({alpha:1.0});
    }
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
