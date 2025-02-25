<template>
  <span>The following example demonstrates how to work with pages transition.</span>
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
function DrawPage(page) {
    let pageWidth = page.Canvas.ClientSize.Width;
    let y = 0.0;
    //Title
    y = y + 5;
    let brush2 = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_Black()});
    let inputArialFont = "/Library/Fonts/ARIALUNI.TTF";
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
    const inputFileName3 = "Wikipedia_Science.png";
    let image = wasmModule.PdfImage.FromFile(inputFileName3);

    page.Canvas.DrawImage({
        image: image, point: wasmModule.PointF.Create(
            {x: pageWidth - image.PhysicalDimension.Width, y: y})
    });
    let imageLeftSpace = pageWidth - image.PhysicalDimension.Width - 2;
    let imageBottom = image.PhysicalDimension.Height + y;
    // Reference content
    inputArialFont = "/Library/Fonts/ARIALUNI.TTF";
    let font3 = wasmModule.PdfTrueTypeFont.Create({
        fontFile: inputArialFont,
        size: 12,
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
    const decoder = new TextDecoder('utf-8');
    text =decoder.decode(FS.readFile("Summary_of_Science.txt"));

    let font5 = wasmModule.PdfTrueTypeFont.Create({
        fontFile: inputArialFont,
        size: 10,
        style: wasmModule.PdfFontStyle.Regular
    });
    format4.LineSpacing = font5.Size * 1.5;
    let textLayouter = wasmModule.PdfStringLayouter.Create();
    let imageLeftBlockHeight = imageBottom - y;
    let result = textLayouter.Layout(text, font5, format4, wasmModule.SizeF.Create(
        {width: imageLeftSpace, height: imageLeftBlockHeight}));
    if (result.ActualSize.Height < imageBottom - y) {
        imageLeftBlockHeight = imageLeftBlockHeight + result.LineHeight;
        result = textLayouter.Layout(text, font5, format4, wasmModule.SizeF.Create(
            {width: imageLeftSpace, height: imageLeftBlockHeight}));
    }

    let lines = result.Lines;
    let count = lines.length;
    for (let i = 0; i < count; i++) {
        let line = lines[i];
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

    let layoutWidget = new wasmModule.PdfLayoutWidget(textWidget.H);
    layoutWidget.Draw({page: page, layoutRectangle: bounds, format: textLayout});

}
    const startProcessing = async () => {
      wasmModule = window.wasmModule;
      if (wasmModule) {
        // Load the ARIALUNI.TTF font file into the virtual file system (VFS)
        await wasmModule.FetchFileToVFS("ARIALUNI.TTF", "/Library/Fonts/", `${import.meta.env.BASE_URL}static/font/`);

        // Load the sample file into the virtual file system (VFS)
        let inputFileName = 'ViewerPreference.pdf';
        await wasmModule.FetchFileToVFS(inputFileName, '', `${import.meta.env.BASE_URL}static/data/`);
        //Icon
        const inputFileName3 = "Wikipedia_Science.png";
        await wasmModule.FetchFileToVFS(inputFileName3, '', `${import.meta.env.BASE_URL}static/data/`);
        
        await wasmModule.FetchFileToVFS("Summary_of_Science.txt", '', `${import.meta.env.BASE_URL}static/data/`);
       
       // Create a pdf document
        let doc = wasmModule.PdfDocument.Create();
        doc.ViewerPreferences.PageMode = wasmModule.PdfPageMode.FullScreen;
        //Set the margin
        let unitCvtr = wasmModule.PdfUnitConvertor.Create();
        let margin = wasmModule.PdfMargins.Create();
        margin.Top = unitCvtr.ConvertUnits(2.54, wasmModule.PdfGraphicsUnit.Centimeter, wasmModule.PdfGraphicsUnit.Point);
        margin.Bottom = margin.Top;
        margin.Left = unitCvtr.ConvertUnits(3.17, wasmModule.PdfGraphicsUnit.Centimeter, wasmModule.PdfGraphicsUnit.Point);
        margin.Right = margin.Left;
        //Create one section
        let section = doc.Sections.Add();
        // Configure the transition settings for the section's pages
        section.PageSettings.Size = wasmModule.PdfPageSize.A4();
        section.PageSettings.Margins = margin;
        section.PageSettings.Transition.Duration = 2.0;
        section.PageSettings.Transition.Style = wasmModule.PdfTransitionStyle.Fly;
        section.PageSettings.Transition.PageDuration = 1.0;
        // Add a new page to the section with a red background color and draw its content
        let page = section.Pages.Add();
        page.BackgroundColor = wasmModule.Color.get_Red();
        DrawPage(page);
        // Add another page to the section with a green background color and draw its content
        page = section.Pages.Add();
        page.BackgroundColor = wasmModule.Color.get_Green();
        DrawPage(page);
        // Add a third page to the section with a blue background color and draw its content
        page = section.Pages.Add();
        page.BackgroundColor = wasmModule.Color.get_Blue();
        DrawPage(page);
        // Create a new section in the document with the same settings as the previous section
        section = doc.Sections.Add();
        section.PageSettings.Size = wasmModule.PdfPageSize.A4();
        section.PageSettings.Margins = margin;
        // Configure the transition settings for this section's pages
        section.PageSettings.Transition.Duration = 2.0;
        section.PageSettings.Transition.Style = wasmModule.PdfTransitionStyle.Box;
        section.PageSettings.Transition.PageDuration = 1.0;
        // Add a new page to the section with an orange background color and draw its content
        page = section.Pages.Add();
        page.BackgroundColor = wasmModule.Color.get_Orange();
        DrawPage(page);
        // Add another page to the section with a brown background color and draw its content
        page = section.Pages.Add();
        page.BackgroundColor = wasmModule.Color.get_Brown();
        DrawPage(page);
        // Add a third page to the section with a navy background color and draw its content
        page = section.Pages.Add();
        page.BackgroundColor = wasmModule.Color.get_Navy();
        DrawPage(page);
        // Create another section in the document with the same settings as before
        section = doc.Sections.Add();
        section.PageSettings.Size = wasmModule.PdfPageSize.A4();
        section.PageSettings.Margins = margin;
        // Configure the transition settings for this section's pages
        section.PageSettings.Transition.Duration = 2.0;
        section.PageSettings.Transition.Style = wasmModule.PdfTransitionStyle.Split;
        section.PageSettings.Transition.Dimension = wasmModule.PdfTransitionDimension.Vertical;
        section.PageSettings.Transition.Motion = wasmModule.PdfTransitionMotion.Inward;
        section.PageSettings.Transition.PageDuration = 1.0;
        // Add a new page to the section with an orange background color and draw its content
        page = section.Pages.Add();
        page.BackgroundColor = wasmModule.Color.get_Orange();
        DrawPage(page);
        // Add another page to the section with a brown background color and draw its content
        page = section.Pages.Add();
        page.BackgroundColor = wasmModule.Color.get_Brown();
        DrawPage(page);
        // Add a third page to the section with a navy background color and draw its content
        page = section.Pages.Add();
        page.BackgroundColor = wasmModule.Color.get_Navy();

        // Define the output file name
        const outputFileName = "Transition_result.pdf";

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
