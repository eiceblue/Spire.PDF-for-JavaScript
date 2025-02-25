<template>
  <span>The following example demonstrates how to draw template.</span>
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

// SetDocumentTemplate method for configuring document template with specified page size and margins
function SetDocumentTemplate(doc, pageSize, margin) {
    // Create a template element for the left space on the page
    let leftSpace = wasmModule.PdfPageTemplateElement.Create({width: margin.Left, height: pageSize.Height});
    doc.Template.Left = leftSpace;
    let topSpace = wasmModule.PdfPageTemplateElement.Create({width: pageSize.Width, height: margin.Top});
    topSpace.Foreground = true;
    doc.Template.Top = topSpace;
    // Draw header label
    let inputArialFont = "/Library/Fonts/ARIALUNI.TTF";
    let font = wasmModule.PdfTrueTypeFont.Create({
        fontFile: inputArialFont,
        size: 9,
        style: wasmModule.PdfFontStyle.Italic
    });
    let format = wasmModule.PdfStringFormat.Create({alignment: wasmModule.PdfTextAlignment.Right});
    let label = "Demo of Spire.Pdf";
    let size = font.MeasureString({text: label, format: format});
    let y = topSpace.Height - font.Height - 1;
    let pdfrgbColor = wasmModule.PdfRGBColor.Create({color: wasmModule.Color.get_Black()});
    let pen = wasmModule.PdfPen.Create({pdfRgbColor: pdfrgbColor, width: 0.75});

    topSpace.Graphics.SetTransparency({alpha: 0.5});
    topSpace.Graphics.DrawLine({pen: pen, x1: margin.Left, y1: y, x2: pageSize.Width - margin.Right, y2: y});

    y = y - 1 - size.Height;
    topSpace.Graphics.DrawString({
        s: label,
        font: font,
        brush: wasmModule.PdfBrushes.get_Black(),
        x: pageSize.Width - margin.Right,
        y: y,
        format: format
    });
    // Create a template element for the right space on the page
    let rightSpace = wasmModule.PdfPageTemplateElement.Create({width: margin.Right, height: pageSize.Height});
    doc.Template.Right = rightSpace;
    let bottomSpace = wasmModule.PdfPageTemplateElement.Create({width: pageSize.Width, height: margin.Bottom});
    // Create a template element for the bottom space on the page
    bottomSpace.Foreground = true;
    doc.Template.Bottom = bottomSpace;
    // Draw footer label
    y = font.Height + 1;
    bottomSpace.Graphics.SetTransparency({alpha: 0.5});
    bottomSpace.Graphics.DrawLine({pen: pen, x1: margin.Left, y1: y, x2: pageSize.Width - margin.Right, y2: y});
    // Draw a footer label
    y = y + 1;
    let pageNumber = wasmModule.PdfPageNumberField.Create();
    let pageCount = wasmModule.PdfPageCountField.Create();
    let pageNumberLabel = wasmModule.PdfCompositeField.Create();
    pageNumberLabel.AutomaticFields = [pageNumber, pageCount];
    pageNumberLabel.Brush = wasmModule.PdfBrushes.get_Black();
    pageNumberLabel.Font = font;
    pageNumberLabel.StringFormat = format;
    pageNumberLabel.Text = "page {0} of {1}";
    pageNumberLabel.Draw(bottomSpace.Graphics,
        pageSize.Width - margin.Right, y);
    // Load and add a header image to the template as a stamp
    let headerImage = wasmModule.PdfImage.FromFile("Header.png");
    let pageLeftTop = wasmModule.PointF.Create({x: -margin.Left, y: -margin.Top});
    let header = wasmModule.PdfPageTemplateElement.Create({location: pageLeftTop, size: headerImage.PhysicalDimension});
    header.Foreground = false;
    header.Graphics.SetTransparency({alpha: 0.5});
    header.Graphics.DrawImage({
        image: headerImage, point: wasmModule.PointF.Create(
            {x: 0, y: 0})
    });
    doc.Template.Stamps.Add({template: header});
    // Load and add a footer image to the template as a stamp
    let footerImage = wasmModule.PdfImage.FromFile("Footer.png");
    y = pageSize.Height - footerImage.PhysicalDimension.Height;
    let footerLocation = wasmModule.PointF.Create({x: -margin.Left, y: y});
    let footer = wasmModule.PdfPageTemplateElement.Create(
        {location: footerLocation, size: footerImage.PhysicalDimension});
    footer.Foreground = false;
    footer.Graphics.SetTransparency({alpha: 0.5});
    footer.Graphics.DrawImage({
        image: footerImage, point: wasmModule.PointF.Create(
            {x: 0, y: 0})
    });
    doc.Template.Stamps.Add({template: footer});
}

function SetSectionTemplate(section, pageSize, margin, label) {
    // Create an element for the left blank space with the width of the left margin and the height of the page
    let leftSpace = wasmModule.PdfPageTemplateElement.Create({width: margin.Left, height: pageSize.Height});
    // Set the element as a foreground element
    leftSpace.Foreground = true;
    // Set the element as the left template for odd pages
    section.Template.OddLeft = leftSpace;
    // Create an Arial font object with a size of 9f and an italic style
    let inputArialFont = "/Library/Fonts/ARIALUNI.TTF";
    let font = wasmModule.PdfTrueTypeFont.Create({
        fontFile: inputArialFont,
        size: 9,
        style: wasmModule.PdfFontStyle.Italic
    });
    // Create a string format object with centered text alignment and vertically centered alignment
    let format = wasmModule.PdfStringFormat.Create({
        alignment: wasmModule.PdfTextAlignment.Center,
        lineAlignment: wasmModule.PdfVerticalAlignment.Middle
    });
    // Calculate the y-coordinate value to center the text on the page
    let y = (pageSize.Height - margin.Top - margin.Bottom) * (1 - 0.618);
    // Create a rectangle object with the top-left corner at (10, y), and the bottom-right corner at (left margin - 20, font height + 6)
    let bounds = wasmModule.RectangleF.Create({x: 10.0, y: y, width: margin.Left - 20, height: font.Height + 6});
    // Draw an orange-red rectangle within the left blank space element
    leftSpace.Graphics.DrawRectangle({brush: wasmModule.PdfBrushes.get_OrangeRed(), rectangle: bounds});
    // Draw the label text within the rectangle using the Arial font created earlier, white color, and the previously created string format object
    leftSpace.Graphics.DrawString({
        s: label,
        font: font,
        brush: wasmModule.PdfBrushes.get_White(),
        layoutRectangle: bounds,
        format: format
    });
    // Create an element for the right blank space with the width of the right margin and the height of the page
    let rightSpace = wasmModule.PdfPageTemplateElement.Create({width: margin.Right, height: pageSize.Height});
    // Set the element as a foreground element
    rightSpace.Foreground = true;
    // Set the element as the right template for even pages
    section.Template.EvenRight = rightSpace;
    // Recalculate the bottom-right corner of the rectangle
    bounds = wasmModule.RectangleF.Create({x: 10.0, y: y, width: margin.Right - 20, height: font.Height + 6});
    // Draw a brown rectangle within the right blank space element
    rightSpace.Graphics.DrawRectangle({brush: wasmModule.PdfBrushes.get_SaddleBrown(), rectangle: bounds});
    // Draw the label text within the rectangle using the Arial font created earlier, white color, and the previously created string format object
    rightSpace.Graphics.DrawString({
        s: label,
        font: font,
        brush: wasmModule.PdfBrushes.get_White(),
        layoutRectangle: bounds,
        format: format
    });
}


function DrawPage(page) {
    // Get the width of the page
    let pageWidth = page.Canvas.ClientSize.Width;
    let y = 0.0;
    // Title
    y = y + 5;
    // Create a black brush for drawing text
    let brush2 = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_Black()});
    // Set the font to Arial, size 16, and bold style
    let inputArialFont ="/Library/Fonts/ARIALUNI.TTF";
    let font2 = wasmModule.PdfTrueTypeFont.Create({
        fontFile: inputArialFont,
        size: 16,
        style: wasmModule.PdfFontStyle.Bold
    });
    // Create a string format for center alignment
    let format2 = wasmModule.PdfStringFormat.Create({alignment: wasmModule.PdfTextAlignment.Center});
    // Set character spacing to 1
    format2.CharacterSpacing = 1.0;
    // Set the text content
    let text = "Summary of Science";
    page.Canvas.DrawString({s: text, font: font2, brush: brush2, x: pageWidth / 2, y: y, format: format2});
    let size = font2.MeasureString({text: text, format: format2});
    y = y + size.Height + 6;
    // Icon
    const inputFileName3 = "Wikipedia_Science.png";
    let image = wasmModule.PdfImage.FromFile(inputFileName3)
    // Draw the image on the page at the specified position
    page.Canvas.DrawImage({
        image: image, point: wasmModule.PointF.Create(
            {x: pageWidth - image.PhysicalDimension.Width, y: y})
    });
    let imageLeftSpace = pageWidth - image.PhysicalDimension.Width - 2;
    // Calculate the bottom position for the icon
    let imageBottom = image.PhysicalDimension.Height + y;
    // Reference content
    inputArialFont = "/Library/Fonts/ARIALUNI.TTF";
    let font3 = wasmModule.PdfTrueTypeFont.Create({
        fontFile: inputArialFont,
        size: 9,
        style: wasmModule.PdfFontStyle.Regular
    });
    let format3 = wasmModule.PdfStringFormat.Create();
    // Set paragraph indentation to twice the size of the font
    format3.ParagraphIndent = font3.Size * 2;
    // Enable trailing spaces measurement
    format3.MeasureTrailingSpaces = true;
    // Set line spacing to 1.5 times the size of the font
    format3.LineSpacing = font3.Size * 1.5;
    let text1 = "(All text and picture from ";
    let text2 = "Wikipedia";
    let text3 = ", the free encyclopedia)";
    // Draw the first line of text on the page using the specified format
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
    y = y + size.Height
    // Content
    let format4 = wasmModule.PdfStringFormat.Create();
    const decoder = new TextDecoder('utf-8');
    // Read all text from a file
    text =decoder.decode(FS.readFile("Summary_of_Science.txt"));
    // Set the font to Arial, size 10
    let font5 = wasmModule.PdfTrueTypeFont.Create({
        fontFile: inputArialFont,
        size: 10,
        style: wasmModule.PdfFontStyle.Regular
    });
    // Set line spacing to 1.5 times the size of the fon
    format4.LineSpacing = font5.Size * 1.5;
    // Create a string layouter for measuring and arranging text
    let textLayouter = wasmModule.PdfStringLayouter.Create();
    // Calculate the height of the content block based on the image bottom position and y position
    let imageLeftBlockHeight = imageBottom - y;
    // Layout the text content using the specified format and size
    let result = textLayouter.Layout(text, font5, format4, wasmModule.SizeF.Create(
        {width: imageLeftSpace, height: imageLeftBlockHeight}));
    // If the content is too tall for the available space, increase the height by the line height
    if (result.ActualSize.Height < imageBottom - y) {
        imageLeftBlockHeight = imageLeftBlockHeight + result.LineHeight;
        result = textLayouter.Layout(text, font5, format4, wasmModule.SizeF.Create(
            {width: imageLeftSpace, height: imageLeftBlockHeight}));
    }


    let count = result.Lines.length;
   // Iterate through each line of text in the layout result
    for (let i = 0; i < count; i++) {
        let line = result.Lines[i];
        //Draw text
        page.Canvas.DrawString({s: line.Text, font: font5, brush: brush2, x: 0, y: y, format: format4});
        y = y + result.LineHeight;
    }
    // Create a text widget for rendering the remaining text content
    let textWidget = wasmModule.PdfTextWidget.Create({text: result.Remainder, font: font5, brush: brush2});
    // Create a text layout for specifying how to arrange the text content
    let textLayout = wasmModule.PdfTextLayout.Create();
    // Set the break type to fit the page width
    textLayout.Break = wasmModule.PdfLayoutBreakType.FitPage;
    // Set the layout type to paginate the text content
    textLayout.Layout = wasmModule.PdfLayoutType.Paginate;
    // Define the bounds of the text content area on the page
    let bounds = wasmModule.RectangleF.Create({
        location: wasmModule.PointF.Create({x: 0.0, y: y}),
        size: page.Canvas.ClientSize
    });
    // Set the string format for the text widget
    textWidget.StringFormat = format4;

    let layoutWidget = new wasmModule.PdfLayoutWidget(textWidget.H);
    layoutWidget.Draw({page: page, layoutRectangle: bounds, format: textLayout});
}

    const startProcessing = async () => {
      wasmModule = window.wasmModule;
      if (wasmModule) {
        // Load the ARIALUNI.TTF font file into the virtual file system (VFS)
        await wasmModule.FetchFileToVFS("ARIALUNI.TTF", "/Library/Fonts/", `${import.meta.env.BASE_URL}static/font/`);
        
        // Load the sample file into the virtual file system (VFS)
        let inputFileName = 'SetZoomFactor.pdf';
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);
        const inputFileName1 = "Header.png";
        await wasmModule.FetchFileToVFS(inputFileName1, '', `${import.meta.env.BASE_URL}static/data/`);
        
        const inputFileName2 = "Footer.png";
        await wasmModule.FetchFileToVFS(inputFileName2, '', `${import.meta.env.BASE_URL}static/data/`);

        const inputFileName3 = "Wikipedia_Science.png";
        await wasmModule.FetchFileToVFS(inputFileName3, '', `${import.meta.env.BASE_URL}static/data/`);

        const inputFileName4 = "Summary_of_Science.txt";
        await wasmModule.FetchFileToVFS(inputFileName4, '', `${import.meta.env.BASE_URL}static/data/`);

        // Create a pdf document
        let doc = wasmModule.PdfDocument.Create();
        doc.ViewerPreferences.PageLayout = wasmModule.PdfPageLayout.TwoColumnLeft;
        // Set the margin
        let unitCvtr = wasmModule.PdfUnitConvertor.Create();
        let margin = wasmModule.PdfMargins.Create();
        margin.Top = unitCvtr.ConvertUnits(2.54, wasmModule.PdfGraphicsUnit.Centimeter, wasmModule.PdfGraphicsUnit.Point);
        margin.Bottom = margin.Top;
        margin.Left = unitCvtr.ConvertUnits(3.17, wasmModule.PdfGraphicsUnit.Centimeter, wasmModule.PdfGraphicsUnit.Point);
        margin.Right = margin.Left;
        SetDocumentTemplate(doc, wasmModule.PdfPageSize.A4(), margin);
        // Create one section
        let section = doc.Sections.Add();
        section.PageSettings.Size = wasmModule.PdfPageSize.A4();
        section.PageSettings.Margins = wasmModule.PdfMargins.Create({margin: 0});
        SetSectionTemplate(section, wasmModule.PdfPageSize.A4(), margin, "Section 1");
        // Create one page
        let page = section.Pages.Add();
        // Draw page
        DrawPage(page);
        page = section.Pages.Add();
        DrawPage(page);
        page = section.Pages.Add();
        DrawPage(page);
        page = section.Pages.Add();
        DrawPage(page);
        page = section.Pages.Add();
        DrawPage(page);


        // Define the output file name
        const outputFileName = "Template_result.pdf";

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
