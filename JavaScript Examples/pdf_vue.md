# PDF Document Creation with Hello World
## Demonstrates how to create a PDF document and add "Hello World" text
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();
// Create one page
let pagebase = doc.Pages.Add();

const text = "Hello World";
let pdffont = wasmModule.PdfFont.Create(wasmModule.PdfFontFamily.Helvetica, 30);
let pdfBrush = wasmModule.PdfSolidBrush.Create({color:wasmModule.Color.get_Blue()});
// Draw the text
pagebase.Canvas.DrawString({s: text, font: pdffont, brush: pdfBrush, x: 10, y: 10});
```

---

# Spire.PDF JavaScript Text Border
## Add border around text in PDF document
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();
// Create one page
let pagebase = doc.Pages.Add();

const text = "Hello World";

// Create the font to use and set the font style 
let trueTypeFont = wasmModule.PdfTrueTypeFont.Create({
    fontName: 'Arial Unicode MS',
    emSize: 12,
    style: wasmModule.FontStyle.Regular,
    unicode:true
});

// Measure the size of the text
let size = trueTypeFont.MeasureString({text:text});

// Create a PdfSolidBrush instance for setting the color of text
let pdfRGBColor = wasmModule.PdfRGBColor.Create({red: 200, green: 100, blue: 0});
let pdfBrush = wasmModule.PdfSolidBrush.Create({pdfRGBColor:pdfRGBColor});
let x = 60;
let y = 100;

// Draw the text on page
pagebase.Canvas.DrawString({s: text, font: trueTypeFont, brush: pdfBrush, x: x, y: y});

// Draw border for text   
let pen = wasmModule.PdfPen.Create({pdfRgbColor:pdfRGBColor, width:1});
let rect = wasmModule.RectangleF.Create({x: x, y: y, width: size.Width, height: size.Height});
pagebase.Canvas.DrawRectangle({pen: pen, rectangle: rect});
```

---

# Spire.PDF JavaScript Tooltip
## Add tooltip to text in PDF document
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();
// Create one page
let page = doc.Pages.Add();

let str = "Move the mouse cursor over the following text to display a tooltip";

let font = wasmModule.PdfTrueTypeFont.Create({
    fontName: 'Arial Unicode MS',
    emSize: 15,
    style: wasmModule.FontStyle.Regular,
    unicode:true
});

let location = wasmModule.PointF.Create({x: 10, y: 20});
page.Canvas.DrawString({s: str, font: font, brush:wasmModule.PdfBrushes.get_Black(), point: location});

//Define the text and its style
let text1 = "Your Office Development Master";
let font1 = wasmModule.PdfTrueTypeFont.Create({
    fontName: 'Arial Unicode MS',
    emSize: 18,
    style: wasmModule.FontStyle.Regular,
    unicode:true
});

let size1 = font1.MeasureString(text1);
let location1 = wasmModule.PointF.Create({x: 100, y: 100});
let rect1 = wasmModule.RectangleF.Create({location: location1, size: size1});
let brush1 = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_Blue()});

//Draw text
page.Canvas.DrawString({s: text1, font: font1, brush: brush1, layoutRectangle: rect1});

//Create invisible button on text position
let name1 = "field1";
let field1 = wasmModule.PdfButtonField.Create(page, name1);
field1.Bounds = rect1;
field1.ToolTip = "E-iceblue Co. Ltd., a vendor of .NET, Java, Cpp, Python and Javascript development components";

//Set no border for field
field1.BorderWidth = 0;

//Set backcolor and forcolor for field
field1.BackColor = wasmModule.Color.Transparent;
field1.ForeColor = wasmModule.Color.Transparent;
field1.LayoutMode = wasmModule.PdfButtonLayoutMode.IconOnly;
field1.IconLayout.IsFitBounds = true;

//Define the text and its style
let text2 = "Spire.PDF";
let font2 = wasmModule.PdfFont.Create(wasmModule.PdfFontFamily.TimesRoman, 20);
let size2 = font2.MeasureString(text2);
let rect2 = wasmModule.RectangleF.Create({x: 100, y: 160, width: size2.Width, height: size2.Height});
//Draw text
page.Canvas.DrawString({
    s: text2,
    font: font2,
    brush: wasmModule.PdfBrushes.get_DarkOrange(),
    layoutRectangle: rect2
});
//Create invisible button on text position
let name2 = "field2";
let field2 = wasmModule.PdfButtonField.Create(page, name2);
field2.Bounds = rect2;

field2.ToolTip = "A professional PDF library applied to creating," +
    "writing, editing, handling and reading PDF files" +
    "without any external dependencies.";
field2.BorderWidth = 0;
field2.BackColor = wasmModule.Color.Transparent;
field2.ForeColor = wasmModule.Color.Transparent;
field2.LayoutMode = wasmModule.PdfButtonLayoutMode.IconOnly;
field2.IconLayout.IsFitBounds = true;

//Add the buttons to pdf form
doc.AllowCreateForm = true;
doc.Form.Fields.Add(field1);
doc.Form.Fields.Add(field2);
```

---

# Spire.PDF JavaScript Transparent Text
## Add transparent text to PDF document
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();

// Create one A4 page
let page = doc.Pages.Add({size: wasmModule.PdfPageSize.A4(), margins: wasmModule.PdfMargins.Create(0)});
page.Canvas.Save();

// Set transparency for page
let alpha = 0.25;
page.Canvas.SetTransparency({alphaPen: alpha, alphaBrush: alpha, blendMode: wasmModule.PdfBlendMode.Normal});

// Create rectangle with specified dimensions
let rect = wasmModule.RectangleF.Create({x: 50, y: 50, width: 450, height: page.Size.Height});

// Create transparent text
let text = "Spire.PDF for .NET,a professional PDF library applied to" +
    " creating, writing, editing, handling and reading PDF files" +
    " without any external dependencies within .NET" +
    "( C#, VB.NET, ASP.NET, .NET Core) application.";
text += "\n\n\n\n\n";
text += "Spire.PDF for Java,a PDF Java API that enables" +
    "developers to read, write, convert and print PDF documents" +
    "in Java applications without using Adobe Acrobat.";

// Create brush from color channel
let pdfRGBColor = wasmModule.PdfRGBColor.Create({red: 0, green: 255, blue: 0});
let brush = wasmModule.PdfSolidBrush.Create({pdfRGBColor: pdfRGBColor});

// Draw the text
let font = wasmModule.PdfFont.Create(wasmModule.PdfFontFamily.Helvetica, 14);
page.Canvas.DrawString({s: text, font: font, brush: brush, layoutRectangle: rect});

page.Canvas.Restore();
```

---

# spire.pdf javascript text rotation
## draw rotated text in pdf document
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();
// Create one page
let page = doc.Pages.Add();

let font = wasmModule.PdfTrueTypeFont.Create({
    fontName: 'Arial Unicode MS',
    emSize: 12,
    style: wasmModule.FontStyle.Regular,
    unicode:true
});

let brush = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_Blue()});

let text = "This is a text";

//Draw text before rotating Canvas
page.Canvas.DrawString({s:text, font:font, brush:brush, x:20, y:30});

//Draw text before rotating Canvas
page.Canvas.DrawString({s:text, font:font, brush:brush, x:20, y:150});

//Create PdfGraphicsState instance to save the state of page Canvas
let state = page.Canvas.Save();

let point1 = wasmModule.PointF.Create({x:20,y:30});

//Rotate Canvas 90 degrees clockwise
page.Canvas.RotateTransform({angle:90, point:point1});

//Draw text in rotated Canvas
page.Canvas.DrawString({s:text, font:font, brush:brush, point:point1});

//Restores the state of this page Canvas to the state represented by a PdfGraphicsState.
page.Canvas.Restore({state:state});

//Redrawing a new text requires initializing a new state
let state2 = page.Canvas.Save();

let point2 = wasmModule.PointF.Create({x:20,y:150});

//Rotate Canvas 90 degrees counterclockwise
page.Canvas.RotateTransform({angle:-90, point:point2});

//Draw text in rotated Canvas
page.Canvas.DrawString({s:text, font:font, brush:brush, point:point2});

//Restores the state of this page Canvas to the state represented by a PdfGraphicsState.
page.Canvas.Restore({state:state2});
```

---

# Spire.PDF JavaScript Text Drawing
## Demonstrates how to draw text in a PDF document with various formatting options
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();

// Create one page
let page = doc.Pages.Add();

// Draw the text - brush
DrawTextInPage(page);

// Draw the text - alignment
AlignText(page);

// Draw the text - align in rectangle
AlignTextInRectangle(page);

// Draw the text - transform
TransformText(page);
RotateText(page);

function AlignText(page) {
    let font = wasmModule.PdfFont.Create({fontFamily:wasmModule.PdfFontFamily.Helvetica,size:20});
    let brush = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_Blue()});

    // Draws left-aligned text
    let leftAlignment = wasmModule.PdfStringFormat.Create({alignment:wasmModule.PdfTextAlignment.Left, lineAlignment:wasmModule.PdfVerticalAlignment.Middle});
    page.Canvas.DrawString({s:"Left!", font:font, brush:brush, x:0, y:20, format:leftAlignment});
    page.Canvas.DrawString({s:"Left!", font:font, brush:brush, x:0, y:50, format:leftAlignment});

    // Draws right-aligned text
    let rightAlignment = wasmModule.PdfStringFormat.Create({alignment:wasmModule.PdfTextAlignment.Right, lineAlignment:wasmModule.PdfVerticalAlignment.Middle});
    page.Canvas.DrawString({s:"Right!", font:font, brush:brush,x: page.Canvas.ClientSize.Width, y:20, format:rightAlignment});
    page.Canvas.DrawString({s:"Right!", font:font, brush:brush,x: page.Canvas.ClientSize.Width, y:50, format:rightAlignment});

    // Draws center-aligned text
    let centerAlignment = wasmModule.PdfStringFormat.Create({alignment:wasmModule.PdfTextAlignment.Center, lineAlignment:wasmModule.PdfVerticalAlignment.Middle});
    page.Canvas.DrawString({s:"Go! Turn Around! Go! Go! Go!", font:font, brush:brush, x:page.Canvas.ClientSize.Width / 2,y: 40, format:centerAlignment});
}

function AlignTextInRectangle(page) {
    // Create the font to use and set the font style
    let font = wasmModule.PdfFont.Create({fontFamily:wasmModule.PdfFontFamily.Helvetica,size:10});
    let brush = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_Blue()});

    let rctg1 = new wasmModule.RectangleF.Create({x:0,y: 70,width: page.Canvas.ClientSize.Width / 2, height:100});
    let rctg2 = new wasmModule.RectangleF.Create({x:page.Canvas.ClientSize.Width / 2,y: 70, width:page.Canvas.ClientSize.Width / 2,height: 100});

    // Draw rectangle and specifies its fill color and position brush, rectangle
    let brush1 = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_LightBlue()});
    let brush2 = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_LightSkyBlue()});
    page.Canvas.DrawRectangle({brush:brush1, rectangle:rctg1});
    page.Canvas.DrawRectangle({brush:brush2, rectangle:rctg2});

    // Draws left-aligned text s, font, brush, layoutRectangle, format
    let leftAlignment = new wasmModule.PdfStringFormat.Create({alignment:wasmModule.PdfTextAlignment.Left, lineAlignment:wasmModule.PdfVerticalAlignment.Top});
    page.Canvas.DrawString({s:"Left! Left!", font:font, brush:brush,layoutRectangle: rctg1, format:leftAlignment});
    page.Canvas.DrawString({s:"Left! Left!", font:font, brush:brush,layoutRectangle: rctg2, format:leftAlignment});

    // Draws right-aligned text
    let rightAlignment = new wasmModule.PdfStringFormat.Create({alignment:wasmModule.PdfTextAlignment.Right, lineAlignment:wasmModule.PdfVerticalAlignment.Middle});
    page.Canvas.DrawString({s:"Right! Right!", font:font, brush:brush,layoutRectangle: rctg1, format:rightAlignment});
    page.Canvas.DrawString({s:"Right! Right!", font:font, brush:brush,layoutRectangle: rctg2, format:rightAlignment});

    // Draws center-aligned text
    let centerAlignment = new wasmModule.PdfStringFormat.Create({alignment:wasmModule.PdfTextAlignment.Center, lineAlignment:wasmModule.PdfVerticalAlignment.Bottom});
    page.Canvas.DrawString({s:"Go! Turn Around! Go! Go! Go!", font:font, brush:brush,layoutRectangle: rctg1, format:centerAlignment});
    page.Canvas.DrawString({s:"Go! Turn Around! Go! Go! Go!", font:font, brush:brush,layoutRectangle: rctg2, format:centerAlignment});
}

function RotateText(page) {
    // Save graphics state
    let state = page.Canvas.Save();

    // Draw the text - transform
    let font = wasmModule.PdfFont.Create({fontFamily:wasmModule.PdfFontFamily.Helvetica,size:10});
    let brush = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_Blue()});

    let centerAlignment = new wasmModule.PdfStringFormat.Create({alignment:wasmModule.PdfTextAlignment.Left, lineAlignment:wasmModule.PdfVerticalAlignment.Middle});
    let x = page.Canvas.ClientSize.Width / 2;
    let y = 380;

    page.Canvas.TranslateTransform(x, y);
    for (let i = 0; i < 12; i++)
    {
        // Rotate Canvas
        page.Canvas.RotateTransform({angle:30});
        // Draw text
        page.Canvas.DrawString({s:"Go! Turn Around! Go! Go! Go!", font:font, brush:brush, x:20,y: 0, format:centerAlignment});
    }

    // Restore graphics
    page.Canvas.Restore({state});
}

function TransformText(page) {
    // Save graphics state
    let state = page.Canvas.Save();

    // Draw the text - transform
    let font = wasmModule.PdfFont.Create({fontFamily:wasmModule.PdfFontFamily.Helvetica,size:18});
    let brush1 = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_DeepSkyBlue()});
    let brush2 = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_CadetBlue()});

    page.Canvas.TranslateTransform(20, 200);
    page.Canvas.ScaleTransform(1, 0.6);
    page.Canvas.SkewTransform(-10, 0);
    page.Canvas.DrawString({s:"Go! Turn Around! Go! Go! Go!", font:font, brush:brush1,x: 0,y: 0});

    page.Canvas.SkewTransform(10, 0);
    page.Canvas.DrawString({s:"Go! Turn Around! Go! Go! Go!", font:font, brush:brush2,x: 0,y: 0});

    page.Canvas.ScaleTransform(1, -1);
    page.Canvas.DrawString({s:"Go! Turn Around! Go! Go! Go!", font:font, brush:brush2,x: 0,y: -2*18});

    // Restore graphics
    page.Canvas.Restore({state:state});
}

function DrawTextInPage(page) {
    // Save graphics state
    let state = page.Canvas.Save();
    // Draw text - brush
    let text = "Go! Turn Around! Go! Go! Go!";
    let pen = wasmModule.PdfPens.get_DeepSkyBlue();
    let brush = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_White()});

    let format = wasmModule.PdfStringFormat.Create();//fontFamily, size, style
    let font = wasmModule.PdfFont.Create({fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 18, style: wasmModule.PdfFontStyle.Italic});

    let size = font.MeasureString({text: text, format: format});

    let rctg = wasmModule.RectangleF.Create({x: page.Canvas.ClientSize.Width / 2 + 10, y: 180, width: size.Width / 3 * 2, height: size.Height * 2});

    page.Canvas.DrawString({s: text, font: font, pen: pen, brush: brush, layoutRectangle: rctg, format: format});
    // Restore graphics
    page.Canvas.Restore({state:state});
}
```

---

# spire.pdf javascript gradient text
## draw text with gradient color in pdf
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();
// Create one page
let page = doc.Pages.Add();

//Create a rectangle
let pointf = wasmModule.PointF.Create({x:0, y:0});
let sizef = wasmModule.SizeF.Create({width:300, height:100});
let rectf = wasmModule.RectangleF.Create({location:pointf, size:sizef});

//Create a brush with gradient
let color1 = wasmModule.PdfRGBColor.Create({color:wasmModule.Color.get_Red()});
let color2 = wasmModule.PdfRGBColor.Create({color:wasmModule.Color.get_Blue()});
let horizontal = wasmModule.PdfLinearGradientMode.Horizontal;
let brush = wasmModule.PdfLinearGradientBrush.Create({rect:rectf, color1:color1, color2:color2, mode:horizontal});

//Create a true type font with size 20f
let font = wasmModule.PdfFont.Create(wasmModule.PdfFontFamily.Helvetica, 20);

//Draw text
let textpoint = wasmModule.PointF.Create({x:0, y:0});
page.Canvas.DrawString({s:"Welcome to E-iceblue!", font:font, brush:brush, point:textpoint});
```

---

# PDF Highlighted Text Extraction
## Extract highlighted text from PDF annotations
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();

// Load a pdf file
doc.LoadFromFile({fileName: inputFileName});    
let page = doc.Pages.get_Item(0);

let textMarkupAnnotation;
let outContent = "Extracted hightlighted text:";
let pdfTextExtractor = spirepdf.PdfTextExtractor.Create(page);

// Get PdfTextMarkupAnnotationWidget objects
for (let i = 0; i < page.Annotations.Count; i++) {
    if (page.Annotations.get_Item(i) instanceof wasmModule.PdfTextMarkupAnnotationWidget) {
        textMarkupAnnotation = page.Annotations.get_Item(i);
        // Get the highlighted text
        let pdfTextExtractOptions = wasmModule.PdfTextExtractOptions.Create();
        pdfTextExtractOptions.ExtractArea = textMarkupAnnotation.Bounds;
        outContent = outContent + pdfTextExtractor.ExtractText(pdfTextExtractOptions);

        // Get the highlighted color
        let color = textMarkupAnnotation.TextMarkupColor;
    }
}
```

---

# PDF Text Extraction
## Extract text from a particular page of a PDF document
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();

//Load a pdf file
doc.LoadFromFile({fileName: inputFileName});

// Get the first page
let page = doc.Pages.get_Item(0);

// Extract text from page keeping white space
let options = wasmModule.PdfTextExtractOptions.Create();
options.IsExtractAllText = true; //false->Extract text from page without keeping white space
let pdfTextExtractor = wasmModule.PdfTextExtractor.Create(page);
let text = pdfTextExtractor.ExtractText(options);
```

---

# PDF Text Extraction
## Extract text from a specific area of a PDF document
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();

//Load a pdf file
doc.LoadFromFile({fileName: inputFileName});

// Get the first page
let page = doc.Pages.get_Item(0);

// Define options for text extraction
let options = wasmModule.PdfTextExtractOptions.Create();
options.ExtractArea = wasmModule.RectangleF.Create({x:80, y:180, width:500,height: 200});

// Create a PdfTextExtractor object and extract text using the specified options
let pdfTextExtractor = wasmModule.PdfTextExtractor.Create(page);
let text = pdfTextExtractor.ExtractText(options);
```

---

# Spire.PDF JavaScript Text Finding
## Find and highlight specific text in a PDF document
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();

// Load a pdf file
doc.LoadFromFile({fileName: inputFileName});

for (let i = 0; i < doc.Pages.Count; i++) {
    let page = doc.Pages.get_Item(i);
    // Create a PdfTextFinder object for the current page
    let finder = wasmModule.PdfTextFinder.Create(page);
    finder.Options.Parameter = wasmModule.TextFindParameter.WholeWord;
    // Find the occurrences of the specified text
    let finds = finder.Find("science");
    
    // Highlight the found text
    for (let j = 0; j < finds.length; j++) {
        finds[j].HighLight();
    }
}

// Save the document to the specified path
doc.SaveToFile({fileName: outputFileName});
doc.Close();
```

---

# Spire.PDF JavaScript Text Search
## Find and highlight text in a specified rectangle area of a PDF document
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();

// Load a pdf file
doc.LoadFromFile({fileName: inputFileName});

// Define a rectangle to specify the search area
let rctg = wasmModule.RectangleF.Create({x: 0, y: 0, width: 300, height: 300});

// Get the first page
let pdfPageBase = doc.Pages.get_Item(0);

// Create a PdfTextFinder object for the first page
let finder = wasmModule.PdfTextFinder.Create(pdfPageBase);
finder.Options.Parameter = wasmModule.TextFindParameter.WholeWord;
finder.Options.Area = rctg;

// Find text in the rectangle
let finds = finder.Find("Spire");

// Highlight the found text
for (let j = 0; j < finds.length; j++) {
    finds[j].HighLight({color: wasmModule.Color.get_Green()});
}
```

---

# Spire.PDF JavaScript Font
## Working with different font types in PDF documents
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();

// Get one page
let page = doc.Pages.Add();

let l = page.Canvas.ClientSize.Width / 2;
let center = wasmModule.PointF.Create({x: l, y: l});
let r = Math.sqrt(2 * l * l);

let blue = wasmModule.PdfRGBColor.Create({color: wasmModule.Color.get_Blue()});
let red = wasmModule.PdfRGBColor.Create({color: wasmModule.Color.get_Red()});
let brush = wasmModule.PdfRadialGradientBrush.Create({
    centreStart: center,
    radiusStart: 0,
    centreEnd: center,
    radiusEnd: r,
    colorStart: blue,
    colorEnd: red
});

// Working with different font families
let fontFamilies = ["Helvetica", "Courier", "TimesRoman", "Symbol", "ZapfDingbats"];
let y = 100;
for (let i = 0; i < fontFamilies.length; i++) {
    let text = "Font Family:" + fontFamilies[i];

    let x1 = 40;
    y = 100 + i * 16;

    // Define font
    let font1 = wasmModule.PdfFont.Create({fontFamily: wasmModule.PdfFontFamily.Courier, size: 14});
    let font2 = wasmModule.PdfFont.Create({fontFamily: fontFamilies[i], size: 14});

    // Measure the width of text
    let x2 = x1 + 10 + font1.MeasureString({text: text}).Width;

    // Draw text
    page.Canvas.DrawString({s: text, font: font1, brush: brush, x: x1, y: y});
    page.Canvas.DrawString({s: text, font: font2, brush: brush, x: x2, y: y});
}

// True type font - embedded
let trueTypeFont = wasmModule.PdfFont.Create({
    fontFamily: wasmModule.PdfFontFamily.Helvetica,
    size: 15,
    style: wasmModule.PdfFontStyle.Bold
});
y = y + 26;
page.Canvas.DrawString({s: "Font Family: Arial - Embedded", font: trueTypeFont, brush: brush, x: 40, y: y});

// Right to left text (Arabic)
let arabicText
    = "\u0627\u0644\u0630\u0647\u0627\u0628\u0021\u0020"
    + "\u0628\u062F\u0648\u0631\u0647\u0020\u062D\u0648\u0644\u0647\u0627\u0021\u0020"
    + "\u0627\u0644\u0630\u0647\u0627\u0628\u0021\u0020"
    + "\u0627\u0644\u0630\u0647\u0627\u0628\u0021\u0020"
    + "\u0627\u0644\u0630\u0647\u0627\u0628\u0021";

let ARIALUNIFont = wasmModule.PdfTrueTypeFont.Create({
    fontName: 'Arial Unicode MS',
    emSize: 15,
    style: wasmModule.FontStyle.Regular,
    unicode:true
});

y = y + 26;
let pointf = wasmModule.PointF.Create({x: 40, y: y});
let rctg = wasmModule.RectangleF.Create({location: pointf, size: page.Canvas.ClientSize});

// Define the format of string
let format = wasmModule.PdfStringFormat.Create({alignment: wasmModule.PdfTextAlignment.Right});
format.RightToLeft = true;

// Draw text with right-to-left format
page.Canvas.DrawString({s: arabicText, font: ARIALUNIFont, brush: brush, layoutRectangle: rctg, format: format});

// True type font - not embedded
trueTypeFont = wasmModule.PdfFont.Create({
    fontFamily: wasmModule.PdfFontFamily.Helvetica,
    size: 14,
    style: wasmModule.PdfFontStyle.Italic
});
y = y + 26;
page.Canvas.DrawString({s: "Font Family: Batang - Not Embedded", font: trueTypeFont, brush: brush, x: 40, y: y});

// CJK font - Chinese
y = y + 36;
let cjkFont = wasmModule.PdfCjkStandardFont.Create({
    fontFamily: wasmModule.PdfCjkFontFamily.MonotypeHeiMedium,
    size: 14
});
page.Canvas.DrawString({s: "How to say 'Font' in Chinese? \u5B57\u4F53", font: cjkFont, brush: brush, x: 40, y: y});

// CJK font - Japanese
y = y + 36;
cjkFont = wasmModule.PdfCjkStandardFont.Create({
    fontFamily: wasmModule.PdfCjkFontFamily.HanyangSystemsGothicMedium,
    size: 14
});
page.Canvas.DrawString({
    s: "How to say 'Font' in Japanese? \u30D5\u30A9\u30F3\u30C8",
    font: cjkFont,
    brush: brush,
    x: 40,
    y: y
});

// CJK font - Korean
y = y + 36;
cjkFont = wasmModule.PdfCjkStandardFont.Create({
    fontFamily: wasmModule.PdfCjkFontFamily.HanyangSystemsShinMyeongJoMedium,
    size: 14
});
page.Canvas.DrawString({s: "How to say 'Font' in Korean? \uAE00\uAF34", font: cjkFont, brush: brush, x: 40, y: y});
```

---

# PDF Text Search Details
## Extract details of searched text in a PDF document
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();

// Load a pdf file
doc.LoadFromFile({fileName: inputFileName});

let page = doc.Pages.get_Item(0);

// Create a PdfTextFinder object for searching text within the page
let finder = wasmModule.PdfTextFinder.Create(page);
finder.Options.Parameter = wasmModule.TextFindParameter.IgnoreCase;

// Find occurrences of the specified text within the page
let results = finder.Find("Spire.PDF");

// Create a StringBuilder object to store the details of the searched text
let builder = "";

// Iterate through each found text fragment
for (let i = 0; i < results.length; i++) {
    let find = results[i];
    builder = builder + "==================================================================================" + "\r\n";
    // Append the matched text and the detail of matched text to the StringBuilder
    builder = builder + "Match Text: " + find.Text + "\r\n";
    builder = builder + "Size: " + find.Sizes[0].Width + "," + find.Sizes[0].Height + "\r\n";
    builder = builder + "Position: " + find.Positions[0].X + "," + find.Positions[0].Y + "\r\n";
    builder = builder + "The line that contains the searched text: " + find.LineText + "\r\n";
}
```

---

# spire.pdf javascript text measurement
## get text size based on font
```javascript
//Create an instance for PdfFont
let font1 = new wasmModule.PdfFont.Create({fontFamily: wasmModule.PdfFontFamily.TimesRoman, size: 12});

//Get text size based on font name and size
let sizeF1 = font1.MeasureString(text);

let font2 = wasmModule.PdfTrueTypeFont.Create({
    fontName: 'Arial Unicode MS',
    emSize: 12,
    style: wasmModule.FontStyle.Regular,
    unicode:true
});

//Get text size based on font name and size
let sizeF2 = font2.MeasureString({text: text});
```

---

# PDF Text Replacement
## Replace all occurrences of searched text in a PDF document
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();

//Load a pdf file
doc.LoadFromFile({fileName: inputFileName});

let page = doc.Pages.get_Item(0);

// Create a PdfTextFinder object for searching text within the page
let finder = wasmModule.PdfTextFinder.Create(page);
finder.Options.Parameter = wasmModule.TextFindParameter.IgnoreCase;

// Find occurrences of the specified text within the page
let finds = finder.Find("Spire.PDF for JavaScript");

let newText = "E-iceblue Spire.PDF";

// Creates a brush
let brush = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_DarkBlue()});

let font2 = wasmModule.PdfTrueTypeFont.Create({
    fontName: 'Arial Unicode MS',
    emSize: 9,
    style: wasmModule.FontStyle.Regular,
    unicode:true
});

let rec;

// Iterate through each found text fragment
for (let i = 0; i < finds.length; i++) {
    let find = finds[i];
    // Gets the bound of the found text in page
    rec = find.Bounds[0];

    page.Canvas.DrawRectangle({brush: wasmModule.PdfBrushes.get_White(), rectangle: rec});
    // Draws new text as defined font and color
    page.Canvas.DrawString({s: newText, font: font2, brush: brush, layoutRectangle: rec});

    // This method can directly replace old text with newText,but it just can set the background color, can not set font/forecolor
    // find.ApplyRecoverString(newText, Color.Gray);
}
```

---

# Spire.PDF JavaScript Text Replacement
## Replace the first searched text in a PDF document
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();

//Load a pdf file
doc.LoadFromFile({fileName: inputFileName});

let page = doc.Pages.get_Item(0);

// Create a PdfTextFinder object for searching text within the first page
let finder = wasmModule.PdfTextFinder.Create(page);
finder.Options.Parameter = wasmModule.TextFindParameter.IgnoreCase;

// Find occurrences of the specified text within the first page
let finds = finder.Find("Spire.PDF for JavaScript");

let newText = "E-iceblue Spire.PDF";

// Gets the first found object
let find = finds[0];

// Creates a brush
let brush = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_DarkBlue()});

let font = wasmModule.PdfTrueTypeFont.Create({
    fontName: 'Arial Unicode MS',
    emSize: 9,
    style: wasmModule.FontStyle.Regular,
    unicode:true
});

// Gets the bound of the first found text in page
let rec = find.Bounds[0];
page.Canvas.DrawRectangle({brush: wasmModule.PdfBrushes.get_White(), rectangle: rec});

// Draws new text as defined font and color
page.Canvas.DrawString({s: newText, font: font, brush: brush, layoutRectangle: rec});
```

---

# PDF Font Replacement
## Replace fonts in a PDF document with a new font
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();

// Load a pdf file
doc.LoadFromFile({fileName: inputFileName});

// Get the fonts used in PDF
let fonts = doc.UsedFonts;

let newfont = wasmModule.PdfTrueTypeFont.Create({
    fontName: 'Arial Unicode MS',
    emSize: 12,
    style: wasmModule.FontStyle.Regular,
    unicode: true
});

// Iterate through each used fonts
for (let i = 0; i < fonts.length; i++) {
    let font = fonts[i];
    // Replace the font with new font
    font.Replace(newfont);
}

// Save the document to the specified path
doc.SaveToFile({fileName: outputFileName});
doc.Close();
```

---

# Spire.PDF JavaScript Text Replacement
## Replace text in PDF documents
```javascript
// Get the first page of the pdf file
let page = doc.Pages.get_Item(0);

// Create a PdfTextReplacer using the first page
let replacer = wasmModule.PdfTextReplacer.Create(page);

// Replace all occurrences of "Spire.PDF" with "E-iceblue" in this page
replacer.ReplaceAllText({oldText: "Spire.PDF", newText: "E-iceblue"});

// Replace the first occurrence of "Adobe Acrobat" with "PDF editors"
replacer.ReplaceText("Adobe Acrobat", "PDF editors");
```

---

# PDF Text Search and Hyperlink Addition
## Search for specific text in a PDF document and add hyperlinks to all occurrences
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();

//Load a pdf file
doc.LoadFromFile({fileName: inputFileName});

// Get the first page of the pdf file
let page = doc.Pages.get_Item(0);

// Create a PdfTextFinder using the first page
let finder = wasmModule.PdfTextFinder.Create(page);

// Set the search parameter to ignore case
finder.Options.Parameter = wasmModule.TextFindParameter.IgnoreCase;

// Find all occurrences of "e-iceblue" in the PDF and store them in a list
let finds = finder.Find("e-iceblue");

// Define the hyperlink URL
let url = "http://www.e-iceblue.com";

// Iterate through each found text fragment
for (let i = 0; i < finds.length; i++) {
    // Create a PdfUriAnnotation object to add a hyperlink for the searched text
    let find = finds[i];
    let uri = wasmModule.PdfUriAnnotation.Create({rectangle: find.Bounds[0]});
    uri.Uri = url;
    uri.Border = wasmModule.PdfAnnotationBorder.Create({borderWidth: 1});
    uri.Color = wasmModule.PdfRGBColor.Create({color: spirepdf.Color.get_Blue()});

    // Add the annotation to the page's annotation widget
    page.Annotations.Add(uri);
}
```

---

# Spire.PDF JavaScript Text Search
## Search for text in a PDF document and draw rectangles around found text
```javascript
// Get the first page of the pdf file
let page = doc.Pages.get_Item(0);

// Create a PdfTextFinder object for searching text within the first page
let finder = wasmModule.PdfTextFinder.Create(page);
finder.Options.Parameter = wasmModule.TextFindParameter.IgnoreCase;

// Find occurrences of the specified text within the first page
let finds = finder.Find("Spire.PDF");

// Iterate through each found text fragment
for (let i = 0; i < finds.length; i++) {
    let find = finds[i];
    // Draw a rectangle with red pen brush, width
    let pen = wasmModule.PdfPen.Create({brush: wasmModule.PdfBrushes.get_Red(), width: 0.9});
    page.Canvas.DrawRectangle({pen: pen, rectangle: find.Bounds[0]});
}
```

---

# spire.pdf javascript text search
## search text with regular expression and replace
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();
//Load a pdf file
doc.LoadFromFile({fileName: inputFileName});

// Get the first page of the pdf file
let page = doc.Pages.get_Item(0);

// Create a PdfTextFinder object for searching text within the first page
let finder = wasmModule.PdfTextFinder.Create(page);
finder.Options.Parameter = wasmModule.TextFindParameter.Regex;

// Find occurrences of the specified text within the first page
let finds = finder.Find("\\d{4}");

let newText = "NewYear";

// Creates a brush
let brush = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_DarkBlue()});

let font = wasmModule.PdfTrueTypeFont.Create({
    fontName: 'Arial Unicode MS',
    emSize: 6,
    style: wasmModule.FontStyle.Regular,
    unicode:true
});

// Defines text horizontal/vertical center format
let centerAlign = wasmModule.PdfStringFormat.Create({
    alignment: wasmModule.PdfTextAlignment.Left,
    lineAlignment: wasmModule.PdfVerticalAlignment.Middle
});

let rec;
for (let i = 0; i < finds.length; i++) {
    let find = finds[i];
    // Gets the bound of the found text in page
    rec = find.Bounds[0];

    //brush, rectangle
    page.Canvas.DrawRectangle({brush: wasmModule.PdfBrushes.get_GreenYellow(), rectangle: rec});
    // Draws new text as defined font and color
    page.Canvas.DrawString({s: newText, font: font, brush: brush, layoutRectangle: rec, format: centerAlign});

    // This method can directly replace old text with newText,but it just can set the background color, can not set font/forecolor
    // find.ApplyRecoverString(newText, Color.Gray);
}
```

---

# Spire.PDF JavaScript Line Break
## Set line breaks in PDF text

```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();

// Create one A4 page
let page = doc.Pages.Add({size: wasmModule.PdfPageSize.A4(), margins: wasmModule.PdfMargins.Create(40)});
// Create brush from color channel
let brush = wasmModule.PdfSolidBrush.Create({color:wasmModule.Color.get_Black()});

// Create text with line breaks
let text = "Spire.PDF for JavaScript" +
    "\n" +
    "A professional PDF library applied to" +
    " creating, writing, editing, handling and reading PDF files" +
    " without any external dependencies.";

text += "\n\rSpire.PDF for Java" +
    "\n" +
    "A PDF Java API that enables developers to read, " +
    "write, convert and print PDF documents" +
    "in Java applications without using Adobe Acrobat.";
text += "\n\r";
text += "Welcome to evaluate Spire.PDF!";

// Create rectangle with specified dimensions
let rect = wasmModule.RectangleF.Create({x:50, y:50, width: page.Size.Width - 150,height: page.Size.Height});
let font=wasmModule.PdfFont.Create({fontFamily:wasmModule.PdfFontFamily.Helvetica,size:13});
// Draw the text with line breaks
page.Canvas.DrawString({s:text, font:font, brush:brush,layoutRectangle: rect});
```

---

# PDF Superscript and Subscript Text
## Demonstrates how to add superscript and subscript text in PDF documents
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();
// Create one page
let page = doc.Pages.Add();

let text = "Spire.PDF for JavaScript";
let font = wasmModule.PdfFont.Create(wasmModule.PdfFontFamily.Helvetica, 20);
let brush = wasmModule.PdfSolidBrush.Create({color:wasmModule.Color.get_Black()});

//Draw Superscript
DrawSuperscript(page, text, font, brush);

//Draw Subscript
DrawSubscript(page, text, font, brush);

function DrawSuperscript(page, text, font, brush) {
    let x = 120;
    let y = 100;
    page.Canvas.DrawString({s: text, font: font, brush: brush, point: wasmModule.PointF.Create({x: x, y: y})});

    //Measure the string
    let size = font.MeasureString({text: text});

    //Set the x coordinate of the superscript text
    x += size.Width;

    //Instantiate a PdfStringFormat instance
    let format = wasmModule.PdfStringFormat.Create();

    //Set format as superscript
    format.SubSuperScript = wasmModule.PdfSubSuperScript.SuperScript;

    //Draw superscript text with format
    text = "Superscript";
    let point = wasmModule.PointF.Create({x: x, y: y});
    //s, font, pen, point, format
    page.Canvas.DrawString({s: text, font: font, brush: brush, point: point, format: format});
}

function DrawSubscript(page, text, font, brush) {
    let x = 120;
    let y = 150;
    let point = wasmModule.PointF.Create({x: x, y: y});
    // s, font, brush, point
    page.Canvas.DrawString({s: text, font: font, brush: brush, point: point});

    //Measure the string
    let size = font.MeasureString({text: text});

    //Set the x coordinate of the superscript text
    x += size.Width;

    //Instantiate a PdfStringFormat instance
    let format = wasmModule.PdfStringFormat.Create();

    //Set format as superscript
    format.SubSuperScript = wasmModule.PdfSubSuperScript.SubScript;

    //Draw superscript text with format
    text = "SubScript";
    let point1 = wasmModule.PointF.Create({x: x, y: y});
    page.Canvas.DrawString({s: text, font: font, brush: brush, point: point1, format: format});
}
```

---

# PDF Text Layout
## Demonstrates how to work with text layout in PDF documents
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();

// Create one page
let page = doc.Pages.Add();
let pageWidth = page.Canvas.ClientSize.Width;
let y = 0;

// Page header
let pdfrgbColor = wasmModule.PdfRGBColor.Create({color: wasmModule.Color.get_LightGray()});
let pen1 = wasmModule.PdfPen.Create({pdfRgbColor: pdfrgbColor, width: 1});
let brush1 = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_LightGray()});

let font1 = wasmModule.PdfFont.Create(wasmModule.PdfFontFamily.Helvetica, 20);
let format1 = wasmModule.PdfStringFormat.Create({alignment: wasmModule.PdfTextAlignment.Right});
let text = "Demo of Spire.Pdf";

// Draw text
page.Canvas.DrawString({s: text, font: font1, brush: brush1, x: pageWidth - 2, y: y, format: format1});

// Measure the size of text
let size = font1.MeasureString({text: text, format: format1});
y = y + size.Height + 1;

// Draw the text of header
page.Canvas.DrawLine({pen: pen1, x1: 0, y1: y, x2: pageWidth, y2: y});

// Title
y = y + 25;
let brush2 = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_Black()});
let format2 = wasmModule.PdfStringFormat.Create({alignment: wasmModule.PdfTextAlignment.Center});
format2.CharacterSpacing = 1;
text = "Summary of Science";
page.Canvas.DrawString({s: text, font: font1, brush: brush2, x: pageWidth / 2, y: y, format: format2});
size = font1.MeasureString({text: text, format: format2});
y = y + size.Height + 16;

// Reference content
let font2 = wasmModule.PdfFont.Create(wasmModule.PdfFontFamily.Helvetica, 12);
// Define string format
let format3 = wasmModule.PdfStringFormat.Create();
format3.ParagraphIndent = font2.Size * 2;
format3.MeasureTrailingSpaces = true;
format3.LineSpacing = font2.Size * 1.5;
let text1 = "(All text and picture from ";
let text2 = "Wikipedia";
let text3 = ", the free encyclopedia)";

// Draw text
page.Canvas.DrawString({s: text1, font: font2, brush: brush2, x: 0, y: y, format: format3});

// Measure the size of text
size = font2.MeasureString({text: text1, format: format3});
let x1 = size.Width;
format3.ParagraphIndent = 0;

let brush3 = wasmModule.PdfBrushes.get_Blue();
page.Canvas.DrawString({s: text2, font: font2, brush: brush3, x: x1, y: y, format: format3});

// Measure the size of text
size = font2.MeasureString({text: text2, format: format3});
x1 = x1 + size.Width;

// Draw text
page.Canvas.DrawString({s: text3, font: font2, brush: brush2, x: x1, y: y, format: format3});
y = y + size.Height;

// Content
// Define the format of string
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

// Define the font style
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
    // Draw text
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
```

---

# PDF Text Wrapping Around Image
## Demonstrates how to place text around image in PDF document
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();

// Create one page
let page = doc.Pages.Add();

// Gets page width
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

// Set line spacing
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
```

---

# Spire.PDF JavaScript SVG to PDF
## Add SVG content to an existing PDF document
```javascript
// Load existing PDF document
let existingPDF = wasmModule.PdfDocument.Create();
existingPDF.LoadFromFile({fileName: inputName1});

// Create a PDF instance from SVG file
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromSvg({fileName: inputName2});

// Create template from SVG document
let template = doc.Pages.get_Item(0).CreateTemplate();

// Draw template on existing PDF at specified position and size
let point = wasmModule.PointF.Create({x: 50, y: 250});
let size = wasmModule.SizeF.Create({width: 200, height: 200});
existingPDF.Pages.get_Item(0).Canvas.DrawTemplate({template: template, location: point, size: size});
```

---

# PDF to PNG Conversion
## Convert all pages of a PDF document to PNG images
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile({fileName: inputFileName});

// Iterate through each page
for (let i = 0; i < doc.Pages.Count; i++) {
    const outputFileName = "ConvertAllPagesToPNG_" + i + ".png";
    let stream = doc.SaveAsImage({pageIndex: i});
    stream.Save(outputFileName);
    stream.Dispose();
}
```

---

# PDF Image Stream Conversion
## Convert image stream to PDF document
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();
let section = doc.Sections.Add();
let page = section.Pages.Add();

let bytes = wasmModule.FS.readFile(inputFileName);
let stream = wasmModule.Stream.CreateByBytes(bytes);
let image = wasmModule.PdfImage.FromStream(stream);

// Set image display location and size in PDF
// Calculate rate
let widthFitRate = image.PhysicalDimension.Width / page.Canvas.ClientSize.Width;
let heightFitRate = image.PhysicalDimension.Height / page.Canvas.ClientSize.Height;
let fitRate = Math.max(widthFitRate, heightFitRate);

// Calculate the size of image
let fitWidth = image.PhysicalDimension.Width / fitRate;
let fitHeight = image.PhysicalDimension.Height / fitRate;

// Draw image
page.Canvas.DrawImage({image: image, x: 0, y: 30, width: fitWidth, height: fitHeight});
```

---

# spire.pdf javascript image conversion
## convert image to pdf document
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();
let section = doc.Sections.Add();
let page = section.Pages.Add();
let image = wasmModule.PdfImage.FromFile(inputFileName);

//Set image display location and size in PDF
//Calculate rate
let widthFitRate = image.PhysicalDimension.Width / page.Canvas.ClientSize.Width;
let heightFitRate = image.PhysicalDimension.Height / page.Canvas.ClientSize.Height;
let fitRate = Math.max(widthFitRate, heightFitRate);

//Calculate the size of image
let fitWidth = image.PhysicalDimension.Width / fitRate;
let fitHeight = image.PhysicalDimension.Height / fitRate;

//Draw image
page.Canvas.DrawImage({image: image, x: 0, y: 30, width: fitWidth, height: fitHeight});

// Save the document to the specified path
doc.SaveToFile({fileName: outputFileName});
doc.Close();
```

---

# PDF Image Deletion by Bounds
## Delete images from PDF based on their bounds (position and size)
```javascript
// Create a pdf instance
let pdf = wasmModule.PdfDocument.Create();

//Load a pdf file
pdf.LoadFromFile({fileName: inputFileName});

// Get the first page
let page = pdf.Pages.get_Item(0);

//Get the information of all images in this page
let helper = wasmModule.PdfImageHelper.Create();
let images = helper.GetImagesInfo(page);

//Traverse the array
for (let i = 0; i < images.Length; i++)
{
    //Case 1: delete the image if it's bounds contains a certain point
    if (images[i].Bounds.Contains({x:49.68, y:72.75}))
    {
        page.DeleteImage({image:images[i].Image});
    }

    //Case 2: delete the image if it's bounds intersects with a certain rectangle
    let rect=wasmModule.RectangleF.Create({x:100,y:300,width:30,height:40});
    if (images[i].Bounds.IntersectsWith({rect:rect}))
    {
        page.DeleteImage({image:images[i].Image});
    }
}

// Save the document to the specified path
pdf.SaveToFile({fileName: outputFileName});
pdf.Close();
```

---

# PDF Image Deletion
## Delete an image from a PDF document using PdfImageHelper
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();

//Load the PDF document from the specified file
doc.LoadFromFile({fileName: inputFileName});

// Get the first page of the PDF document
let page = doc.Pages.get_Item(0);

// Create a PdfImageHelper instance for working with images in the PDF
let helper = wasmModule.PdfImageHelper.Create();
// Get information about the images on the page
let images = helper.GetImagesInfo(page);

// Delete the first image on the page
helper.DeleteImage({imageInfo: images[0]});

// Save the document to the specified path
doc.SaveToFile({fileName: outputFileName});
doc.Close();
```

---

# Drawing Images in PDF Documents
## Demonstrates how to draw and transform images in PDF documents using Spire.PDF for JavaScript
```javascript
// Transform text on the PDF page
function TransformText(page) {
    // Save graphics state
    let state = page.Canvas.Save();

    // Draw the text - transform
    let font = wasmModule.PdfFont.Create({fontFamily:wasmModule.PdfFontFamily.Helvetica, size:18});
    let brush1 = wasmModule.PdfSolidBrush.Create({color:wasmModule.Color.get_Blue()});
    let brush2 = wasmModule.PdfSolidBrush.Create({color:wasmModule.Color.get_CadetBlue()});
    let format = wasmModule.PdfStringFormat.Create({alignment:wasmModule.PdfTextAlignment.Center});

    // Translate the canvas to the center of the page with an offset of 20 pixels from the top
    page.Canvas.TranslateTransform(page.Canvas.ClientSize.Width / 2, 20);
    page.Canvas.DrawString({s:"Chart image",font: font,brush: brush1,x: 0,y: 0,format: format});

    // Scale the canvas horizontally by 1 and vertically by -0.8
    page.Canvas.ScaleTransform(1, -0.8);

    // Draw the transformed text with a different brush and at a specific position
    page.Canvas.DrawString({s:"Chart image",font: font,brush: brush2,x: 0,y: -2 * 18 * 1.2, format:format});

    // Restore graphics state to the previously saved state
    page.Canvas.Restore({state:state});
}

// Draw image in the PDF page
function DrawImageInPage(page,inputFileName) {
    let image = wasmModule.PdfImage.FromFile(inputFileName);

    // Calculate the scaled width and height of the image
    let width = image.Width * 0.75;
    let height = image.Height * 0.75;

    // Calculate the x-coordinate to center the image horizontally on the page
    let x = (page.Canvas.ClientSize.Width - width) / 2;

    // Draw the image on the page at the specified position and size
    page.Canvas.DrawImage({image:image, x:x,y: 60,width: width,height: height});
}

// Transform image in the PDF page
function TransformImage(page,inputFileName) {
    let image = wasmModule.PdfImage.FromFile(inputFileName);

    // Define the skew angles and scaling factors
    let skewX = 20;
    let skewY = 20;
    let scaleX = 0.2;
    let scaleY = 0.6;

    // Calculate the transformed width and height of the image
    let width = ((image.Width + image.Height * Math.tan(Math.PI * skewX / 180)) * scaleX);
    let height = ((image.Height + image.Width * Math.tan(Math.PI * skewY / 180)) * scaleY);

    // Create a template with the transformed dimensions
    let template = new wasmModule.PdfTemplate.Create({width:width, height:height});

    // Apply scale and skew transformations to the graphics of the template
    template.Graphics.ScaleTransform(scaleX, scaleY);
    template.Graphics.SkewTransform(skewX, skewY);

    // Draw the image onto the template
    template.Graphics.DrawImage(image, 0, 0);

    // Save the current graphics state
    let state = page.Canvas.Save();

    // Adjust the position and transparency for multiple repetitions of the template
    page.Canvas.TranslateTransform(page.Canvas.ClientSize.Width - 50, 260);
    let offset = (page.Canvas.ClientSize.Width - 100) / 12;

    // Repeat the template drawing with varying transparency levels
    for (let i = 0; i < 12; i++)
    {
        page.Canvas.TranslateTransform(-offset, 0);
        page.Canvas.SetTransparency({alpha:i / 12.0});
        page.Canvas.DrawTemplate({template:template,location: wasmModule.PointF.Create({x:0,y: 0})});
    }

    // Restore the graphics state to its original settings
    page.Canvas.Restore({state:state});
}
```

---

# Spire.PDF JavaScript Page to PNG Conversion
## Convert a specific PDF page to PNG image format
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();

// Load the PDF document from the specified file
doc.LoadFromFile({fileName: inputFileName});

// Convert a particular page to image
// Set page index
let pageIndex = 0;
let pdfstream = doc.SaveAsImage({pageIndex: pageIndex});

// Save the image
pdfstream.Save(outputFileName);
pdfstream.Dispose();
doc.Dispose();
```

---

# spire.pdf javascript image replacement
## replace image in pdf document
```javascript
// Create a pdf instance
let doc = wasmModule.PdfDocument.Create();

// Load a pdf file
doc.LoadFromFile({fileName: inputFileName});
// Get the first page
let page = doc.Pages.get_Item(0);

// Create PdfImageHelper object to get Image from page
let helper = wasmModule.PdfImageHelper.Create();
let images = helper.GetImagesInfo(page);

let newImage = wasmModule.PdfImage.FromFile(inputImageName);

// Replace the first image on the page. imageInfo, newImage
helper.ReplaceImage(images[0], newImage);

// Save the document to the specified path
doc.SaveToFile({fileName: outputFileName});
doc.Close();
```

---

# PDF Image Replacement with Text
## Replace an image in a PDF document with text content
```javascript
// Get the first page
let page = doc.Pages.get_Item(0);

// Create PdfImageHelper object to get Image from page
let helper = wasmModule.PdfImageHelper.Create();
let images = helper.GetImagesInfo(page);

// Get width and height of image
let width = images[0].Bounds.Width;
let height = images[0].Bounds.Height;

//Get location of image
let xPos = images[0].Bounds.X;
let yPos = images[0].Bounds.Y;

// Remove the image
helper.DeleteImage({ imageInfo: images[0] });

// Define a rectangle location,size
let point = wasmModule.PointF.Create({ x: xPos, y: yPos });
let size = wasmModule.SizeF.Create({ width: width, height: height });
let rect = wasmModule.RectangleF.Create({ location: point, size: size });

// Define string format
let format = wasmModule.PdfStringFormat.Create();
format.Alignment = wasmModule.PdfTextAlignment.Center;
format.LineAlignment = wasmModule.PdfVerticalAlignment.Middle;

let font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 18 });

// Draw the string at the location
page.Canvas.DrawString({
  s: "ReplacedText",
  font: font,
  brush: wasmModule.PdfBrushes.get_Purple(),
  layoutRectangle: rect,
  format: format
});
```

---

# spire.pdf javascript image
## set image size and insert in pdf
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Add a new page
let page = doc.Pages.Add();

// Load the image file
let image = wasmModule.PdfImage.FromFile(inputImageName);

// Set the width and height of image
let width = image.Width * 0.75;
let height = image.Height * 0.75;

// Define a position to draw image
let x = (page.Canvas.ClientSize.Width - width) / 2;
let y = 60;

// Draw image on page canvas
page.Canvas.DrawImage({ image: image, x: x, y: y, width: width, height: height });
```

---

# Spire.PDF JavaScript Barcode Generation
## Draw various types of barcodes in PDF documents
```javascript
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
barcode5.Draw({ page: page, location: wasmModule.PointF.Create({ x: 0, y: y }) });
y = barcode5.Bounds.Bottom + 5;

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
barcode6.Draw({ page: page, location: wasmModule.PointF.Create({ x: 0, y: y }) });
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
barcode7.Draw({ page: page, location: wasmModule.PointF.Create({ x: 0, y: y }) });
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
barcode8.Draw({ page: page, location: wasmModule.PointF.Create({ x: 0, y: y }) });
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
barcode9.Draw({ page: page, location: wasmModule.PointF.Create({ x: 0, y: y }) });
y = barcode9.Bounds.Bottom + 5;
```

---

# Spire.PDF JavaScript Spot Color
## Draw content with spot color in PDF document
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Add a new page
let page = doc.Pages.Add();

// Initialize an instance of PdfSeparationColorSpace
let pdfRgbColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_DarkViolet() });
let cs = wasmModule.PdfSeparationColorSpace.Create("MySpotColor", pdfRgbColor);

// Set tini = 1 for the cs
let color = wasmModule.PdfSeparationColor.Create(cs, 1);

// Create a brush with spot color
let brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });
let font1 = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 10 });

// Draw a string
page.Canvas.DrawString({ s: "Tint=1.0", font: font1, brush: brush, point: wasmModule.PointF.Create({ x: 160, y: 160 }) });

// Draw pie with spot color(DarkViolet)
page.Canvas.DrawPie({ brush: brush, x: 148, y: 200, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });

// Create a font
let font2 = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 10 });
page.Canvas.DrawString({ s: "Tint=0.7", font: font2, brush: brush, point: wasmModule.PointF.Create({ x: 230, y: 160 }) });

// Create a separation color with tint 0.7 and draw a pie
color = wasmModule.PdfSeparationColor.Create(cs, 0.7);
brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });
page.Canvas.DrawPie({ brush: brush, x: 218, y: 200, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });

// Create a font
let font3 = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 10 });
page.Canvas.DrawString({ s: "Tint=0.4", font: font3, brush: brush, point: wasmModule.PointF.Create({ x: 300, y: 160 }) });

// Create a separation color with tint 0.4 and draw a pie
color = wasmModule.PdfSeparationColor.Create(cs, 0.4);
brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });
page.Canvas.DrawPie({ brush: brush, x: 288, y: 200, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });

// Create a font
let font4 = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 10 });
page.Canvas.DrawString({ s: "Tint=0.1", font: font4, brush: brush, point: wasmModule.PointF.Create({ x: 370, y: 160 }) });

// Create a separation color with tint 0.1 and draw a pie
color = wasmModule.PdfSeparationColor.Create(cs, 0.1);
brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });
page.Canvas.DrawPie({ brush: brush, x: 358, y: 200, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });

// Draw pie with spot color(Purple)  colorant, baseColor
pdfRgbColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Purple() });
cs = wasmModule.PdfSeparationColorSpace.Create("MySpotColor", pdfRgbColor);
color = wasmModule.PdfSeparationColor.Create(cs, 1);
brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });
page.Canvas.DrawPie({ brush: brush, x: 148, y: 280, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });

// Draw pies with different tints of the purple spot color
color = wasmModule.PdfSeparationColor.Create(cs, 0.7);
brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });
page.Canvas.DrawPie({ brush: brush, x: 218, y: 280, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });

color = wasmModule.PdfSeparationColor.Create(cs, 0.4);
brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });
page.Canvas.DrawPie({ brush: brush, x: 288, y: 280, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });

color = wasmModule.PdfSeparationColor.Create(cs, 0.1);
brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });
page.Canvas.DrawPie({ brush: brush, x: 358, y: 280, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });

// Draw pie with spot color (DarkSlateBlue) and base color
pdfRgbColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_DarkSlateBlue() });
cs = wasmModule.PdfSeparationColorSpace.Create("MySpotColor", pdfRgbColor);
color = wasmModule.PdfSeparationColor.Create(cs, 1);
brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });
page.Canvas.DrawPie({ brush: brush, x: 148, y: 360, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });

// Draw pies with different tints of the dark slate blue spot color
color = wasmModule.PdfSeparationColor.Create(cs, 0.7);
brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });
page.Canvas.DrawPie({ brush: brush, x: 218, y: 360, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });

color = wasmModule.PdfSeparationColor.Create(cs, 0.4);
brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });
page.Canvas.DrawPie({ brush: brush, x: 288, y: 360, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });

color = wasmModule.PdfSeparationColor.Create(cs, 0.1);
brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });
page.Canvas.DrawPie({ brush: brush, x: 358, y: 360, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });
```

---

# Spire.PDF JavaScript Dashed Line
## Draw dashed line in PDF document
```javascript
// Save graphics state
let state = page.Canvas.Save();

// Set location and size for line
let x = 150;
let y = 200;
let width = 300;

// Create pens and set style for it
let pdfRgbColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Red() });
let pen = wasmModule.PdfPen.Create({ pdfRgbColor: pdfRgbColor, width: 3 });

// Set dash style and pattern
pen.DashStyle = wasmModule.PdfDashStyle.Dash;
pen.DashPattern = [1.0, 4.0, 1.0];

// Draw dashed lines
page.Canvas.DrawLine({ pen: pen, x1: x, y1: y, x2: x + width, y2: y });

// Restore graphics
page.Canvas.Restore({ state: state });
```

---

# Spire.PDF JavaScript Drawing
## Draw Filled Rectangles in PDF
```javascript
// Set rectangle display location and size
let x = 200;
let y = 300;
let width = 200;
let height = 120;

// Create a pen and a brush
let pdfRgbColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Black() });
let pen = wasmModule.PdfPen.Create({ pdfRgbColor: pdfRgbColor, width: 1 });
let brush = wasmModule.PdfSolidBrush.Create({ color: wasmModule.Color.get_OrangeRed() });

// Draw a filled rectangle
let point = wasmModule.PointF.Create({ x: x, y: y });
let size = wasmModule.SizeF.Create({ width: width, height: height });
let rect = wasmModule.RectangleF.Create({ location: point, size: size });
page.Canvas.DrawRectangle({ pen: pen, brush: brush, rectangle: rect });
```

---

# spire.pdf javascript drawing
## draw lines and shapes in pdf document
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Get the first page
let page = doc.Pages.get_Item(0);

// Save graphics state
let state = page.Canvas.Save();

// Set location and size
let x = 95;
let y = 95;
let width = 400;
let height = 500;

// Create pens
let pdfRgbColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Black() });
let pen = wasmModule.PdfPen.Create({ pdfRgbColor: pdfRgbColor, width: 0.1 });

let pdfRgbColor1 = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Red() });
let pen1 = wasmModule.PdfPen.Create({ pdfRgbColor: pdfRgbColor1, width: 0.1 });

// Draw a rectangle
page.Canvas.DrawRectangle({ pen: pen, x: x, y: y, width: width, height: height });

// Draw two crossed lines
page.Canvas.DrawLine({ pen: pen1, x1: x, y1: y, x2: x + width, y2: y + height });
page.Canvas.DrawLine({ pen: pen1, x1: x + width, y1: y, x2: x, y2: y + height });

// Restore graphics
page.Canvas.Restore({ state });
```

---

# spire.pdf javascript rectangles
## draw rectangles in pdf document
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Get the first page
let page = doc.Pages.get_Item(0);

// Save graphics state
let state = page.Canvas.Save();

// Set rectangle display location and size
let x = 130;
let y = 100;
let width = 300;
let height = 400;

// Draw rectangles
let pdfRgbColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Black() });
let pen = wasmModule.PdfPen.Create({ pdfRgbColor: pdfRgbColor, width: 0.1 });

let point = wasmModule.PointF.Create({ x: x, y: y });
let size = wasmModule.SizeF.Create({ width: width, height: height });
let rect = wasmModule.RectangleF.Create({ location: point, size: size });
page.Canvas.DrawRectangle({ pen: pen, rectangle: rect });

y = y + height - 50;
width = 100;
height = 50;

// Initialize an instance of PdfSeparationColorSpace
let pdfRgbColor1 = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.FromArgb({ alpha: 0, red: 100, green: 0, blue: 0 }) });
let cs = wasmModule.PdfSeparationColorSpace.Create("MySpotColor", pdfRgbColor1);

let pdfRgbColor2 = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Red() });
let pen2 = wasmModule.PdfPen.Create({ pdfRgbColor: pdfRgbColor2, width: 1 });

// Create a brush with spot color
let separationColor = wasmModule.PdfSeparationColor.Create(cs, 0.1);
let brush = wasmModule.PdfSolidBrush.Create({ complexColor: separationColor });

// Draw rectangles
let point1 = wasmModule.PointF.Create({ x: x, y: y });
let size1 = wasmModule.SizeF.Create({ width: width, height: height });
let rect1 = wasmModule.RectangleF.Create({ location: point1, size: size1 });
page.Canvas.DrawRectangle({ pen: pen2, brush: brush, rectangle: rect1 });

// Restore graphics
page.Canvas.Restore({ state: state });
```

---

# spire.pdf javascript shapes
## drawing various shapes in a pdf document
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Create a new page
let page = doc.Pages.Add();

// Draw a path
DrawPath(page);

// Draw a spirograph shape
DrawSpiro(page);

// Draw a rectangle 
DrawRectangle(page);

// Draw a pie
DrawPie(page);

// Draw an ellipse
DrawEllipse(page);

function DrawPath(page) {
  // Define array to store points of line
  let points = new Array(5);

  // Calculate the points for a pentagon shape using trigonometry
  for (let i = 0; i < points.length; i++) {
    let x = Math.cos(i * 2 * Math.PI / 5);
    let y = Math.sin(i * 2 * Math.PI / 5);
    let pointf = wasmModule.PointF.Create({ x: x, y: y });
    points[i] = pointf;
  }

  // Create a path and define the lines connecting the points
  let path = wasmModule.PdfPath.Create();
  path.AddLine({ point1: points[2], point2: points[0] });
  path.AddLine({ point1: points[0], point2: points[3] });
  path.AddLine({ point1: points[3], point2: points[1] });
  path.AddLine({ point1: points[1], point2: points[4] });
  path.AddLine({ point1: points[4], point2: points[2] });

  // Save the current graphics state
  let state = page.Canvas.Save();

  // Define a pen and brush with specific colors and styles
  let pdfrgbColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_DeepSkyBlue() });
  let pen = wasmModule.PdfPen.Create({ pdfRgbColor: pdfrgbColor, width: 0.02 });
  let brush1 = wasmModule.PdfSolidBrush.Create({ color: wasmModule.Color.get_CadetBlue() });

  // Apply scaling and translation to the graphics context
  page.Canvas.ScaleTransform(50, 50);
  page.Canvas.TranslateTransform(5, 1.2);

  // Draw the path with the defined pen
  page.Canvas.DrawPath({ pen: pen, path: path });

  // Translate and draw with different fill modes
  page.Canvas.TranslateTransform(2, 0);
  path.FillMode = wasmModule.PdfFillMode.Alternate;
  page.Canvas.DrawPath({ pen: pen, brush: brush1, path: path });

  page.Canvas.TranslateTransform(2, 0);
  path.FillMode = wasmModule.PdfFillMode.Winding;
  page.Canvas.DrawPath({ pen: pen, brush: brush1, path: path });

  // Create a linear gradient brush for another path
  let point1 = wasmModule.PointF.Create({ x: -2, y: 0 });
  let point2 = wasmModule.PointF.Create({ x: 2, y: 0 });
  let regColor1 = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Red() });
  let regColor2 = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Blue() });
  let brush2 = wasmModule.PdfLinearGradientBrush.Create({
    point1: point1,
    point2: point2,
    color1: regColor1,
    color2: regColor2
  });

  // Translate and draw the path with the gradient brush
  page.Canvas.TranslateTransform(-4, 2);
  path.FillMode = wasmModule.PdfFillMode.Alternate;
  page.Canvas.DrawPath({ pen: pen, brush: brush2, path: path });

  // Create a radial gradient brush for another effect
  let point3 = wasmModule.PointF.Create({ x: 0, y: 0 });
  let point4 = wasmModule.PointF.Create({ x: 0, y: 0 });
  let brush3 = wasmModule.PdfRadialGradientBrush.Create({
    centreStart: point3,
    radiusStart: 0,
    centreEnd: point4,
    radiusEnd: 1,
    colorStart: regColor1,
    colorEnd: regColor2
  });

  // Translate and draw the path with the radial gradient brush
  page.Canvas.TranslateTransform(2, 0);
  path.FillMode = wasmModule.PdfFillMode.Winding;
  page.Canvas.DrawPath({ pen: pen, brush: brush3, path: path });

  // Create a tiling brush for a pattern
  let rect = wasmModule.RectangleF.Create({ x: 0, y: 0, width: 4, height: 4 });
  let brush4 = wasmModule.PdfTilingBrush.Create({ rectangle: rect });
  brush4.Graphics.DrawRectangle({ brush: brush2, x: 0, y: 0, width: 4, height: 4 });

  // Translate and draw the path with the tiling brush
  page.Canvas.TranslateTransform(2, 0);
  path.FillMode = wasmModule.PdfFillMode.Winding;
  page.Canvas.DrawPath({ pen: pen, brush: brush4, path: path });

  // Restore the previous graphics state
  page.Canvas.Restore({ state: state });
};

function DrawSpiro(page) {
  // Save the current graphics state
  let state = page.Canvas.Save();

  // Draw a spirograph shape
  let pen = wasmModule.PdfPens.get_DeepSkyBlue();

  // Define parameters for the spirograph
  let nPoints = 1000;
  let r1 = 30;
  let r2 = 25;
  let p = 35;
  let x1 = r1 + r2 - p;
  let y1 = 0;
  let x2 = 0;
  let y2 = 0;

  // Translate to the desired position
  page.Canvas.TranslateTransform(100, 100);

  // Calculate and draw the spirograph points
  for (let i = 0; i < nPoints; i++) {
    let t = i * Math.PI / 90;
    x2 = (r1 + r2) * Math.cos(t) - p * Math.cos((r1 + r2) * t / r2);
    y2 = (r1 + r2) * Math.sin(t) - p * Math.sin((r1 + r2) * t / r2);
    page.Canvas.DrawLine({ pen: pen, x1: x1, y1: y1, x2: x2, y2: y2 });
    x1 = x2;
    y1 = y2;
  }

  // Restore the previous graphics state
  page.Canvas.Restore({ state: state });
};

function DrawRectangle(page) {
  // Save the current graphics state
  let state = page.Canvas.Save();

  // Create a pen with a chocolate color and set its width
  let pdfrgbColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Chocolate() });
  let pen = wasmModule.PdfPen.Create({ pdfRgbColor: pdfrgbColor, width: 1 });

  // Define the position and size of the rectangle
  let point = wasmModule.PointF.Create({ x: 20, y: 310 });
  let size = wasmModule.SizeF.Create({ width: 150, height: 120 });
  let rect = wasmModule.RectangleF.Create({ location: point, size: size });

  // Draw the rectangle on the canvas
  page.Canvas.DrawRectangle({ pen: pen, rectangle: rect });

  // Restore the previous graphics state
  page.Canvas.Restore({ state: state });
};

function DrawPie(page) {
  // Save the current graphics state
  let state = page.Canvas.Save();

  // Create a pen with a dark red color and set its width
  let pdfrgbColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_DarkRed() });
  let pen = wasmModule.PdfPen.Create({ pdfRgbColor: pdfrgbColor, width: 2 });

  // Draw a pie shape with specified parameters
  // Parameters: pen, x position, y position, width, height, start angle, sweep angle
  page.Canvas.DrawPie({ pen: pen, x: 220, y: 320, width: 100, height: 90, startAngle: 360, sweepAngle: 360 });

  // Restore the previous graphics state
  page.Canvas.Restore({ state: state });
};

function DrawEllipse(page) {
  // Save the current graphics state
  let state = page.Canvas.Save();

  // Create a brush with a cadet blue color
  let brush = wasmModule.PdfSolidBrush.Create({ color: wasmModule.Color.get_CadetBlue() });

  // Draw an ellipse with specified parameters
  page.Canvas.DrawEllipse({ brush: brush, x: 380, y: 325, width: 80, height: 80 });

  // Restore the previous graphics state
  page.Canvas.Restore({ state: state });
};
```

---

# spire.pdf javascript gradient fill
## draw shapes with gradient fill in PDF
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Create a new page
let page = doc.Pages.Add();

// Create a PdfLinearGradientBrush and set its style
let point1 = wasmModule.PointF.Create({ x: 100, y: 100 });
let size1 = wasmModule.SizeF.Create({ width: 200, height: 120 });
let rect1 = wasmModule.RectangleF.Create({ location: point1, size: size1 });
let pdfRgbColor1 = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_BlueViolet() });
let pdfRgbColor2 = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_DarkBlue() });

// Create the linear gradient brush with horizontal mode
let brush1 = wasmModule.PdfLinearGradientBrush.Create({
  rect: rect1,
  color1: pdfRgbColor1,
  color2: pdfRgbColor2,
  mode: wasmModule.PdfLinearGradientMode.Horizontal
});

// Draw a rectangle using the linear gradient brush
page.Canvas.DrawRectangle({ brush: brush1, rectangle: rect1 });

// Create a PdfRadialGradientBrush and set its style
let point2 = wasmModule.PointF.Create({ x: 200, y: 400 });
let point3 = wasmModule.PointF.Create({ x: 300, y: 500 });
let pdfRgbColor3 = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_SkyBlue() });
let pdfRgbColor4 = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_DarkBlue() });

// Create the radial gradient brush
let brush2 = wasmModule.PdfRadialGradientBrush.Create({ centreStart: point2, radiusStart: 100, centreEnd: point3, radiusEnd: 100, colorStart: pdfRgbColor3, colorEnd: pdfRgbColor4 });

// Draw an ellipse using the radial gradient brush
let point5 = wasmModule.PointF.Create({ x: 100, y: 300 });
let size5 = wasmModule.SizeF.Create({ width: 200, height: 200 });
let rect5 = wasmModule.RectangleF.Create({ location: point5, size: size5 });
page.Canvas.DrawEllipse({ brush: brush2, rectangle: rect5 });
```

---

# spire.pdf javascript list
## create different types of lists in PDF document
```javascript
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
```

---

# PDF Page Overlay
## Demonstrates how to overlay a page from one PDF document onto pages of another PDF document with transparency
```javascript
// Create page template form the first page of doc1
let template = doc1.Pages.get_Item(0).CreateTemplate();

// Iterate each page in doc2
for (let i = 0; i < doc2.Pages.Count; i++) {
  let page = doc2.Pages.get_Item(i);
  // Set transparency for page
  page.Canvas.SetTransparency({ alphaPen: 0.25, alphaBrush: 0.25, blendMode: wasmModule.PdfBlendMode.Overlay });

  // Draw template
  let point = wasmModule.PointF.Create({ x: 0, y: 0 });
  page.Canvas.DrawTemplate({ template: template, location: point });
}
```

---

# PDF Rectangle Transparency
## Drawing rectangles with transparency effects in PDF documents
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Get the first page of the document
let page = doc.Pages.get_Item(0);

// Save the current graphics state
let state = page.Canvas.Save();

// Create a black pen for outlining the rectangles
let pdfrgbColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Black() });
let pen = wasmModule.PdfPen.Create({ pdfRgbColor: pdfrgbColor, width: 1 });

// Create a red brush for filling the rectangles
let brush = wasmModule.PdfSolidBrush.Create({ color: wasmModule.Color.get_Red() });

// Set the blend mode for transparency effects
let mode = wasmModule.PdfBlendMode.Normal;

// Set transparency for the first rectangle (50% transparency)
page.Canvas.SetTransparency({ alphaPen: 0.5, alphaBrush: 0.5, blendMode: mode });

// Define and draw the first rectangle
let point = wasmModule.PointF.Create({ x: 200, y: 300 });
let size = wasmModule.SizeF.Create({ width: 200, height: 100 });
let rect = wasmModule.RectangleF.Create({ location: point, size: size });
page.Canvas.DrawRectangle({ pen: pen, brush: brush, rectangle: rect });

// Set transparency for the second rectangle (20% transparency)
page.Canvas.SetTransparency({ alphaPen: 0.2, alphaBrush: 0.2, blendMode: mode });

// Define and draw the second rectangle
let point1 = wasmModule.PointF.Create({ x: 300, y: 250 });
let size1 = wasmModule.SizeF.Create({ width: 200, height: 100 });
let rect1 = wasmModule.RectangleF.Create({ location: point1, size: size1 });
page.Canvas.DrawRectangle({ pen: pen, brush: brush, rectangle: rect1 });

// Restore the graphics state to the saved state
page.Canvas.Restore({ state: state });

// Clean up resources
doc.Close();
```

---

# spire.pdf javascript separation color space
## set separation color space with different tint values
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Create a new page
let page = doc.Pages.Add();

// Initialize an instance of PdfSeparationColorSpace with RGB color
let pdfRGBColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Purple() });
let rgb = wasmModule.PdfSeparationColorSpace.Create("MySpotColor", pdfRGBColor);

// Set tint = 1 for the color space
let color = wasmModule.PdfSeparationColor.Create(rgb, 1);

// Create a brush with spot color
let brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });

let font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 10 });

// Draw pie with tint=1.0  brush, x, y, width, height, startAngle, sweepAngle
page.Canvas.DrawPie({ brush: brush, x: 10, y: 30, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });
page.Canvas.DrawString({ s: "Tint=1.0", font: font, brush: brush, point: wasmModule.PointF.Create({ x: 22, y: 100 }) });

// Change tint to 0.5
color = wasmModule.PdfSeparationColor.Create(rgb, 0.5);
brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });

// Draw pie with tint=0.5
page.Canvas.DrawPie({ brush: brush, x: 80, y: 30, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });
page.Canvas.DrawString({ s: "Tint=0.5", font: font, brush: brush, point: wasmModule.PointF.Create({ x: 92, y: 100 }) });

// Change tint to 0.25
color = wasmModule.PdfSeparationColor.Create(rgb, 0.25);
brush = wasmModule.PdfSolidBrush.Create({ complexColor: color });

// Draw pie with tint=0.25
page.Canvas.DrawPie({ brush: brush, x: 150, y: 30, width: 60, height: 60, startAngle: 360, sweepAngle: 360 });
page.Canvas.DrawString({ s: "Tint=0.25", font: font, brush: brush, point: wasmModule.PointF.Create({ x: 162, y: 100 }) });
```

---

# PDF Transparency Effects
## Demonstrates how to set transparency with different blend modes in PDF documents
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Add a new section to the document
let section = doc.Sections.Add();

// Load an image from the specified file
let image = wasmModule.PdfImage.FromFile(inputFileName);
let imageWidth = image.PhysicalDimension.Width / 2;
let imageHeight = image.PhysicalDimension.Height / 2;

// Define an array of blend modes for transparency effects
let models = ["Normal", "Multiply", "Screen", "Overlay", "Darken", "Lighten", "ColorDodge", "ColorBurn", "HardLight"
  , "SoftLight", "Difference", "Exclusion", "Hue", "Saturation", "Color", "Luminosity"];

// Loop through each blend mode to create a new page for each one
for (let i = 0; i < models.length; i++) {
  let mode = models[i];
  // Add a new page to the section
  let page = section.Pages.Add();
  let pageWidth = page.Canvas.ClientSize.Width;
  let y = 0;

  // Title section
  y = y + 15;
  let brush = wasmModule.PdfSolidBrush.Create({ color: wasmModule.Color.get_OrangeRed() });
  let font = wasmModule.PdfTrueTypeFont.Create({
    fontFile: "/Library/Fonts/ARIALUNI.TTF",
    size: 16,
    style: wasmModule.PdfFontStyle.Bold
  });

  // Create a PdfStringFormat object to center-align the title
  let format = wasmModule.PdfStringFormat.Create({ alignment: wasmModule.PdfTextAlignment.Center });
  let text = "Transparency Blend Mode:" + mode;

  // Draw the title text on the page
  page.Canvas.DrawString({ s: text, font: font, brush: brush, x: pageWidth / 2, y: y, format: format });

  // Measure the size of the title text for positioning
  let size = font.MeasureString({ text: text, format: format });
  y = y + size.Height + 25;

  // Draw the image on the page
  page.Canvas.DrawImage({ image: image, x: 0, y: y, width: imageWidth, height: imageHeight });
  page.Canvas.Save();
  let d = (page.Canvas.ClientSize.Width - imageWidth) / 5;
  let x = d;
  y = y + d / 2 + 40;

  // Draw the image multiple times with varying transparency
  for (let j = 0; j < 5; j++) {
    let alpha = 1.0 / 6 * (5 - j);

    // Set transparency for the canvas
    page.Canvas.SetTransparency({alphaPen: alpha, alphaBrush: alpha, blendMode: wasmModule.PdfBlendMode.fromValue(i)});

    // Draw the image with the current transparency settings
    page.Canvas.DrawImage({ image: image, x: x, y: y, width: imageWidth, height: imageHeight });
    x = x + d;
    y = y + d / 2 + 40;
  }
  // Restore the canvas to the previous state
  page.Canvas.Restore();
}
```

---

# spire.pdf javascript annotation
## add free text annotations to pdf document
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Load the PDF fle
doc.LoadFromFile({ fileName: inputFileName });

// Get the first page
let page = doc.Pages.get_Item(0);

// Define a rectangle for the free text annotation
let rect = wasmModule.RectangleF.Create({ x: 0, y: 300, width: 100, height: 80 });

// Create a free text annotation and set its properties
let textAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);
textAnnotation.Text = "\n  Spire.PDF";

// Set border style for annotation
let border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 1 });
textAnnotation.Border = border;
textAnnotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Gray() });
textAnnotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.Slash;

// Set font properties for the annotation
let font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.TimesRoman, size: 20 });
textAnnotation.Font = font;

// Set color and opacity for the text annotation
textAnnotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightBlue() });
textAnnotation.Opacity = 0.8;

// Add the free text annotation to the page
page.Annotations.Add(textAnnotation);

// Create additional annotations with different properties
rect = wasmModule.RectangleF.Create({ x: 150, y: 200, width: 150, height: 40 });
textAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);
textAnnotation.Text = "\nHigh Fidelity Pdf file Conversion";
border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 1 });
font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 10 });
textAnnotation.Font = font;
textAnnotation.Border = border;
textAnnotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightGoldenrodYellow() });
textAnnotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.RClosedArrow;
textAnnotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightPink() });
textAnnotation.Opacity = 0.8;
page.Annotations.Add(textAnnotation);

rect = wasmModule.RectangleF.Create({ x: 150, y: 280, width: 280, height: 40 });
textAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);
textAnnotation.Text = "\nEasily Manipulate document and Form fields";
border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 1 });
font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 10 });
textAnnotation.Font = font;
textAnnotation.Border = border;
textAnnotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Gray() });
textAnnotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.Circle;
textAnnotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightSkyBlue() });
textAnnotation.Opacity = 0.8;
page.Annotations.Add(textAnnotation);

rect = wasmModule.RectangleF.Create({ x: 150, y: 360, width: 200, height: 40 });
textAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);
textAnnotation.Text = "\nSecurity features";
border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 1 });
font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 10 });
textAnnotation.Font = font;
textAnnotation.Border = border;
textAnnotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Pink() });
textAnnotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.RClosedArrow;
textAnnotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightGreen() });
textAnnotation.Opacity = 0.8;
page.Annotations.Add(textAnnotation);

rect = wasmModule.RectangleF.Create({ x: 150, y: 440, width: 200, height: 40 });
textAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);
textAnnotation.Text = "\nExtract data from Pdf documents";
border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 1 });
font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 10 });
textAnnotation.Font = font;
textAnnotation.Border = border;
textAnnotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_OrangeRed() });
textAnnotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.RClosedArrow;
textAnnotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightGoldenrodYellow() });
textAnnotation.Opacity = 0.8;
page.Annotations.Add(textAnnotation);
```

---

# Spire.PDF JavaScript Annotations
## Demonstrates how to add various types of annotations to a PDF document
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Set margins for the page
let unitCvtr = wasmModule.PdfUnitConvertor.Create();
let margin = wasmModule.PdfMargins.Create();
margin.Top = unitCvtr.ConvertUnits(2.54, wasmModule.PdfGraphicsUnit.Centimeter, wasmModule.PdfGraphicsUnit.Point);
margin.Bottom = margin.Top;
margin.Left = unitCvtr.ConvertUnits(3, wasmModule.PdfGraphicsUnit.Centimeter, wasmModule.PdfGraphicsUnit.Point);
margin.Right = margin.Left;

// Create a page with specified size and margins
let page = doc.Pages.Add({ size: wasmModule.PdfPageSize.A4(), margins: margin });

// Add a title to the page
let brush1 = wasmModule.PdfBrushes.get_Black();
let fontstyle = wasmModule.PdfFontStyle.Bold.value + spirepdf.PdfFontStyle.Italic.value;
let font1 = wasmModule.PdfTrueTypeFont.Create({
  fontName: "Arial",
  emSize: 13,
  styleNumber: fontstyle,
  unicode: true
});
let format1 = wasmModule.PdfStringFormat.Create({ alignment: wasmModule.PdfTextAlignment.Left });
let y = 50;
let s = "The sample demonstrates how to add annotations in PDF document.";

page.Canvas.DrawString({ s: s, font: font1, brush: brush1, x: 0, y: y - 5, format: format1 });
y = y + font1.MeasureString({ text: s, format: format1 }).Height;
y = y + 15;

// Add various types of annotations to the page
y = AddDocumentLinkAnnotation(page, y);
y = y + 6;
y = AddFileLinkAnnotation(page, y);
y = y + 6;
y = AddFreeTextAnnotation(page, y);
y = y + 6;
y = AddLineAnnotation(page, y);
y = y + 6;
y = AddTextMarkupAnnotation(page, y);
y = y + 6;
y = AddPopupAnnotation(page, y);
y = y + 6;
y = AddRubberStampAnnotation(page, y);

function AddDocumentLinkAnnotation(page, y) {
  // Set up font and formatting
  let font = wasmModule.PdfTrueTypeFont.Create({
    fontFile: "/Library/Fonts/ARIALUNI.TTF",
    size: 12,
    style: wasmModule.PdfFontStyle.Regular
  });
  let format = wasmModule.PdfStringFormat.Create();
  format.MeasureTrailingSpaces = true;

  let prompt = "Document Link: ";
  let size = font.MeasureString({ text: prompt });

  // Draw the prompt text
  page.Canvas.DrawString({ s: prompt, font: font, brush: wasmModule.PdfBrushes.get_DodgerBlue(), x: 0, y: y });
  let x = font.MeasureString(prompt, format).Width;

  // Set up the destination for the link annotation
  let dest = wasmModule.PdfDestination.Create({ page: page });
  dest.Mode = wasmModule.PdfDestinationMode.Location;
  dest.Location = wasmModule.PointF.Create({ x: 0, y: y });
  dest.Zoom = 2;

  let label = "Click me, Zoom 200%";
  size = font.MeasureString({ text: label });

  // Draw the label text
  let bounds = wasmModule.RectangleF.Create({ x: x, y: y, width: size.Width, height: size.Height });
  page.Canvas.DrawString({ s: label, font: font, brush: wasmModule.PdfBrushes.get_OrangeRed(), x: x, y: y });

  // Create a document link annotation and set its properties
  let annotation = wasmModule.PdfDocumentLinkAnnotation.Create({ rectangle: bounds, destination: dest });
  annotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Blue() });

  // Add the annotation to the page's annotation collection
  page.Annotations.Add(annotation);
  y = bounds.Bottom;

  return y;
};

function AddFileLinkAnnotation(page, y) {
  // Set up font and formatting
  let font = wasmModule.PdfTrueTypeFont.Create({
    fontFile: "/Library/Fonts/ARIALUNI.TTF",
    size: 12,
    style: wasmModule.PdfFontStyle.Regular
  });
  let format = wasmModule.PdfStringFormat.Create();
  format.MeasureTrailingSpaces = true;

  // Draw the prompt text
  let prompt = "Launch File:  ";
  let size = font.MeasureString({ text: prompt });

  // Draw the prompt text
  page.Canvas.DrawString({ s: prompt, font: font, brush: wasmModule.PdfBrushes.get_DodgerBlue(), x: 0, y: y });
  let x = font.MeasureString(prompt, format).Width;

  let label = "Launch Notepad.exe";
  size = font.MeasureString({ text: label });

  // Draw the label text
  let bounds = wasmModule.RectangleF.Create({ x: x, y: y, width: size.Width, height: size.Height });
  page.Canvas.DrawString({ s: label, font: font, brush: wasmModule.PdfBrushes.get_OrangeRed(), x: x, y: y });

  // Create a file link annotation and set its properties
  let annotation = wasmModule.PdfFileLinkAnnotation.Create(bounds, "D:\\Notepad.exe");
  annotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Blue() });

  // Add the annotation to the page's annotation collection
  page.Annotations.Add(annotation);
  y = bounds.Bottom;

  return y;
};

function AddFreeTextAnnotation(page, y) {
  // Set up font and formatting
  let font = wasmModule.PdfTrueTypeFont.Create({
    fontFile: "/Library/Fonts/ARIALUNI.TTF",
    size: 12,
    style: wasmModule.PdfFontStyle.Regular
  });
  let format = wasmModule.PdfStringFormat.Create();
  format.MeasureTrailingSpaces = true;

  // Draw the prompt text
  let prompt = "Text Markup: ";
  let size = font.MeasureString({ text: prompt, format: format });
  page.Canvas.DrawString({ s: prompt, font: font, brush: wasmModule.PdfBrushes.get_DodgerBlue(), x: 0, y: y });
  let x = size.Width;

  // Draw the label text and bounding rectangle
  let label = "I'm a text box, not a TV";
  size = font.MeasureString({ text: label });
  let bounds = wasmModule.RectangleF.Create({ x: x, y: y, width: size.Width, height: size.Height });
  let pdfRgbColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Blue() });
  let pen = wasmModule.PdfPen.Create({ pdfRgbColor: pdfRgbColor, width: 0.1 });
  page.Canvas.DrawRectangle({ pen: pen, rectangle: bounds });
  page.Canvas.DrawString({ s: label, font: font, brush: wasmModule.PdfBrushes.get_OrangeRed(), x: x, y: y });

  // Set up the free text annotation with its properties
  let location = wasmModule.PointF.Create({ x: bounds.Right + 16, y: bounds.Top - 16 });
  let annotaionBounds = wasmModule.RectangleF.Create({
    location: location,
    size: wasmModule.SizeF.Create({ width: 80, height: 32 })
  });

  let annotation = wasmModule.PdfFreeTextAnnotation.Create({ rectangle: annotaionBounds });
  annotation.AnnotationIntent = wasmModule.PdfAnnotationIntent.FreeTextCallout;
  annotation.Border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 0.5 });
  annotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Red() });
  location = wasmModule.PointF.Create({ x: bounds.Right + 16, y: bounds.Top - 16 });

  let point1 = wasmModule.PointF.Create({ x: bounds.Right, y: bounds.Top });
  let point2 = wasmModule.PointF.Create({ x: bounds.Right + 8, y: bounds.Top - 8 });
  annotation.CalloutLines = [point1, point2, location];
  annotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Yellow() });
  annotation.Flags = wasmModule.PdfAnnotationFlags.Locked;
  annotation.Font = font;
  annotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.OpenArrow;
  annotation.MarkupText = "Just a joke.";
  annotation.Opacity = 0.75;
  annotation.TextMarkupColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Green() });

  // Add the free text annotation to the page's annotation collection
  page.Annotations.Add(annotation);
  y = bounds.Bottom;

  return y;
};

function AddLineAnnotation(page, y) {
  // Set up font and formatting
  let font = wasmModule.PdfTrueTypeFont.Create({
    fontFile: "/Library/Fonts/ARIALUNI.TTF",
    size: 12,
    style: wasmModule.PdfFontStyle.Regular
  });
  let format = wasmModule.PdfStringFormat.Create();
  format.MeasureTrailingSpaces = true;

  // Draw the prompt text
  let prompt = "Line Annotation: ";
  let size = font.MeasureString({ text: prompt, format: format });
  page.Canvas.DrawString({ s: prompt, font: font, brush: wasmModule.PdfBrushes.get_DodgerBlue(), x: 0, y: y });
  let x = size.Width;

  // Draw the label text and bounding rectangle
  let label = "Line Annotation";
  size = font.MeasureString({ text: label });
  page.Canvas.DrawString({ s: label, font: font, brush: wasmModule.PdfBrushes.get_OrangeRed(), x: x, y: y });
  let bounds = wasmModule.RectangleF.Create({ x: x, y: y, width: size.Width, height: size.Height });
  let linePoints = [bounds.Left, bounds.Top, bounds.Right, bounds.Bottom];

  // Create a line annotation and set its properties
  let annotation = wasmModule.PdfLineAnnotation.Create({ linePoints: linePoints, text: "Annotation" });
  annotation.BeginLineStyle = wasmModule.PdfLineEndingStyle.ClosedArrow;
  annotation.EndLineStyle = wasmModule.PdfLineEndingStyle.ClosedArrow;
  annotation.LineCaption = true;
  annotation.BackColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Black() });
  annotation.CaptionType = wasmModule.PdfLineCaptionType.Inline;

  // Add the line annotation to the page's annotation collection
  page.Annotations.Add(annotation);
  y = bounds.Bottom;

  return y;
};

function AddTextMarkupAnnotation(page, y) {
  // Set up font and formatting
  let font = wasmModule.PdfTrueTypeFont.Create({
    fontFile: "/Library/Fonts/ARIALUNI.TTF",
    size: 12,
    style: wasmModule.PdfFontStyle.Regular
  });
  let format = wasmModule.PdfStringFormat.Create();
  format.MeasureTrailingSpaces = true;

  // Draw the prompt text
  let prompt = "Highlight incorrect spelling: ";
  let size = font.MeasureString({ text: prompt, format: format });
  page.Canvas.DrawString({ s: prompt, font: font, brush: wasmModule.PdfBrushes.get_DodgerBlue(), x: 0, y: y });
  let x = size.Width;

  // Draw the label text and bounding rectangle
  let label = "demo of anotation";
  page.Canvas.DrawString({ s: label, font: font, brush: wasmModule.PdfBrushes.get_OrangeRed(), x: x, y: y });
  size = font.MeasureString({ text: "demo of ", format: format });
  x = x + size.Width;
  let incorrectWordLocation = wasmModule.PointF.Create({ x: x, y: y });
  let markupText = "Should be 'annotation'";

  // Create a text markup annotation and set its properties
  let rect = wasmModule.RectangleF.Create({ x: x, y: y, width: 100, height: 100 });
  let annotation = wasmModule.PdfTextMarkupAnnotation.Create({
    title: markupText,
    text: "anotation",
    rect: rect,
    font: font
  });
  annotation.TextMarkupAnnotationType = wasmModule.PdfTextMarkupAnnotationType.Highlight;
  annotation.TextMarkupColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightSkyBlue() });

  // Add the text markup annotation to the page's annotation collection
  page.Annotations.Add(annotation);
  y = y + size.Height;

  return y;
};

function AddPopupAnnotation(page, y) {
  // Set up font and formatting
  let font = wasmModule.PdfTrueTypeFont.Create({
    fontFile: "/Library/Fonts/ARIALUNI.TTF",
    size: 12,
    style: wasmModule.PdfFontStyle.Regular
  });
  let format = wasmModule.PdfStringFormat.Create();
  format.MeasureTrailingSpaces = true;

  // Draw the prompt text
  let prompt = "Markup incorrect spelling: ";
  let size = font.MeasureString({ text: prompt, format: format });
  page.Canvas.DrawString({ s: prompt, font: font, brush: wasmModule.PdfBrushes.get_DodgerBlue(), x: 0, y: y });
  let x = size.Width;

  // Draw the label text and bounding rectangle
  let label = "demo of annotation";
  page.Canvas.DrawString({ s: label, font: font, brush: wasmModule.PdfBrushes.get_OrangeRed(), x: x, y: y });
  x = x + font.MeasureString({ text: label, format: format }).Width;
  let markupText = "All words were spelled correctly";
  size = font.MeasureString({ text: markupText });

  // Create a popup annotation and set its properties
  let pointf = wasmModule.PointF.Create({ x: x, y: y });
  let sizef = wasmModule.SizeF.Create({ width: 0, height: 0 });
  let rect = wasmModule.RectangleF.Create({ location: pointf, size: sizef });
  let annotation = wasmModule.PdfPopupAnnotation.Create({ rectangle: rect, text: markupText });
  annotation.Icon = wasmModule.PdfPopupIcon.Paragraph;
  annotation.Open = true;
  annotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Yellow() });

  // Add the popup annotation to the page's annotation collection
  page.Annotations.Add(annotation);
  y = y + size.Height;

  return y;
};

function AddRubberStampAnnotation(page, y) {
  // Set up font and formatting
  let font = wasmModule.PdfTrueTypeFont.Create({
    fontFile: "/Library/Fonts/ARIALUNI.TTF",
    size: 12,
    style: wasmModule.PdfFontStyle.Regular
  });
  let format = wasmModule.PdfStringFormat.Create();
  format.MeasureTrailingSpaces = true;

  // Draw the prompt text
  let prompt = "Markup incorrect spelling: ";
  let size = font.MeasureString({ text: prompt, format: format });
  page.Canvas.DrawString({ s: prompt, font: font, brush: wasmModule.PdfBrushes.get_DodgerBlue(), x: 0, y: y });
  let x = size.Width;

  // Draw the label text and bounding rectangle
  let label = "demo of annotation";
  page.Canvas.DrawString({ s: label, font: font, brush: wasmModule.PdfBrushes.get_OrangeRed(), x: x, y: y });
  x = x + font.MeasureString({ text: label, format: format }).Width;
  let markupText = "Just a draft, not checked.";
  size = font.MeasureString({ text: markupText });

  // Create a rubber stamp annotation and set its properties
  let pointf = wasmModule.PointF.Create({ x: x, y: y });
  let sizef = wasmModule.SizeF.Create({ width: font.Height, height: font.Height });
  let rect = wasmModule.RectangleF.Create({ location: pointf, size: sizef });
  let annotation = wasmModule.PdfRubberStampAnnotation.Create({ rectangle: rect, text: markupText });
  annotation.Icon = wasmModule.PdfRubberStampAnnotationIcon.Draft;
  annotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Plum() });

  // Add the rubber stamp annotation to the page's annotation collection
  page.Annotations.Add(annotation);
  y = y + size.Height;

  return y;
};
```

---

# spire.pdf javascript 3d annotation
## create 3D annotation in PDF document
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Create a new page
let page = doc.Pages.Add();

// Draw a rectangle on the page to define the canvas area for the 3D file.
let rt = wasmModule.RectangleF.Create({ x: 0, y: 80, width: 200, height: 200 });

let annotation = wasmModule.Pdf3DAnnotation.Create({ rectangle: rt, fileName: "filename.u3d" });
annotation.Activation = wasmModule.Pdf3DActivation.Create();
annotation.Activation.ActivationMode = wasmModule.Pdf3DActivationMode.PageOpen;

// Define a 3D view mode and set its parameter
let View = wasmModule.Pdf3DView.Create();
let pdfRgbColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Purple() });
View.Background = wasmModule.Pdf3DBackground.Create({ color: pdfRgbColor });
View.ViewNodeName = "3DAnnotation";
View.RenderMode = wasmModule.Pdf3DRendermode.Create({ style: wasmModule.Pdf3DRenderStyle.Solid });
View.InternalName = "3DAnnotation";
View.LightingScheme = wasmModule.Pdf3DLighting.Create();
View.LightingScheme.Style = wasmModule.Pdf3DLightingStyle.Day;

// Set the 3D view mode for the annotation
annotation.Views.Add(View);

// Add the annotation to Pdf
page.Annotations.Add(annotation);
```

---

# Spire.PDF JavaScript Line Annotation
## Create line annotations in PDF documents
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Get the first page
let page = doc.Pages.Add();

// Create a line annotation
let linePoints = [100, 650, 180, 650];
let lineAnnotation = wasmModule.PdfLineAnnotation.Create({
  linePoints: linePoints,
  text: "This is the first line annotation"
});

// Set the line border
lineAnnotation.lineBorder.BorderStyle = wasmModule.PdfBorderStyle.Solid;
lineAnnotation.lineBorder.BorderWidth = 1;

// Set the line intent
lineAnnotation.LineIntent = wasmModule.PdfLineIntent.LineDimension;

// Set the line style
lineAnnotation.BeginLineStyle = wasmModule.PdfLineEndingStyle.Butt;
lineAnnotation.EndLineStyle = wasmModule.PdfLineEndingStyle.Diamond;

// Set the line flag
lineAnnotation.Flags = wasmModule.PdfAnnotationFlags.Default;

// Set the line color
lineAnnotation.InnerLineColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Green() });
lineAnnotation.BackColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Green() });

// Set the leader line
lineAnnotation.LeaderLineExt = 0;
lineAnnotation.LeaderLine = 0;

// Add the line annotation to the page
page.Annotations.Add(lineAnnotation);

// Create another line annotation
linePoints = [100, 550, 280, 550];
lineAnnotation = wasmModule.PdfLineAnnotation.Create({
  linePoints: linePoints,
  text: "This is the second line annotation"
});
lineAnnotation.lineBorder.BorderStyle = wasmModule.PdfBorderStyle.Underline;
lineAnnotation.lineBorder.BorderWidth = 2;
lineAnnotation.LineIntent = wasmModule.PdfLineIntent.LineArrow;
lineAnnotation.BeginLineStyle = wasmModule.PdfLineEndingStyle.Circle;
lineAnnotation.EndLineStyle = wasmModule.PdfLineEndingStyle.Diamond;
lineAnnotation.Flags = wasmModule.PdfAnnotationFlags.Default;
lineAnnotation.InnerLineColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Pink() });
lineAnnotation.BackColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Pink() });
lineAnnotation.LeaderLineExt = 0;
lineAnnotation.LeaderLine = 0;
page.Annotations.Add(lineAnnotation);

// Create another line annotation
linePoints = [100, 450, 280, 450];
lineAnnotation = wasmModule.PdfLineAnnotation.Create({
  linePoints: linePoints,
  text: "This is the third line annotation"
});
lineAnnotation.lineBorder.BorderStyle = wasmModule.PdfBorderStyle.Beveled;
lineAnnotation.lineBorder.BorderWidth = 2;
lineAnnotation.LineIntent = wasmModule.PdfLineIntent.LineDimension;
lineAnnotation.BeginLineStyle = wasmModule.PdfLineEndingStyle.None;
lineAnnotation.EndLineStyle = wasmModule.PdfLineEndingStyle.None;
lineAnnotation.Flags = wasmModule.PdfAnnotationFlags.Default;
lineAnnotation.InnerLineColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Blue() });
lineAnnotation.BackColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Blue() });
lineAnnotation.LeaderLineExt = 1;
lineAnnotation.LeaderLine = 1;
page.Annotations.Add(lineAnnotation);
```

---

# Spire.PDF JavaScript Link Annotation
## Creating a PDF link annotation with free text annotation
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Create a new page
let page = doc.Pages.Add();

// Define the rectangle for the file link annotation
let rect = wasmModule.RectangleF.Create({ x: 0, y: 40, width: 250, height: 35 });

// Create a file link annotation with the specified rectangle and input file name
let link = wasmModule.PdfFileLinkAnnotation.Create(rect, inputFileName);

// Add the file link annotation to the page's annotation collection
page.Annotations.Add(link);

// Create a free text annotation using the same rectangle
let text = wasmModule.PdfFreeTextAnnotation.Create(rect);
text.Text = "Click here! This is a link annotation.";

// Define the font for the free text annotation
let font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 15 });
text.Font = font;

// Add the free text annotation to the page's annotation collection
page.Annotations.Add(text);
```

---

# PDF Polygon Annotation Creation
## Create and customize a polygon annotation in a PDF document
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Create a new page
let page = doc.Pages.Add();

// Initialize an instance of PdfPolygonAnnotation, specifying all vertex coordinates which can form a complete shape
let point1 = wasmModule.PointF.Create({ x: 0, y: 30 });
let point2 = wasmModule.PointF.Create({ x: 30, y: 15 });
let point3 = wasmModule.PointF.Create({ x: 60, y: 30 });
let point4 = wasmModule.PointF.Create({ x: 45, y: 50 });
let point5 = wasmModule.PointF.Create({ x: 15, y: 50 });
let point6 = wasmModule.PointF.Create({ x: 0, y: 30 });
let points = [point1, point2, point3, point4, point5, point6];
let polygon = wasmModule.PdfPolygonAnnotation.Create(page, points);

// Set the border color, text, border effect and other properties of polygon annotation
polygon.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_PaleVioletRed() });
polygon.Text = "This is a polygon annotation";
polygon.Author = "E-ICEBLUE";
polygon.Subject = "polygon annotation demo";
polygon.BorderEffect = wasmModule.PdfBorderEffect.BigCloud;
polygon.ModifiedDate = wasmModule.DateTime.get_Now();

// Add the annotation to Pdf page
page.Annotations.Add(polygon);
```

---

# Spire.PDF JavaScript Polyline Annotation
## Create polyline annotation in PDF document
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Create a new page
let page = doc.Pages.Add();

// Create a polyline annotation
let point1 = wasmModule.PointF.Create({ x: 0, y: 60 });
let point2 = wasmModule.PointF.Create({ x: 30, y: 45 });
let point3 = wasmModule.PointF.Create({ x: 60, y: 90 });
let point4 = wasmModule.PointF.Create({ x: 90, y: 80 });
let points = [point1, point2, point3, point4];
let polyline = wasmModule.PdfPolyLineAnnotation.Create(page, points);

// Set properties of polyline
polyline.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_PaleVioletRed() });
polyline.Text = "This is a polygon annotation";
polyline.Author = "E-ICEBLUE";
polyline.Subject = "polygon annotation demo";
polyline.Name = "Test Annotation";
polyline.Border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 1 });
polyline.ModifiedDate = wasmModule.DateTime.get_Now();

// Add the annotation into page
page.Annotations.Add(polyline);
```

---

# PDF Annotation Deletion
## Delete all annotations from a PDF document
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Load the PDF file
doc.LoadFromFile({ fileName: inputFileName });

//Remove all annotations
doc.Pages.get_Item(0).Annotations.Clear();

// Clean up resources
doc.Close();
```

---

# Spire.PDF JavaScript Annotation
## Delete annotation from PDF document
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Load the PDF file
doc.LoadFromFile({ fileName: inputFileName });

// Remove the first annotation from the first page
doc.Pages.get_Item(0).Annotations.RemoveAt(0);

// Clean up resources
doc.Close();
```

---

# spire.pdf javascript extract 3d video
## extract 3D video file from Pdf3DAnnotation
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Load the PDF file
doc.LoadFromFile({ fileName: inputFileName });

// Get the first page
let firstPage = doc.Pages.get_Item(0);

// Get the annotation collection of the first page
let annot = firstPage.Annotations;

// Define an int variable
let count = 0;

// Traverse the annotations
for (let i = 0; i < annot.Count; i++) {
  // If it is Pdf3DAnnotation
  if (annot.get_Item(i) instanceof wasmModule.Pdf3DAnnotation) {
    let annot3D = annot.get_Item(i);
    // Get the 3D video data
    let stream = annot3D._3DData;
    // Write the data into .u3d format file
    if (stream != null) {
      const outputFile = "3d_"+count+".u3d";
      wasmModule.FS.writeFile(outputDirectoryName+outputFile, stream);
      count++;
    }
  }
}

// Clean up resources
doc.Close();
```

---

# spire.pdf javascript annotations
## get all annotations from a pdf page
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Load the PDF file
doc.LoadFromFile({ fileName: inputFileName });

// Get all annotations from the first page
let annotations = doc.Pages.get_Item(0).Annotations;

let content = "";

for (let i = 0; i < annotations.Count; i++) {
  // A text annotation will attach a popup annotation since they are father-son relationship.
  // The annotation information exists in the text annotation, so here we mask the blank popup annotation.
  if (annotations.get_Item(i) instanceof wasmModule.PdfPopupAnnotationWidget)
    continue;
  let popupAnnotationWidget = annotations.get_Item(i);
  content = content + "Annotation information: " + "\r\n";
  let annotBase = new wasmModule.PdfAnnotation(popupAnnotationWidget.H);
  content = content + "Text: " + annotBase.Text + "\r\n";
  let modifiedDate = annotBase.ModifiedDate.ToString() + "\r\n";
  content = content + "ModifiedDate: " + modifiedDate + "\r\n";
}

// Clean up resources
doc.Close();
```

---

# spire.pdf javascript annotation
## get particular annotation information from PDF document
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Load the PDF fle
doc.LoadFromFile({ fileName: inputFileName });

let page = doc.Pages.get_Item(0);

// Get the annotation collection from the document
let annotations = page.Annotations;

// Get particular annotation information from the document
let content = "";
if (annotations.get_Item(0) instanceof wasmModule.PdfTextAnnotationWidget) {
  let textAnnotation = annotations.get_Item(0);
  let styledAnnotationWidget = new wasmModule.PdfStyledAnnotationWidget(textAnnotation.H);
  content = content + "Annotation text: " + styledAnnotationWidget.Text + "\r\n";
  let annotBase = new wasmModule.PdfAnnotation(textAnnotation.H);
  content = content + "Annotation ModifiedDate: " + annotBase.ModifiedDate.ToString() + "\r\n";
  let markUpAnnotationWidget = new wasmModule.PdfMarkUpAnnotationWidget(textAnnotation.H);
  content = content + "Annotation author: " + markUpAnnotationWidget.Author + "\r\n";
  content = content + "Annotation Name: " + annotBase.Name + "\r\n";
}

// Clean up resources
doc.Close();
```

---

# Spire.PDF JavaScript Annotation
## Create invisible free text annotation in PDF document
```javascript
// Get the first page
let page = doc.Pages.get_Item(0);

// Define a rectangle with specific coordinates and dimensions
let rect = wasmModule.RectangleF.Create({ x: 100, y: 120, width: 150, height: 30 });

// Create a new free text annotation using the defined rectangle
let FreetextAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);

// Set the text content of the free text annotation
FreetextAnnotation.Text = "Invisible Free Text Annotation";

// Specify the font for the free text annotation
let font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.TimesRoman, size: 10 });

// Specify the border width for the free text annotation
let border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 1 });

// Set various properties for the free text annotation
FreetextAnnotation.Font = font;
FreetextAnnotation.Border = border;
FreetextAnnotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Purple() });
FreetextAnnotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.Circle;
FreetextAnnotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Green() });
FreetextAnnotation.Opacity = 0.8;

// Set flags to make the free text annotation invisible
FreetextAnnotation.Flags = wasmModule.PdfAnnotationFlags.NoView;

// Add the free text annotation to the page's annotations collection
page.AnnotationsWidget.Add(FreetextAnnotation);

// Define a new rectangle for the next free text annotation
rect = wasmModule.RectangleF.Create({ x: 100, y: 180, width: 150, height: 30 });

// Create another free text annotation using the new rectangle
FreetextAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);

// Set properties for the second free text annotation
FreetextAnnotation.Text = "Show Free Text Annotation";
FreetextAnnotation.Font = font;
FreetextAnnotation.Border = border;
FreetextAnnotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightPink() });
FreetextAnnotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.Circle;
FreetextAnnotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightGreen() });
FreetextAnnotation.Opacity = 0.8;

// Add the second free text annotation to the page's annotations collection
page.AnnotationsWidget.Add(FreetextAnnotation);
```

---

# PDF Line Annotation Modification
## Modify properties of a line annotation in a PDF document
```javascript
let page = doc.Pages.get_Item(0);

// Get the annotation collection from the document
let annotation = page.Annotations.get_Item(0);

// Check if the retrieved annotation is a PdfLineAnnotationWidget
if (annotation instanceof wasmModule.PdfLineAnnotationWidget) {
  // Cast the annotation as a PdfLineAnnotationWidget to access its specific properties and methods
  let lineAnn = annotation;

  // Set the author property of the line annotation to "Author_test"
  lineAnn.Author = "Author_test";

  // Set the subject property of the line annotation to "Subject_test"
  lineAnn.Subject = "Subject_test";
}
```

---

# PDF FreeTextAnnotation Alignment
## Set alignment for FreeTextAnnotation in PDF document
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

let page = doc.Pages.Add();

// Define a rectangle with specific coordinates and dimensions
let rect = wasmModule.RectangleF.Create({ x: 0, y: 300, width: 200, height: 80 });

// Create a new PdfFreeTextAnnotation object and pass the rectangle as a parameter
let textAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);

// Set the text content of the free text annotation
textAnnotation.Text = "\n  Spire.PDF";

// Create a new PdfFont object using the Times Roman font family and a font size of 20
let font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.TimesRoman, size: 20 });

// Assign the created font to the font property of the free text annotation
textAnnotation.Font = font;

// Create a new PdfAnnotationBorder object with a specified width
let border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 1 });

// Assign the created border to the border property of the free text annotation
textAnnotation.Border = border;

// Set the border color of the free text annotation to gray
textAnnotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Gray() });

// Set the line ending style of the free text annotation to slash
textAnnotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.Slash;

// Set the background color of the free text annotation to light blue
textAnnotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightBlue() });

// Set the opacity of the free text annotation to 0.8
textAnnotation.Opacity = 0.8;

// Set the alignment of the text within the free text annotation to center
textAnnotation.TextAlignment = wasmModule.PdfAnnotationTextAlignment.Center;

// Add the free text annotation to the annotations collection of the page
page.Annotations.Add(textAnnotation);
```

---

# Spire.PDF JavaScript Free Text Annotation Styling
## Demonstrates how to set free text annotation styles in PDF documents
```javascript
// Get the first page
let page = doc.Pages.get_Item(0);

// Define and create a free text annotation object
let rect = wasmModule.RectangleF.Create({ x: 150, y: 120, width: 150, height: 30 });
let textAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);

// Specify content for the annotation
textAnnotation.Text = "\nFree Text Annotation Formatting";

// Set the formatting for the first free text annotation and add it to the page
let font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.TimesRoman, size: 10 });
let border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 1 });
textAnnotation.Font = font;
textAnnotation.Border = border;
textAnnotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Purple() });
textAnnotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.Circle;
textAnnotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Green() });
textAnnotation.Opacity = 0.8;
page.Annotations.Add(textAnnotation);

// Define a second rectangle for another free text annotation
rect = wasmModule.RectangleF.Create({ x: 150, y: 200, width: 150, height: 40 });
textAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);
textAnnotation.Text = "\nFree Text Annotation Formatting";

// Set formatting for the second annotation
border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 1 });
font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 10 });
textAnnotation.Font = font;
textAnnotation.Border = border;
textAnnotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightGoldenrodYellow() });
textAnnotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.RClosedArrow;
textAnnotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightPink() });
textAnnotation.Opacity = 0.8;
page.Annotations.Add(textAnnotation);

// Define a third rectangle for a different free text annotation
rect = wasmModule.RectangleF.Create({ x: 150, y: 280, width: 280, height: 40 });
textAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);
textAnnotation.Text = "\noHow to Set Free Text Annotation Formatting in Pdf file";

// Set formatting for the third annotation
border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 1 });
font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 10 });
textAnnotation.Font = font;
textAnnotation.Border = border;
textAnnotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Gray() });
textAnnotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.Circle;
textAnnotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightSkyBlue() });
textAnnotation.Opacity = 0.8;
page.Annotations.Add(textAnnotation);

// Define a fourth rectangle for yet another free text annotation
rect = wasmModule.RectangleF.Create({ x: 150, y: 360, width: 200, height: 40 });
textAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);
textAnnotation.Text = "\nFree Text Annotation Formatting";

// Set formatting for the fourth annotation
border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 1 });
font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 10 });
textAnnotation.Font = font;
textAnnotation.Border = border;
textAnnotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Pink() });
textAnnotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.RClosedArrow;
textAnnotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightGreen() });
textAnnotation.Opacity = 0.8;
page.Annotations.Add(textAnnotation);
```

---

# spire.pdf javascript annotation
## set free text annotation subject in PDF
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Load the PDF file
doc.LoadFromFile({ fileName: inputFileName });

// Get the first page
let page = doc.Pages.get_Item(0);

// Initialize a PdfFreeTextAnnotation
let rect = wasmModule.RectangleF.Create({ x: 150, y: 120, width: 150, height: 30 });
let textAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);

// Specify content
textAnnotation.Text = "\nSet free text annotation subject";

// Set subject
textAnnotation.Subject = "SubjectTest";

// Create a new PdfFont object
let font = wasmModule.PdfFont.Create({ fontFamily: wasmModule.PdfFontFamily.TimesRoman, size: 10 });

// Create a new PdfAnnotationBorder object
let border = wasmModule.PdfAnnotationBorder.Create({ borderWidth: 1 });

// Assign the created font to the font property
textAnnotation.Font = font;

// Assign the created border to the border property
textAnnotation.Border = border;

// Set the border color to purple
textAnnotation.BorderColor = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Purple() });

// Set the line ending style to circle
textAnnotation.LineEndingStyle = wasmModule.PdfLineEndingStyle.Circle;

// Set the background color to green
textAnnotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Green() });

// Set the opacity
textAnnotation.Opacity = 0.8;
page.Annotations.Add(textAnnotation);
```

---

# spire.pdf javascript text annotation
## copy text annotation properties to another annotation
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Load the PDF file
doc.LoadFromFile({ fileName: inputFileName });

// Get the first page
let firstPage = doc.Pages.get_Item(0);

// Create a new PDF document object to store the copied annotations
let newPdf = wasmModule.PdfDocument.Create();

// Iterate through each annotation in the first page's annotation list
for (let i = 0; i < firstPage.Annotations.Count; i++) {
  let annotation = firstPage.Annotations.get_Item(i);
  // Check if the annotation is a free text annotation
  if (annotation instanceof wasmModule.PdfFreeTextAnnotationWidget) {
    // Convert the annotation to a free text annotation object
    let textAnnotation = firstPage.Annotations.get_Item(i);

    // Retrieve the rectangle bounds of the annotation
    let rect = textAnnotation.Bounds;

    // Retrieve the text content of the annotation
    let annotBase = new wasmModule.PdfAnnotation(textAnnotation.H);
    let text = annotBase.Text;

    // Create a new page in the new PDF document with the same size as the first page
    let newPage = newPdf.Pages.Add({ size: firstPage.Size });

    // Create a new free text annotation with the same rectangle bounds
    let newAnnotation = wasmModule.PdfFreeTextAnnotation.Create(rect);

    // Set the text content of the new annotation
    newAnnotation.Text = text;

    // Copy the callout lines information from the original annotation
    newAnnotation.CalloutLines = textAnnotation.CalloutLines;

    // Copy the line ending style from the original annotation
    newAnnotation.LineEndingStyle = textAnnotation.LineEndingStyle;

    // Set the annotation intent to indicate it's a free text callout
    newAnnotation.AnnotationIntent = wasmModule.PdfAnnotationIntent.FreeTextCallout;

    // Copy the rectangular differences information from the original annotation
    newAnnotation.RectangleDifferences = textAnnotation.RectangularDifferenceArray;

    // Set the color of the new annotation to match the original annotation
    newAnnotation.Color = textAnnotation.Color;

    // Add the new annotation to the annotations widget of the new page
    newPage.Annotations.Add(newAnnotation);
  }
}
```

---

# Spire.PDF JavaScript Annotation Update
## Update free text annotation color in PDF document
```javascript
// Get the first page
let page = doc.Pages.get_Item(0);

// Get the collection of annotations from the first page of the PDF document
let annotations = page.Annotations;

// Iterate through each free text annotation in the collection
for (let i = 0; i < annotations.Count; i++) {
  let annotation = annotations.get_Item(i);
  // Change the color of the annotation to YellowGreen
  annotation.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_YellowGreen() });
}
```

---

# spire.pdf javascript attachment
## add attachments to pdf document
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Add an attachment to the PDF document
let attachment = wasmModule.PdfAttachment.Create({ fileName: "Header.png" });
attachment.Data = wasmModule.FS.readFile(inputFilePath);
attachment.Description = "Page header picture of demo.";
attachment.MimeType = "image/png";
doc.Attachments.Add({ attachment: attachment });

// Add another attachment to the PDF document
attachment = wasmModule.PdfAttachment.Create({ fileName: "Footer.png" });
attachment.Data = wasmModule.FS.readFile(inputFilePath);
attachment.Description = "Page footer picture of demo.";
attachment.MimeType = "image/png";
doc.Attachments.Add({ attachment: attachment });

// Create a PdfAttachmentAnnotation for the Sales Report Chart attachment
let annotation1 = wasmModule.PdfAttachmentAnnotation.Create({
  rectangle: bounds,
  fileName: "SalesReportChart.png",
  data: data
});
annotation1.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Teal() });
annotation1.Flags = wasmModule.PdfAnnotationFlags.ReadOnly;
annotation1.Icon = wasmModule.PdfAttachmentIcon.Graph;
annotation1.Text = "Sales Report Chart";

// Add the annotation to the page
page.Annotations.Add(annotation1);

// Create a PdfAttachmentAnnotation for the Science Personification Boston
let annotation2 = wasmModule.PdfAttachmentAnnotation.Create({
  rectangle: bounds,
  fileName: "SciencePersonificationBoston.jpg",
  data: data
});
annotation2.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Orange() });
annotation2.Flags = wasmModule.PdfAnnotationFlags.NoZoom;
annotation2.Icon = wasmModule.PdfAttachmentIcon.PushPin;
annotation2.Text = "SciencePersonificationBoston.jpg, from Wikipedia, the free encyclopedia";
page.Annotations.Add(annotation2);

// Create a PdfAttachmentAnnotation for the Wikipedia Science image
let annotation3 = wasmModule.PdfAttachmentAnnotation.Create({
  rectangle: bounds,
  fileName: "Wikipedia_Science.png",
  data: data
});
annotation3.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_SaddleBrown() });
annotation3.Flags = wasmModule.PdfAnnotationFlags.Locked;
annotation3.Icon = wasmModule.PdfAttachmentIcon.Tag;
annotation3.Text = "Wikipedia_Science.png, from Wikipedia, the free encyclopedia";
page.Annotations.Add(annotation3);

// Create a PdfAttachmentAnnotation for the font file
let annotation4 = wasmModule.PdfAttachmentAnnotation.Create({
  rectangle: bounds,
  fileName: "PT_Serif-Caption-Web-Regular.ttf",
  data: data
});
annotation4.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_CadetBlue() });
annotation4.Flags = wasmModule.PdfAnnotationFlags.NoRotate;
annotation4.Icon = wasmModule.PdfAttachmentIcon.Paperclip;
annotation4.Text = "PT_Serif-Caption-Web-Regular, from http://www.1001freefonts.com";
page.Annotations.Add(annotation4);
```

---

# Spire.PDF JavaScript Attachment Management
## Delete all attachments from a PDF document
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Load the PDF file
doc.LoadFromFile({ fileName: "input.pdf" });

// Get all attachments
let attachments = doc.Attachments;

// Delete all attachments
attachments.Clear();

// Save the document to the specified path
doc.SaveToFile({ fileName: "output.pdf" });

// Clean up resources
doc.Close();
```

---

# Spire.PDF JavaScript Get Attachments
## Get all attachments from a PDF document
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Load the PDF file
doc.LoadFromFile({ fileName: inputFileName });

// Get a collection of attachments on the PDF document
let collection = doc.Attachments;

// Save all the attachments to the files
for (let i = 0; i < collection.Count; i++) {
  let attachment = collection.get_Item(i);
  let embeddedFileSpecification = new wasmModule.PdfEmbeddedFileSpecification(attachment.H);
  wasmModule.FS.writeFile(outputDirectoryName+embeddedFileSpecification.FileName, embeddedFileSpecification.Data);
}

// Clean up resources
doc.Close();
```

---

# spire.pdf javascript attachment
## get individual attachment from pdf document
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Load the PDF file
doc.LoadFromFile({ fileName: inputFileName });

// Get a collection of attachments on the PDF document
let collection = doc.Attachments;

// Get the second attachment in PDF file
let attachment = collection.get_Item(1);

// Save the second attachment to the file
let embeddedFileSpecification = new wasmModule.PdfEmbeddedFileSpecification(attachment.H);

// Define the output file name
const outputFileName = embeddedFileSpecification.FileName;

// Write the attachment in specified path
wasmModule.FS.writeFile(outputFileName, embeddedFileSpecification.Data);

// Clean up resources
doc.Close();
```

---

# spire.pdf javascript attachment
## get PDF attachment information
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Load the PDF file
doc.LoadFromFile({ fileName: inputFileName });

// Get a collection of attachments on the PDF document
let collection = doc.Attachments;

// Get the first attachment
let attachment = collection.get_Item(0);
let embeddedFileSpecification = new wasmModule.PdfEmbeddedFileSpecification(attachment.H);

// Get the information of the first attachment
let content = "";
content = content + "Filename: " + embeddedFileSpecification.FileName + "\r\n";
content = content + "Description: " + embeddedFileSpecification.Description + "\r\n";
content = content + "Creation Date: " + embeddedFileSpecification.CreationDate.ToString() + "\r\n";
content = content + "Modification Date: " + embeddedFileSpecification.ModificationDate.ToString() + "\r\n";

// Clean up resources
doc.Close();
```

---

# PDF Attachment Relationship
## Add attachment with specified relationship to PDF document
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Define PdfAttachment
let attachment = wasmModule.PdfAttachment.Create({ fileName: attachmentFileName });

// Add attachment with attachment, associatedDocument, and association
doc.Attachments.Add({
  attachment: attachment,
  associatedDocument: doc,
  association: wasmModule.PdfAttachmentRelationship.Alternative
});

// Clean up resources
doc.Close();
```

---

# spire.pdf javascript sort attachments
## sort PDF attachments by custom fields
```javascript
// Create a new PdfDocument object
let doc = wasmModule.PdfDocument.Create();

// Add a custom field with the name "No", the internal name "number", and the type NumberField to the document's collection
doc.Collection.AddCustomField("No", "number", wasmModule.CustomFieldType.NumberField);

// Add a file-related field with the name "Desc", the internal name "desc", and the type Desc to the document's collection
doc.Collection.AddFileRelatedField("Desc", "desc", wasmModule.FileRelatedFieldType.Desc);

// Sort the document's collection based on the fields "No" and "Desc" in ascending order
let fieldNames = ["No", "Desc"];
let order = [true, true];
doc.Collection.Sort(fieldNames, order);

// Create a PdfAttachment object
let pdfAttachment = wasmModule.PdfAttachment.Create({ fileName: inputFileName1 });

// Add the PdfAttachment object to the document's collection
doc.Collection.AddAttachment(pdfAttachment);

// Create a PdfAttachment object 
pdfAttachment = wasmModule.PdfAttachment.Create({ fileName: inputFileName2 });

// Add the PdfAttachment object to the document's collection
doc.Collection.AddAttachment(pdfAttachment);

// Create a PdfAttachment object 
pdfAttachment = wasmModule.PdfAttachment.Create({ fileName: inputFileName3 });

// Add the PdfAttachment object to the document's collection
doc.Collection.AddAttachment(pdfAttachment);

// Iterate through each PdfAttachment object in the document's associated files
for (let i = 0; i < doc.Collection.AssociatedFiles.length;) {
  let attachment = doc.Collection.AssociatedFiles[i];
  let embeddedFileSpecification = new wasmModule.PdfEmbeddedFileSpecification(attachment.H);

  // Set the value of the "No" field in the attachment to the current counter value
  embeddedFileSpecification.SetFieldValue({ fieldName: "No", intFieldValue: i+1 });

  // Set the value of the "Desc" field in the attachment to the file name of the attachment
  embeddedFileSpecification.SetFieldValue({ fieldName: "Desc", strFieldValue: attachment.FileName });

  // Increment the counter variable
  i++;
}
```

---

# PDF Bookmark Creation
## Demonstrates how to add bookmarks to a PDF document using Spire.PDF for JavaScript
```javascript
//Create a PdfDocument object
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
```

---

# PDF Bookmark Management
## Delete all bookmarks from a PDF document
```javascript
//Get all bookmarks
let bookmarks = doc.Bookmarks;

//Remove
bookmarks.Clear();
```

---

# Spire.PDF JavaScript Bookmark
## Delete a bookmark from PDF document
```javascript
// Create a new PdfDocument object
let doc = wasmModule.PdfDocument.Create();

// Load an existing PDF document from a file.
doc.LoadFromFile({ fileName: inputFileName });

//Delete the first bookmark
doc.Bookmarks.RemoveAt(0);
```

---

# PDF Bookmark Expansion
## Expand all bookmarks in a PDF document recursively
```javascript
function ForeachBookmark(collections, expand) {
  // Check if the collection is empty, and return if it is
  if (collections.Count == 0)
    return;

  // Iterate through each bookmark in the collection
  for (let i = 0; i < collections.Count; i++) {
    let bookmark = collections.get_Item(i);
    // Recursively call the function to process child bookmarks
    ForeachBookmark(bookmark, expand);

    // Set the ExpandBookmark property of the current bookmark to the specified flag
    bookmark.ExpandBookmark = expand;
  }
}

// Create a new PdfDocument object
let doc = wasmModule.PdfDocument.Create();

// Load an existing PDF document from a file
doc.LoadFromFile({ fileName: inputFileName });

// Set true to expand the bookmarks
ForeachBookmark(doc.Bookmarks, true);

// Save the document to the specified path
doc.SaveToFile({ fileName: outputFileName });
doc.Close();
```

---

# PDF Bookmark Expansion Control
## Expand or collapse specific bookmarks in a PDF document
```javascript
// Create a new PdfDocument object
let doc = wasmModule.PdfDocument.Create();

// Load an existing PDF document from a file
doc.LoadFromFile({ fileName: inputFileName });

// Set BookMarkExpandOrCollapse as "true" for the first bookmarks and "false" for the first level of the second bookmarks
doc.Bookmarks.get_Item(0).ExpandBookmark = true;

let collections = doc.Bookmarks.get_Item(1);
let collections1 = collections.get_Item(0);
collections1.ExpandBookmark = false;

// Save the document to the specified path
doc.SaveToFile({ fileName: outputFileName });
doc.Close();
```

---

# spire.pdf javascript bookmarks
## get all bookmarks from pdf document
```javascript
// Get bookmarks collection of the Pdf file.
let bookmarks = doc.Bookmarks;

// Get Pdf bookmarks information.
function GetBookmarks(bookmarks) {
  let content = "";
  
  if (bookmarks.Count > 0) {
    content = "Pdf bookmarks:" + "\r\n";
    
    for (let i = 0; i < bookmarks.Count; i++) {
      let parentBookmark = bookmarks.get_Item(i);
      // Get the title.
      content += parentBookmark.Title + "\r\n";
      // Get the text style.
      let textStyle = parentBookmark.DisplayStyle.toString();
      content += textStyle + "\r\n";
      let childContent = GetChildBookmark(parentBookmark);
      content += childContent;
    }
  }
  return content;
}

function GetChildBookmark(parentBookmark) {
  let childContent = "";
  if (parentBookmark.Count > 0) {
    for (let i = 0; i < parentBookmark.Count; i++) {
      let childBookmark = parentBookmark.get_Item(i);
      // Get the title.
      childContent += childBookmark.Title + "\r\n";
      // Get the text style.
      let textStyle = childBookmark.DisplayStyle.toString();
      childContent += textStyle + "\r\n";
      childContent += GetChildBookmark(childBookmark);
    }
  }
  return childContent;
}

// Get Pdf Bookmarks.
let bookmarkInfo = GetBookmarks(bookmarks);
```

---

# spire.pdf javascript bookmark
## get PDF bookmark page number
```javascript
// Create a new PdfDocument object
let doc = wasmModule.PdfDocument.Create();

// Load an existing PDF document from a file.
doc.LoadFromFile({ fileName: inputFileName });

//Get bookmarks collections of the PDF file.
let bookmarks = doc.Bookmarks;

//Get the page number of the first bookmark.
let bookmark = bookmarks.get_Item(0);
let pageNumber = doc.Pages.IndexOf(bookmark.Destination.Page) + 1;
```

---

# spire.pdf javascript bookmarks
## get child bookmarks from pdf document
```javascript
function GetChildBookmarks(bookmarks, result) {
  //Declare a new StringBuilder content
  let content = "";
  //Get Pdf child Bookmarks information.
  for (let i = 0; i < bookmarks.Count; i++) {
    let parentBookmark = bookmarks.get_Item(i);

    if (parentBookmark.Count > 0) {
      content = content + "Child Bookmarks:" + "\r\n";
      for (let j = 0; j < parentBookmark.Count; j++) {
        let childBookmark = parentBookmark.get_Item(j);
        //Get the title
        content = content + childBookmark.Title + "\r\n";
        //Get the color.
        let color = childBookmark.Color.R + "," + childBookmark.Color.G + "," + childBookmark.Color.B;
        content = content + "color: "+color + "\r\n";
        //Get the text style.
        let textStyle = "";
        if (childBookmark.DisplayStyle == spirepdf.PdfTextStyle.Bold) {
          textStyle = "Bold" + "\r\n";
        } else
          if (childBookmark.DisplayStyle == spirepdf.PdfTextStyle.Italic) {
            textStyle = "Italic" + "\r\n";
          }
        if (childBookmark.DisplayStyle == spirepdf.PdfTextStyle.Regular) {
          textStyle = "Regular" + "\r\n";
        }

        content = content + textStyle;
      }
    }
  }
  wasmModule.FS.writeFile(result, content.toString());
}

// Create a new PdfDocument object
let doc = wasmModule.PdfDocument.Create();

// Load an existing PDF document from a file.
doc.LoadFromFile({ fileName: inputFileName });

//Get bookmarks collections of the PDF file.
let bookmarks = doc.Bookmarks;

//Get Pdf child Bookmarks.
GetChildBookmarks(bookmarks, outputFileName);

// Close
doc.Close();
```

---

# PDF Bookmark Zoom Setting
## Set inherit zoom for PDF bookmark destinations
```javascript
// Create a new PdfDocument object
let doc = wasmModule.PdfDocument.Create();

// Load an existing PDF document from a file
doc.LoadFromFile({ fileName: inputFileName });

// Get bookmarks collections of the PDF file
let bookmarks = doc.Bookmarks;

// Set Zoom level as 0.5 for each bookmark
for (let i = 0; i < bookmarks.Count; i++) {
  let bookMark = bookmarks.get_Item(i);
  bookMark.Destination.Zoom = 0.5;
}

// Close the document
doc.Close();
```

---

# spire.pdf javascript bookmarks
## set inherit zoom property for bookmarks
```javascript
function SetBookmarkAction(bookmark) {
  // Get the destination of the bookmark
  let dest = bookmark.Destination;

  // Set the destination mode to "Location" and the zoom level to 0
  dest.Mode = wasmModule.PdfDestinationMode.Location;
  dest.Zoom = 0;

  // Iterate through each child bookmark recursively
  for (let i = 0; i < bookmark.Count; i++) {
    let childbookmark = bookmark.get_Item(i);
    SetBookmarkAction(childbookmark);
  }
}

// Get the bookmarks collection of the PDF file
let bookmarks = doc.Bookmarks;

// Iterate through each bookmark in the collection
for (let i = 0; i < bookmarks.Count; i++) {
  // Set inherit zoom for the current bookmark
  let bookmark = bookmarks.get_Item(i);
  SetBookmarkAction(bookmark);
}
```

---

# Spire.PDF JavaScript Bookmark Update
## Update PDF bookmarks and their properties recursively
```javascript
// Function to edit the child bookmarks of a parent bookmark recursively
function EditChildBookmark(parentBookmark) {
  // Iterate through each child bookmark of the parent bookmark
  for (let i = 0; i < parentBookmark.Count; i++) {
    let childBookmark = parentBookmark.get_Item(i);
    // Set the color of the child bookmark to Blue
    childBookmark.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Blue() });

    // Set the outline text style of the child bookmark to Regular
    childBookmark.DisplayStyle = wasmModule.PdfTextStyle.Regular;

    // Recursively call EditChild2Bookmark to edit the child's child bookmarks
    EditChild2Bookmark(childBookmark);
  }
}

// Function to edit the child's child bookmarks recursively
function EditChild2Bookmark(childBookmark) {
  // Iterate through each child bookmark of the child bookmark
  for (let i = 0; i < childBookmark.Count; i++) {
    let child2Bookmark = childBookmark.get_Item(i);
    // Set the color of the child's child bookmark to LightSalmon
    child2Bookmark.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_LightSalmon() });

    // Set the outline text style of the child's child bookmark to Italic
    child2Bookmark.DisplayStyle = wasmModule.PdfTextStyle.Italic;
  }
}

// Create a new PdfDocument object
let doc = wasmModule.PdfDocument.Create();

// Load an existing PDF document from a file
doc.LoadFromFile({ fileName: inputFileName });

// Get the first bookmark from the document
let bookmark = doc.Bookmarks.get_Item(0);

// Change the title of the bookmark
bookmark.Title = "Modified BookMark";

// Set the color of the bookmark to Black
bookmark.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Black() });

// Set the outline text style of the bookmark to Bold
bookmark.DisplayStyle = wasmModule.PdfTextStyle.Bold;

// Edit the child bookmarks of the parent bookmark
EditChildBookmark(bookmark);

// Save the document to the specified path
doc.SaveToFile({ fileName: outputFileName });
doc.Close();
```

---

# Spire.PDF JavaScript DateTime Stamp
## Add a datetime stamp to a PDF document using rubber stamp annotation
```javascript
// Create a new PDF document object.
let doc = wasmModule.PdfDocument.Create();
// Load an existing PDF document from a file.
doc.LoadFromFile({ fileName: inputFileName });

// Get the first page of the loaded document.
let page = doc.Pages.get_Item(0);

// Set the font and brush for the text
let trueTypeFont = wasmModule.PdfTrueTypeFont.Create({
  fontFile: "ARIALUNI.TTF",
  size: 12,
  style: spirepdf.PdfFontStyle.Regular
});
let font = trueTypeFont;
let brush = spirepdf.PdfSolidBrush.Create({ color: spirepdf.Color.get_Blue() });

// Generate a string representing the current date and time
let timeString = spirepdf.DateTime.get_Now().ToString({ format: "MM/dd/yy hh:mm:ss tt " });

// Create a template with specified dimensions
let template = spirepdf.PdfTemplate.Create({ width: 140, height: 15 });

// Define a rectangle to position the template on the page
let point = spirepdf.PointF.Create({
  x: page.ActualSize.Width - template.Width - 10,
  y: page.ActualSize.Height - template.Height - 10
});
let rect = spirepdf.RectangleF.Create({ location: point, size: template.Size });

// Draw the time string onto the template
template.Graphics.DrawString({
  s: timeString,
  font: font,
  brush: brush,
  point: spirepdf.PointF.Create({ x: 0, y: 0 })
});

// Create a rubber stamp annotation
let stamp = spirepdf.PdfRubberStampAnnotation.Create({ rectangle: rect });

// Create a custom appearance for the stamp annotation
let appearance = spirepdf.PdfAppearance.Create(stamp);
appearance.Normal = template;

// Assign the custom appearance to the stamp annotation
stamp.Appearance = appearance;

// Add the stamp annotation to the page's annotations widget
page.AnnotationsWidget.Add(stamp);
```

---

# Spire.PDF JavaScript Image Stamp
## Add an image stamp to a PDF document
```javascript
// Get the first page of the loaded document.
let page = doc.Pages.get_Item(0);

// Create a rubber stamp annotation with a specified rectangle for its size and position
let rect = spirepdf.RectangleF.Create({
  location: spirepdf.PointF.Create({ x: 0, y: 0 }),
  size: spirepdf.SizeF.Create({ width: 60, height: 60 })
});

let loStamp = spirepdf.PdfRubberStampAnnotation.Create({ rectangle: rect });

// Create an instance of PdfAppearance for the rubber stamp annotation
let loApprearance = spirepdf.PdfAppearance.Create(loStamp);

// Load an image file to be used as the stamp
let image = spirepdf.PdfImage.FromFile(inputImageName1);

// Create a template with specific dimensions
let template = spirepdf.PdfTemplate.Create({ width: 210, height: 210 });

// Draw the loaded image onto the template
template.Graphics.DrawImage({ image: image, x: 60, y: 60 });

// Set the normal appearance of the stamp to use the created template
loApprearance.Normal = template;

// Assign the custom appearance to the rubber stamp annotation
loStamp.Appearance = loApprearance;

// Add the rubber stamp annotation to the page's annotations widget
page.AnnotationsWidget.Add(loStamp);
```

---

# Spire.PDF JavaScript Text Stamp
## Add text stamp to PDF document
```javascript
// Create a new PDF document object
let doc = wasmModule.PdfDocument.Create();
// Load an existing PDF document from a file
doc.LoadFromFile({ fileName: inputFileName });

// Get the first page of the loaded document
let page = doc.Pages.get_Item(0);

// Create a template with specific dimensions to hold the stamp
let template = spirepdf.PdfTemplate.Create({ width: 125, height: 55 });

// Set the font, brush, and pen for drawing the stamp
let trueTypeFont = wasmModule.PdfTrueTypeFont.Create({
    fontFile: "ELEPHNTI.ttf",
    size: 12,
    style: spirepdf.PdfFontStyle.Italic
});
let font1 = trueTypeFont;

let brush = spirepdf.PdfSolidBrush.Create({ color: spirepdf.Color.get_DarkRed() });
let pen = spirepdf.PdfPen.Create({ brush: brush });

// Define a rectangle that represents the bounds of the stamp
let rectangle = spirepdf.RectangleF.Create({ location: spirepdf.PointF.Create({ x: 5, y: 5 }), size: template.Size });

// Define the corner radius for the stamp's rounded corners
let CornerRadius = 20;

// Create a path for the stamp shape using arcs and lines
let path = spirepdf.PdfPath.Create();
//x, y, width, height, startAngle, sweepAngle
path.AddArc({
  x: template.GetBounds().X,
  y: template.GetBounds().Y,
  width: CornerRadius,
  height: CornerRadius,
  startAngle: 180,
  sweepAngle: 90
});
path.AddArc({
  x: template.GetBounds().X + template.Width - CornerRadius,
  y: template.GetBounds().Y,
  width: CornerRadius,
  height: CornerRadius,
  startAngle: 270,
  sweepAngle: 90
});
path.AddArc({
  x: template.GetBounds().X + template.Width - CornerRadius,
  y: template.GetBounds().Y + template.Height - CornerRadius,
  width: CornerRadius,
  height: CornerRadius,
  startAngle: 0,
  sweepAngle: 90
});
path.AddArc({
  x: template.GetBounds().X,
  y: template.GetBounds().Y + template.Height - CornerRadius,
  width: CornerRadius,
  height: CornerRadius,
  startAngle: 90,
  sweepAngle: 90
});
path.AddLine({
  x1: template.GetBounds().X,
  y1: template.GetBounds().Y + template.Height - CornerRadius,
  x2: template.GetBounds().X,
  y2: template.GetBounds().Y + CornerRadius / 2
});

// Draw the stamp shape on the template
template.Graphics.DrawPath({ pen: pen, path: path });

// Draw the stamp text on the template
let s1 = "REVISED\n";
let s2 = "by E-iceblue at " + spirepdf.DateTime.get_Now().ToString({ format: "MM dd, yyyy" });
template.Graphics.DrawString({ s: s1, font: font1, brush: brush, point: spirepdf.PointF.Create({ x: 5, y: 10 }) });

let trueTypeFont2 = wasmModule.PdfTrueTypeFont.Create({
    fontFile: "Lucida Sans Unicode.ttf",
    size: 9,
    style: spirepdf.PdfFontStyle.Bold
});
let font2 = trueTypeFont2;
template.Graphics.DrawString({ s: s2, font: font2, brush: brush, point: spirepdf.PointF.Create({ x: 2, y: 30 }) });

// Create a rubber stamp annotation with the specified rectangle for its size and position
let stamp = spirepdf.PdfRubberStampAnnotation.Create({ rectangle: rectangle });

// Create an appearance object for the rubber stamp annotation
let apprearance = spirepdf.PdfAppearance.Create(stamp);

// Set the normal appearance of the stamp to use the created template
apprearance.Normal = template;

// Assign the custom appearance to the rubber stamp annotation
stamp.Appearance = apprearance;

// Add the rubber stamp annotation to the page's annotations widget
page.AnnotationsWidget.Add(stamp);
```

---

# Spire.PDF JavaScript Tiling Background
## Add tiling background image to PDF document
```javascript
// Load an image to be used as the background
let image = spirepdf.PdfImage.FromFile("image.png");

// Iterate through each page in the PDF document
for (let i = 0; i < doc.Pages.Count; i++) {
  let page = doc.Pages.get_Item(i);
  // Create a PdfTilingBrush with a size relative to the page canvas
  let brush = spirepdf.PdfTilingBrush.Create({
    size: spirepdf.SizeF.Create({
      width: page.Canvas.Size.Width / 3,
      height: page.Canvas.Size.Height / 5
    })
  });

  // Set the transparency of the brush graphics
  brush.Graphics.SetTransparency({ alpha: 0.3 });

  // Draw the image onto the brush graphics, centered within the brush
  let point = spirepdf.PointF.Create({
    x: (brush.Size.Width - image.Width) / 2,
    y: (brush.Size.Height - image.Height) / 2
  });
  brush.Graphics.DrawImage({ image: image, point: point });

  // Use the brush to draw a rectangle on the page canvas, covering the entire page area
  let rect = spirepdf.RectangleF.Create({ location: spirepdf.PointF.Create({ x: 0, y: 0 }), size: page.Canvas.Size });
  page.Canvas.DrawRectangle({ brush: brush, rectangle: rect });
}
```

---

# PDF Fill and Stroke Text
## Demonstrates how to fill and stroke text in a PDF document
```javascript
// Create a new PDF document object
let doc = wasmModule.PdfDocument.Create();
// Load an existing PDF document
doc.LoadFromFile({ fileName: inputFileName });

// Get the first page of the loaded document
let page = doc.Pages.get_Item(0);

// Define a PDF pen with gray color
let pen = spirepdf.PdfPen.Create({ color: spirepdf.Color.get_Gray() });

// Save the current graphics state
let state = page.Canvas.Save();

// Rotate the page canvas by -20 degrees
page.Canvas.RotateTransform({ angle: -20 });

// Create a PdfStringFormat object and set the character spacing to 5f
let format = spirepdf.PdfStringFormat.Create();
format.CharacterSpacing = 5;

// Draw the string "E-ICEBLUE" on the page using a specified font, pen, position, and format
let font = spirepdf.PdfFont.Create({ fontFamily: spirepdf.PdfFontFamily.Helvetica, size: 45 });
page.Canvas.DrawString({ s: "E-ICEBLUE", font: font, pen: pen, x: 0, y: 500, format: format });

// Restore the graphics state to its previous state
page.Canvas.Restore({ state: state });
```

---

# PDF Image Watermark
## Add image watermark to PDF document
```javascript
// Create a new PDF document object.
let doc = wasmModule.PdfDocument.Create();
// Load an existing PDF document from VFS
doc.LoadFromFile({ fileName: inputFileName });

// Get the first page of the loaded document
let page = doc.Pages.get_Item(0);

// Load the image from a file
let stream = spirepdf.Stream.CreateByFile(inputImageName1);

// Set the loaded image as the background image of the page
page.BackgroundImage = stream;
```

---

# PDF Image Watermark
## Add image watermark to PDF document with transparency
```javascript
// Create a new PDF document object
let doc = wasmModule.PdfDocument.Create();
// Load an existing PDF document from a file
doc.LoadFromFile({ fileName: inputFileName });

// Load an image from a file
let image = spirepdf.PdfImage.FromFile(inputImageName1);

// Get the first page from the document
let page = doc.Pages.get_Item(0);

// Specify the position on the page to insert the image
let position = spirepdf.PointF.Create({ x: 160, y: 260 });

// Save the current state of the canvas and set transparency for the image
page.Canvas.Save();
page.Canvas.SetTransparency({ alphaPen: 0.5, alphaBrush: 0.5, blendMode: spirepdf.PdfBlendMode.Multiply });

// Draw the image on the page using the specified position
page.Canvas.DrawImage({ image: image, point: position });

// Restore the previous state of the canvas
page.Canvas.Restore();

// Save the document to the specified path
doc.SaveToFile({ fileName: outputFileName });
doc.Close();
```

---

# PDF Stamp Properties Configuration
## Set properties for rubber stamp annotations in PDF documents
```javascript
// Get the first page of the loaded document.
let page = doc.Pages.get_Item(0);

// Traverse through each annotation widget on the page.
for (let i = 0; i < page.Annotations.Count; i++) {
  // Check if the current annotation is a rubber stamp annotation.
  if (page.Annotations.get_Item(i) instanceof spirepdf.PdfRubberStampAnnotationWidget) {
    // Cast the annotation to a PdfRubberStampAnnotationWidget.
    let stamp = page.Annotations.get_Item(i);

    // Set properties for the rubber stamp annotation.
    stamp.Author = "TestUser";
    stamp.Subject = "E-iceblue";
    stamp.CreationDate = spirepdf.DateTime.get_Now();
    stamp.ModifiedDate = spirepdf.DateTime.get_Now();
  }
}
```

---

# Spire.PDF JavaScript Text Watermark
## Create text watermark in PDF document
```javascript
// Create a tiling brush for drawing a text watermark.
let size = spirepdf.SizeF.Create({
  width: page.Canvas.ClientSize.Width / 2,
  height: page.Canvas.ClientSize.Height / 3
});
let brush = spirepdf.PdfTilingBrush.Create({ size: size });

// Set transparency for the brush graphics.
brush.Graphics.SetTransparency(0.3);

// Save the current state of the brush graphics.
brush.Graphics.Save();

// Translate and rotate the brush graphics to position the watermark.
brush.Graphics.TranslateTransform(brush.Size.Width / 2, brush.Size.Height / 2);
brush.Graphics.RotateTransform({ angle: -45 });

// Draw the text watermark using the brush graphics.
let font = spirepdf.PdfFont.Create({ fontFamily: spirepdf.PdfFontFamily.Helvetica, size: 24 });
let format = spirepdf.PdfStringFormat.Create({ alignment: spirepdf.PdfTextAlignment.Center });
brush.Graphics.DrawString({
  s: "Spire.Pdf Demo",
  font: font, brush: spirepdf.PdfBrushes.get_Violet(), x: 0, y: 0,
  format: format
});

// Restore the previous state of the brush graphics.
brush.Graphics.Restore();

// Reset the transparency to fully opaque.
brush.Graphics.SetTransparency({ alpha: 1 });

// Draw a rectangle filled with the tiling brush as the watermark on the page.
let rect = spirepdf.RectangleF.Create({
  location: spirepdf.PointF.Create({ x: 0, y: 0 }),
  size: page.Canvas.ClientSize
});
page.Canvas.DrawRectangle({ brush: brush, rectangle: rect });
```

---

# spire.pdf javascript headers
## add different headers to PDF pages
```javascript
// Create a new PDF document object
let doc = wasmModule.PdfDocument.Create();
// Load an existing PDF document from VFS
doc.LoadFromFile({ fileName: inputFileName });

// Define header texts
let header1 = "Header 1";
let header2 = "Header 2";

// Define the font style for the headers
let trueTypeFont = wasmModule.PdfTrueTypeFont.Create({
  fontFile: "ARIALUNI.TTF",
  size: 12,
  style: spirepdf.PdfFontStyle.Bold
});

let font = trueTypeFont

// Define the brush color for the headers
let brush = spirepdf.PdfBrushes.get_Red();

// Define the rectangle to position the header on the first page
let point = spirepdf.PointF.Create({ x: 0, y: 20 });
let size = spirepdf.SizeF.Create({ width: doc.PageSettings.Size.Width, height: 50 });
let rect = spirepdf.RectangleF.Create({ location: point, size: size });

// Define the string format for the headers, aligning them in the center
let format = spirepdf.PdfStringFormat.Create();
format.Alignment = spirepdf.PdfTextAlignment.Center;

// Draw the first header with the defined font, brush, rectangle, and format on the first page of the document
doc.Pages.get_Item(0).Canvas.DrawString({
  s: header1,
  font: font,
  brush: brush,
  layoutRectangle: rect,
  format: format
});

// Change the font style and brush color for the second header
let trueTypeFont1 = wasmModule.PdfTrueTypeFont.Create({
  fontFile: "ARIALUNI.TTF",
  size: 15,
  style: spirepdf.PdfFontStyle.Regular
});
font = trueTypeFont1;
brush = spirepdf.PdfBrushes.get_Black();

// Change the alignment of the string format to left alignment for the second header
format.Alignment = spirepdf.PdfTextAlignment.Left;

// Draw the second header with the updated font, brush, rectangle, and format on the second page of the document
doc.Pages.get_Item(1).Canvas.DrawString({
  s: header2,
  font: font,
  brush: brush,
  layoutRectangle: rect,
  format: format
});
```

---

# PDF Header and Footer Implementation
## Add headers and footers to PDF document with text and images
```javascript
// Create a new PDF document object.
let doc = wasmModule.PdfDocument.Create();
// Load an existing PDF document from a file.
doc.LoadFromFile({ fileName: inputFileName });

// Define the brush for text drawing
let brush = spirepdf.PdfBrushes.get_Black();

// Define the pen for line drawing
let pen = spirepdf.PdfPen.Create({ brush: brush, width: 0.75 });

// Define the font for text drawing
let trueTypeFont1 = wasmModule.PdfTrueTypeFont.Create({
  fontFile: "ARIALUNI.TTF",
  size: 10,
  style: spirepdf.PdfFontStyle.Italic
});
let font = trueTypeFont1

// Define the string format for right-aligned text
let rightAlign = spirepdf.PdfStringFormat.Create({ alignment: spirepdf.PdfTextAlignment.Right });

// Define the string format for left-aligned text
let leftAlign = spirepdf.PdfStringFormat.Create({ alignment: spirepdf.PdfTextAlignment.Left });

rightAlign.MeasureTrailingSpaces = true;
leftAlign.MeasureTrailingSpaces = true;

// Get the page margins of the document
let margin = doc.PageSettings.Margins;

// Calculate the spacing between lines
let space = font.Height * 0.75;

// Initialize variables for position and width calculation
let x = 0;
let y = 0;
let width = 0;

// Create a new PDF document
let newPdf = spirepdf.PdfDocument.Create();
let newPage;

for (let i = 0; i < doc.Pages.Count; i++) {
  let page = doc.Pages.get_Item(i);
  // Add a new page to the new PDF document size, margins
  newPage = newPdf.Pages.Add({ size: page.Size, margins: spirepdf.PdfMargins.Create({ margin: 0 }) });

  // Set transparency for the canvas
  newPage.Canvas.SetTransparency({ alpha: 0.5 });

  // Calculate the position and width for header elements
  x = margin.Left;
  width = page.Canvas.ClientSize.Width - margin.Left - margin.Right;
  y = margin.Top - space;

  // Draw a line as a header separator
  newPage.Canvas.DrawLine({ pen: pen, x1: x, y1: y + 15, x2: x + width, y2: y + 15 });
  y = y + 10 - font.Height;

  // Load the header image
  let headerImage = spirepdf.PdfImage.FromFile(inputImageName1);

  // Draw the header image on the new page
  newPage.Canvas.DrawImage({ image: headerImage, point: spirepdf.PointF.Create({ x: 0, y: 0 }) });

  // Draw the header text on the new page with right alignment
  newPage.Canvas.DrawString({
    s: "Demo of Spire.Pdf",
    font: font,
    brush: brush,
    x: x + width,
    y: y,
    format: rightAlign
  });

  // Load the footer image
  let footerImage = spirepdf.PdfImage.FromFile(inputImageName2);

  // Draw the footer image on the new page
  let point1 = spirepdf.PointF.Create({
    x: 0,
    y: newPage.Canvas.ClientSize.Height - footerImage.PhysicalDimension.Height
  });
  newPage.Canvas.DrawImage({ image: footerImage, point: point1 });

  // Change the font and brush for the footer text
  brush = spirepdf.PdfBrushes.get_DarkBlue();
  let trueTypeFont2 = wasmModule.PdfTrueTypeFont.Create({
    fontFile: "ARIALUNI.TTF",
    size: 12,
    style: spirepdf.PdfFontStyle.Bold
  });
  font = trueTypeFont2;
  y = newPage.Canvas.ClientSize.Height - margin.Bottom - font.Height;

  // Draw the footer text on the new page with left alignment
  newPage.Canvas.DrawString({
    s: "Created by E-iceblue Co,.Ltd",
    font: font,
    brush: brush,
    x: x,
    y: y,
    format: leftAlign
  });

  // Reset the transparency of the canvas
  newPage.Canvas.SetTransparency({ alpha: 1 });

  // Draw the original page content onto the new page
  let template = page.CreateTemplate();
  let graphicsWidget = new spirepdf.PdfGraphicsWidget(template.H);
  graphicsWidget.Draw({ graphics: newPage.Canvas, location: spirepdf.PointF.Create({ x: 0, y: 0 }) });
}
```

---

# PDF Header and Footer Creation
## Add image to header and page numbers to footer in PDF document
```javascript
// Create a new PDF document object
let doc = wasmModule.PdfDocument.Create();
doc.PageSettings.Size = spirepdf.PdfPageSize.A4();

// Reset the default margins to 0
doc.PageSettings.Margins = spirepdf.PdfMargins.Create();

// Create a PdfMargins object, the parameters indicate the page margins you want to set left, top, right, bottom
let margins = spirepdf.PdfMargins.Create({ left: 50, top: 50, right: 50, bottom: 50 });

// Get page size
let pageSize = doc.PageSettings.Size;

// Create a header template with content and apply it to page template
doc.Template.Top = CreateHeaderTemplate(doc, margins, pageSize, inputImageName1);

// Create a footer template with content and apply it to page template
doc.Template.Bottom = CreateFooterTemplate(doc, margins, pageSize);

// Apply blank templates to other parts of page template
doc.Template.Left = spirepdf.PdfPageTemplateElement.Create({
  width: margins.Left,
  height: doc.PageSettings.Size.Height
});
doc.Template.Right = spirepdf.PdfPageTemplateElement.Create({
  width: margins.Right,
  height: doc.PageSettings.Size.Height
});

function CreateHeaderTemplate(doc, margins, pageSize, inputImageName1) {
  // Create a PdfPageTemplateElement object as header space
  let headerSpace = spirepdf.PdfPageTemplateElement.Create({ width: pageSize.Width, height: margins.Top });
  headerSpace.Foreground = false;

  // Declare two float variables
  let x = margins.Left;
  let y = 0;

  // Draw image in header space
  let headerImage = spirepdf.PdfImage.FromFile(inputImageName1);
  let width = headerImage.Width / 2;
  let height = headerImage.Height / 2;
  headerSpace.Graphics.DrawImage({
    image: headerImage,
    x: x,
    y: margins.Top - height - 5,
    width: width,
    height: height
  });

  // Draw line in header space
  let pen = spirepdf.PdfPen.Create({ brush: spirepdf.PdfBrushes.get_LightGray(), width: 1 });
  headerSpace.Graphics.DrawLine({
    pen: pen,
    x1: x,
    y1: y + margins.Top - 2,
    x2: pageSize.Width - x,
    y2: y + margins.Top - 2
  });

  // Return headerSpace
  return headerSpace;
}

function CreateFooterTemplate(doc, margins, pageSize) {
  // Create a PdfPageTemplateElement object to serve as the footer space
  let footerSpace = spirepdf.PdfPageTemplateElement.Create({ width: pageSize.Width, height: margins.Bottom });
  footerSpace.Foreground = false;

  // Declare two float variables for positioning
  let x = margins.Left;
  let y = 0;

  // Draw a line in the footer space using a gray pen
  let pen = spirepdf.PdfPen.Create({ brush: spirepdf.PdfBrushes.get_Gray(), width: 1 });
  footerSpace.Graphics.DrawLine({ pen: pen, x1: x, y1: y, x2: pageSize.Width - x, y2: y });

  // Draw text in the footer space
  y = y + 5;

  // Define the font for the text
  let trueTypeFont1 = wasmModule.PdfTrueTypeFont.Create({
    fontFile: "ARIALUNI.TTF",
    size: 10,
    style: spirepdf.PdfFontStyle.Regular
  });
  let font = trueTypeFont1;

  // Create a dynamic field for the page number
  let number = spirepdf.PdfPageNumberField.Create();

  // Create a dynamic field for the page count
  let count = spirepdf.PdfPageCountField.Create();
  let lists = [number, count];

  // Create a composite field that combines the page number and page count fields
  let compositeField = spirepdf.PdfCompositeField.Create({
    font: font,
    brush: spirepdf.PdfBrushes.get_Black(),
    text: "Page {0} of {1}",
    list: lists
  });
  
  // Set the string format for the composite field (left-aligned, top vertical alignment)
  compositeField.StringFormat = spirepdf.PdfStringFormat.Create({
    alignment: spirepdf.PdfTextAlignment.Left,
    lineAlignment: spirepdf.PdfVerticalAlignment.Top
  });

  // Measure the size of the composite field to determine its bounds
  let size = font.MeasureString({ text: compositeField.Text });
  compositeField.Bounds = spirepdf.RectangleF.Create({ x: x, y: y, width: size.Width, height: size.Height });

  // Draw the composite field on the footer space
  let graphicsWidget = new spirepdf.PdfGraphicsWidget(compositeField.H);
  graphicsWidget.Draw({ graphics: footerSpace.Graphics });

  // Return the footer space element
  return footerSpace;
}
```

---

# PDF Header and Footer with Image and Text
## Create header and footer templates with images and text using Spire.PDF
```javascript
// Create a new PDF document object
let doc = wasmModule.PdfDocument.Create();
// Load an existing PDF document from a file
doc.LoadFromFile({ fileName: inputFileName });

// Get the first page of the loaded document
let page = doc.Pages.get_Item(0);

// Get the margins of Pdf
let margin = doc.PageSettings.Margins;

// Define font and brush
let trueTypeFont1 = wasmModule.PdfTrueTypeFont.Create({
  fontFile: "impact.ttf",
  size: 14,
  style: spirepdf.PdfFontStyle.Regular
});
let font = trueTypeFont1;
let brush = spirepdf.PdfSolidBrush.Create({ color: spirepdf.Color.get_Gray() });

// Load an image
let image = spirepdf.PdfImage.FromFile(inputImageName1);

// Specify the image size
let imageSize = spirepdf.SizeF.Create({ width: image.Width / 2, height: image.Height / 2 });

// Create a header template
let headerTemplate = spirepdf.PdfTemplate.Create({
  width: page.ActualSize.Width - margin.Left - margin.Right,
  height: imageSize.Height
});

// Draw the image in the template
headerTemplate.Graphics.DrawImage({ image: image, point: spirepdf.PointF.Create({ x: 0, y: 0 }), size: imageSize });

// Create a rectangle
let rect = headerTemplate.GetBounds();

// String format alignment, lineAlignment
let format1 = spirepdf.PdfStringFormat.Create({
  alignment: spirepdf.PdfTextAlignment.Right,
  lineAlignment: spirepdf.PdfVerticalAlignment.Middle
});

// Draw a string in the template
headerTemplate.Graphics.DrawString({ s: "Header", font: font, brush: brush, layoutRectangle: rect, format: format1 });

// Create a footer template and draw a text
let footerTemplate = spirepdf.PdfTemplate.Create({
  width: page.ActualSize.Width - margin.Left - margin.Right,
  height: imageSize.Height
});
let format2 = spirepdf.PdfStringFormat.Create({
  alignment: spirepdf.PdfTextAlignment.Center,
  lineAlignment: spirepdf.PdfVerticalAlignment.Middle
});
footerTemplate.Graphics.DrawString({ s: "Footer", font: font, brush: brush, layoutRectangle: rect, format: format2 });

let x = margin.Left;
let y = 0;

// Draw the header template on page at specified location
page.Canvas.DrawTemplate({ template: headerTemplate, location: spirepdf.PointF.Create({ x: x, y: y }) });

// Draw the footer template on page at specified location
y = page.ActualSize.Height - footerTemplate.Height - 10;
page.Canvas.DrawTemplate({ template: footerTemplate, location: spirepdf.PointF.Create({ x: x, y: y }) });
```

---

# PDF Header with Inline Image and Text
## Add inline image and text in PDF header
```javascript
// Create a new PDF document object.
let doc = wasmModule.PdfDocument.Create();
// Load an existing PDF document from a file.
doc.LoadFromFile({ fileName: inputFileName });

// Get the first page of the loaded document.
let page = doc.Pages.get_Item(0);

// Define text and image
let text1 = "Spire.Pdf is a robust component by";
let text2 = "Technology Co., Ltd.";
let image = spirepdf.PdfImage.FromFile(inputImageName1);

// Define font and brush
let trueTypeFont1 = wasmModule.PdfTrueTypeFont.Create({
    fontFile: "impact.ttf",
    size: 10,
    style: spirepdf.PdfFontStyle.Regular
});
let font = trueTypeFont1;
let brush = spirepdf.PdfBrushes.get_DarkGray();

// Measure the size of the text
let s1 = font.MeasureString({ text: text1 });
let s2 = font.MeasureString({ text: text2 });

let x = 10;
let y = 10;

// Define the size of the image
let imgSize = spirepdf.SizeF.Create({ width: image.Width / 2, height: image.Height / 2 });

// Define the rectangle and string format alignment, lineAlignment
let size = spirepdf.SizeF.Create({ width: s1.Width, height: imgSize.Width });
let rect1 = spirepdf.RectangleF.Create({ location: spirepdf.PointF.Create({ x: x, y: y }), size: size });
let format = spirepdf.PdfStringFormat.Create({
  alignment: spirepdf.PdfTextAlignment.Left,
  lineAlignment: spirepdf.PdfVerticalAlignment.Middle
});

// Draw the text1
page.Canvas.DrawString({ s: text1, font: font, brush: brush, layoutRectangle: rect1, format: format });

// Draw the image
x += s1.Width;
page.Canvas.DrawImage({ image: image, point: spirepdf.PointF.Create({ x: x, y: y }), size: imgSize });

// Draw the text2
x += imgSize.Width;
size = spirepdf.SizeF.Create({ width: s2.Width, height: imgSize.Height });
rect1 = spirepdf.RectangleF.Create({ location: spirepdf.PointF.Create({ x: x, y: y }), size: size });
page.Canvas.DrawString({ s: text2, font: font, brush: brush, layoutRectangle: rect1, format: format });
```

---

# PDF Page Number in Footer
## Add page numbers to the footer of a PDF document
```javascript
function DrawPageNumber(doc, margin, startNumber, pageCount) {
  // Iterate through each page in the document
  for (let i = 0; i < doc.Pages.Count; i++) {
    let page = doc.Pages.get_Item(i);
    // Set transparency for the canvas
    page.Canvas.SetTransparency({ alpha: 0.5 });

    // Define brush, pen, font, and string format for drawing the page number
    let brush = spirepdf.PdfBrushes.get_Black();
    let pen = spirepdf.PdfPen.Create({ brush: brush, width: 0.75 });
    let trueTypeFont1 = wasmModule.PdfTrueTypeFont.Create({
      fontFile: "ARIALUNI.TTF",
      size: 12,
      style: spirepdf.PdfFontStyle.Regular
    });
    let font = trueTypeFont1;
    let format = spirepdf.PdfStringFormat.Create({ alignment: spirepdf.PdfTextAlignment.Right });
    format.MeasureTrailingSpaces = true;

    // Calculate the spacing between lines
    let space = font.Height * 0.75;

    // Define the initial position
    let x = margin.Left;
    let width = page.Canvas.ClientSize.Width - margin.Left - margin.Right;
    let y = page.Canvas.ClientSize.Height - margin.Bottom + space;

    // Draw a line above the page number
    page.Canvas.DrawLine({ pen: pen, x1: x, y1: y, x2: x + width, y2: y });

    // Adjust the position and draw the page number label
    y = y + 1;
    let numberLabel = (startNumber++) + " of " + pageCount;
    page.Canvas.DrawString({ s: numberLabel, font: font, brush: brush, x: x + width, y: y, format: format });

    // Reset the transparency for the canvas
    page.Canvas.SetTransparency({ alpha: 1 });
  }
}
```

---

# Spire.PDF JavaScript Launch Action
## Add a PDF launch action to open a file
```javascript
// Create a new PdfDocument object
let doc = wasmModule.PdfDocument.Create();

// Add a new page to the document
let page = doc.Pages.Add();

// Create a PDF Launch Action that will open a text file
let launchAction = spirepdf.PdfLaunchAction.Create({ fileName: inputFileName });

// Create a PDF Action Annotation with the PDF Launch Action
let text = "Click here to open file";
let trueTypeFont1 = wasmModule.PdfTrueTypeFont.Create({
    fontFile: "ARIALUNI.TTF",
    size: 13,
    style: spirepdf.PdfFontStyle.Regular
});
let font = trueTypeFont1;

let rect = spirepdf.RectangleF.Create({ x: 50, y: 50, width: 230, height: 15 });
page.Canvas.DrawString({ s: text, font: font, brush: spirepdf.PdfBrushes.get_ForestGreen(), layoutRectangle: rect });
let annotation = spirepdf.PdfActionAnnotation.Create(rect, launchAction);

// Add the PDF Action Annotation to the page
page.Annotations.Add(annotation);
```

---

# Spire.PDF JavaScript Table of Contents
## Add table of contents to PDF document

```javascript
// Create a new PDF document object
let doc = wasmModule.PdfDocument.Create();

// Load an existing PDF document
doc.LoadFromFile({ fileName: inputFileName });

// Get the total number of pages in the document
let pageCount = doc.Pages.Count;

// Insert a blank page at the beginning of the PDF document
let tocPage = doc.Pages.Insert({ index: 0 });

// Set the title for the table of contents
let title = "Table Of Contents";
let trueTypeFont1 = wasmModule.PdfTrueTypeFont.Create({
    fontFile: "ARIALUNI.TTF",
    size: 20,
    style: spirepdf.PdfFontStyle.Bold
});

let titleFont = trueTypeFont1;
let centerAlignment = spirepdf.PdfStringFormat.Create({
  alignment: spirepdf.PdfTextAlignment.Center,
  lineAlignment: spirepdf.PdfVerticalAlignment.Middle
});
let location = spirepdf.PointF.Create({
  x: tocPage.Canvas.ClientSize.Width / 2,
  y: titleFont.MeasureString(title).Height
});
tocPage.Canvas.DrawString({
  s: title,
  font: titleFont,
  brush: spirepdf.PdfBrushes.get_CornflowerBlue(),
  point: location,
  format: centerAlignment
});

// Draw the table of contents entries
let trueTypeFont2 = wasmModule.PdfTrueTypeFont.Create({
    fontFile: "ARIALUNI.TTF",
    size: 14,
    style: spirepdf.PdfFontStyle.Regular
});
let titlesFont = trueTypeFont2;

let y = titleFont.MeasureString({ text: title }).Height + 10;
let x = 0;

for (let i = 1; i <= pageCount; i++) {
  let text = "This is page" + i;
  let titleSize = titlesFont.MeasureString({ text: text });

  // Get the page that the table of contents entry will navigate to
  let navigatedPage = doc.Pages.get_Item(i);

  let pageNumText = (i + 1).toString();
  let pageNumTextSize = titlesFont.MeasureString({ text: pageNumText });

  // Draw the entry text
  tocPage.Canvas.DrawString({ s: text, font: titlesFont, brush: spirepdf.PdfBrushes.get_CadetBlue(), x: 0, y: y });

  let dotLocation = titleSize.Width + 2 + x;
  let pageNumlocation = tocPage.Canvas.ClientSize.Width - pageNumTextSize.Width;

  // Draw dots between the entry text and page number
  for (let j = dotLocation; j < pageNumlocation; j++) {
    if (dotLocation >= pageNumlocation) {
      break;
    }
    tocPage.Canvas.DrawString({
      s: ".",
      font: titlesFont,
      brush: spirepdf.PdfBrushes.get_Gray(),
      x: dotLocation,
      y: y
    });
    dotLocation += 3;
  }

  // Draw the page number
  tocPage.Canvas.DrawString({
    s: pageNumText,
    font: titlesFont,
    brush: spirepdf.PdfBrushes.get_CadetBlue(),
    x: pageNumlocation,
    y: y
  });

  // Add the table of contents action
  location = spirepdf.PointF.Create({ x: 0, y: y });
  let titleBounds = spirepdf.RectangleF.Create({
    location: location,
    size: spirepdf.SizeF.Create({ width: tocPage.Canvas.ClientSize.Width, height: titleSize.Height })
  });
  let Dest = spirepdf.PdfDestination.Create({
    page: navigatedPage,
    location: spirepdf.PointF.Create({ x: -doc.PageSettings.Margins.Top, y: -doc.PageSettings.Margins.Left })
  });
  let action = spirepdf.PdfActionAnnotation.Create(titleBounds, spirepdf.PdfGoToAction.Create({ destination: Dest }));
  action.Border = spirepdf.PdfAnnotationBorder.Create({ borderWidth: 0 });
  tocPage.Annotations.Add(action);
  y += titleSize.Height + 10;
}
```

---

# PDF Document Link Annotation
## Create a local document link in PDF document
```javascript
// Create a new PDF document.
let pdf = spirepdf.PdfDocument.Create();

// Add pages to the document
let page1 = pdf.Pages.Add();
let page2 = pdf.Pages.Add();

// Add a DocumentLinkAnnotation on the first page and link it to the second page.
AddDocumentLinkAnnotation(pdf, 0, 1, 50);

function AddDocumentLinkAnnotation(pdf, AddPage, DestinationPage, y) {
    // Define a font for text.
    let font = wasmModule.PdfTrueTypeFont.Create({
        fontFile: "ARIALUNI.TTF",
        size: 12,
        style: spirepdf.PdfFontStyle.Regular
    });

    // Set the string format for text alignment.
    let format = spirepdf.PdfStringFormat.Create({alignment: spirepdf.PdfTextAlignment.Left});

    // Specify the prompt text for the link.
    let prompt = "Local document Link: ";

    // Draw the prompt text on the page at the specified vertical position.
    pdf.Pages.get_Item(AddPage).Canvas.DrawString({
        s: prompt,
        font: font,
        brush: spirepdf.PdfBrushes.get_DodgerBlue(),
        x: 0,
        y: y
    });

    // Use MeasureString to get the width of the prompt string.
    let x = font.MeasureString({text: prompt, format: format}).Width;

    // Create a PdfDestination with the specified destination page.
    let dest = spirepdf.PdfDestination.Create({page: pdf.Pages.get_Item(DestinationPage)});

    // Set the location of the destination.
    dest.Location = spirepdf.PointF.Create({x: 0, y: y});

    // Set the zoom factor for the destination.
    dest.Zoom = 0.5;

    // Specify the label string for the link.
    let label = "Click here to link the second page.";

    // Use MeasureString to get the SizeF of the label string.
    let size = font.MeasureString({text: label});

    // Create a rectangle that defines the area for the link.
    let bounds = spirepdf.RectangleF.Create({x: x, y: y, width: size.Width, height: size.Height});

    // Draw the label string on the page.
    pdf.Pages.get_Item(AddPage).Canvas.DrawString({
        s: label,
        font: font,
        brush: spirepdf.PdfBrushes.get_OrangeRed(),
        x: x,
        y: y
    });

    // Create a PdfDocumentLinkAnnotation on the rectangle and link it to the destination.
    let annotation = spirepdf.PdfDocumentLinkAnnotation.Create({rectangle: bounds, destination: dest});

    // Set the color for the annotation.
    annotation.Color = spirepdf.PdfRGBColor.Create({color: spirepdf.Color.get_Blue()});

    // Add the annotation to the page.
    pdf.Pages.get_Item(AddPage).Annotations.Add(annotation);
}
```

---

# PDF Sound Embedding
## Embed sound file in PDF document
```javascript
// Create a new PDF document object.
let doc = wasmModule.PdfDocument.Create();
// Load an existing PDF document from a file.
doc.LoadFromFile({ fileName: inputFileName });

// Get the first page of the loaded document.
let page = doc.Pages.get_Item(0);

// Create a sound action with the specified sound file.
let soundAction = spirepdf.PdfSoundAction.Create(inputFileName1);

// Set properties for the sound action.
soundAction.Sound.Bits = 1;
soundAction.Sound.Channels = spirepdf.PdfSoundChannels.Stereo;
soundAction.Sound.Encoding = spirepdf.PdfSoundEncoding.Signed;
soundAction.Volume = 0.8;
soundAction.Repeat = true;

// Set the sound action to be executed when the PDF document is opened.
doc.AfterOpenAction = soundAction;
```

---

# PDF Link Extraction and Update
## Update link annotations in a PDF document
```javascript
// Create a new PDF document object
let doc = wasmModule.PdfDocument.Create();
// Load an existing PDF document from a file
doc.LoadFromFile({ fileName: inputFileName });

// Get the first page of the loaded document
let page = doc.Pages.get_Item(0);

// Get the annotation collection
let annotations = page.AnnotationsWidget;

// Verify whether widgetCollection is not null or not
if (annotations.Count > 0) {
  // Traverse the PdfAnnotationCollection
  for (let i = 0; i < page.Annotations.Count; i++) {
    // If it is PdfTextWebLinkAnnotationWidget
    if (page.Annotations.get_Item(i) instanceof spirepdf.PdfTextWebLinkAnnotationWidget) {
      // Get the link annotation
      let annotation = page.Annotations.get_Item(i);
      // Change the url
      annotation.Url = "http://www.e-iceblue.com/Introduce/pdf-for-net-introduce.html";
    }
  }
}
```

---

# PDF File Link Annotation
## Creating a file link annotation in PDF document
```javascript
function AddFileLinkAnnotation(page, y) {
    // Define a font
    let trueTypeFont1 = wasmModule.PdfTrueTypeFont.Create({
            fontFile: "ARIALUNI.TTF",
            size: 12,
            style: spirepdf.PdfFontStyle.Regular
        });
    let font = trueTypeFont1;

    // Set the string format
    let format = spirepdf.PdfStringFormat.Create({alignment: spirepdf.PdfTextAlignment.Left});

    // Text string
    let prompt = "Launch a File: ";

    // Draw text string on page canvas
    page.Canvas.DrawString({s: prompt, font: font, brush: spirepdf.PdfBrushes.get_DodgerBlue(), x: 0, y: y});

    // Use MeasureString to get the width of string
    let x = font.MeasureString({text: prompt, format: format}).Width;

    // String of file name
    let label = "Sample.pdf";

    // Use MeasureString to get the SizeF of string
    let size = font.MeasureString({text: label});

    // Create a rectangle
    let bounds = spirepdf.RectangleF.Create({x: x, y: y, width: size.Width, height: size.Height});

    // Draw label string
    page.Canvas.DrawString({s: label, font: font, brush: spirepdf.PdfBrushes.get_OrangeRed(), x: x, y: y});

    // Create PdfFileLinkAnnotation on the rectangle and link file "Sample.pdf"
    let annotation = spirepdf.PdfFileLinkAnnotation.Create(bounds, "Sample.pdf");

    // Set color for annotation
    annotation.Color = spirepdf.PdfRGBColor.Create({color: spirepdf.Color.get_Blue()});

    page.Annotations.Add(annotation);
}
```

---

# Spire.PDF JavaScript Link Annotation
## Extract URL and text from link annotations in PDF documents
```javascript
// Get the first page of the loaded document.
let page = doc.Pages.get_Item(0);

// Get the annotation collection
let annotations = page.Annotations;

// Create string to save results
let content = "";

// Verify whether widgetCollection is not null or not
if (annotations.Count > 0) {
  // Traverse the PdfAnnotationCollection
  for (let i = 0; i < annotations.Count; i++) {
    // If it is PdfTextWebLinkAnnotationWidget
    if (annotations.get_Item(i) instanceof spirepdf.PdfTextWebLinkAnnotationWidget) {
      // Get the link annotation
      let WebLinkAnnotation = annotations.get_Item(i);
      let url = WebLinkAnnotation.Url;
      // Add strings to content
      content = content + "The url of link annotation is " + url + "\r\n";
      content = content + "The text of link annotation is " + WebLinkAnnotation.Text + "\r\n";
    }
  }
}
```

---

# PDF GoTo Action Creation
## Demonstrates how to create Go-To actions in PDF documents
```javascript
function EmbeddedGoToAction(pdf, page) {
  // Add an attachment to the PDF
  let attachment = spirepdf.PdfAttachment.Create({ fileName: "GoToAction.pdf" });
  pdf.Attachments.Add({ attachment: attachment });

  // Specify the text to be displayed on the page
  let text = "Test embedded go-to action! Clicking this will open the attached PDF in a new window.";
  
  // Define the font and dimensions of the text box
  let font = wasmModule.PdfTrueTypeFont.Create({
            fontFile: "ARIALUNI.TTF",
            size: 13,
            style: spirepdf.PdfFontStyle.Regular
        });
  let rect = spirepdf.RectangleF.Create({ x: 0, y: 100, width: 490, height: font.Height * 2.2 });

  // Draw the text on the page
  page.Canvas.DrawString({ s: text, font: font, brush: spirepdf.PdfBrushes.get_Black(), layoutRectangle: rect });

  // Create a PdfDestination with a specific page, location, and zoom factor
  let dest = spirepdf.PdfDestination.Create({ page: page, location: spirepdf.PointF.Create({ x: 0, y: 842 }) });

  // Create a GoToE (Go-To Embedded) action with the specified destination
  let action = spirepdf.PdfEmbeddedGoToAction.Create(attachment.FileName, dest, true);

  // Create a PdfActionAnnotation with the action and the annotation rectangle
  let annotation = spirepdf.PdfActionAnnotation.Create(rect, action);

  // Add the annotation to the page
  page.Annotations.Add(annotation);
}

function JumpToSpecificLocationAction(pdf, page) {
  // Add a new page to the PDF document
  let pagetwo = pdf.Pages.Add();

  // Draw text on the second page
  pagetwo.Canvas.DrawString({
    s: "This is Page Two.",
    font: spirepdf.PdfFont.Create({ fontFamily: spirepdf.PdfFontFamily.Helvetica, size: 20 }),
    brush: spirepdf.PdfSolidBrush.Create({ color: spirepdf.Color.get_Black() }),
    x: 10, y: 10
  });

  // Create a PdfDestination instance and link it to a PdfGoToAction
  let pageBottomDest = spirepdf.PdfDestination.Create({ page: pagetwo });
  pageBottomDest.Location = spirepdf.PointF.Create({ x: 0, y: 5 });
  pageBottomDest.Mode = spirepdf.PdfDestinationMode.Location;
  pageBottomDest.Zoom = 1;
  let action = spirepdf.PdfGoToAction.Create({ destination: pageBottomDest });

  // Define the font, dimensions, and formatting for a button
  let buttonFont = wasmModule.PdfTrueTypeFont.Create({
            fontFile: "ARIALUNI.TTF",
            size: 10,
            style: spirepdf.PdfFontStyle.Bold
        });
  let buttonWidth = 70;
  let buttonHeight = buttonFont.Height * 1.5;
  let format = spirepdf.PdfStringFormat.Create({
    alignment: spirepdf.PdfTextAlignment.Center,
    lineAlignment: spirepdf.PdfVerticalAlignment.Middle
  });
  let buttonBounds = spirepdf.RectangleF.Create({ x: 0, y: 200, width: buttonWidth, height: buttonHeight });

  // Create a rectangle and draw text on the first page
  page.Canvas.DrawRectangle({ brush: spirepdf.PdfBrushes.get_DarkGray(), rectangle: buttonBounds });
  page.Canvas.DrawString({
    s: "To Last Page",
    font: buttonFont,
    brush: spirepdf.PdfBrushes.get_CadetBlue(),
    layoutRectangle: buttonBounds,
    format: format
  });

  // Add the annotation to the first page
  let annotation = spirepdf.PdfActionAnnotation.Create(buttonBounds, action);
  annotation.Border = spirepdf.PdfAnnotationBorder.Create({ borderWidth: 0.75 });
  annotation.Color = spirepdf.PdfRGBColor.Create({ color: spirepdf.Color.get_LightGray() });
  page.Annotations.Add(annotation);
}
```

---

# PDF Launch Action in New Window
## Create PDF annotation to launch a file in a new window when clicked
```javascript
// Create a new PDF document object
let doc = wasmModule.PdfDocument.Create();
// Load an existing PDF document from VFS
doc.LoadFromFile({ fileName: inputFileName });

let collection = null;
let test = ["Spire.PDF"];

// Iterate through each page of the PDF document
for (let pageIndex = 0; pageIndex < doc.Pages.Count; pageIndex++) {
  let page = doc.Pages.get_Item(pageIndex);
  // Iterate through each keyword in the 'test' array
  for (let i = 0; i < test.length; i++) {
    // Create a PdfTextFinder to search for the keyword in the current page
    let finder = spirepdf.PdfTextFinder.Create(page);
    finder.Options.Parameter = spirepdf.TextFindParameter.WholeWord;

    // Find all occurrences of the keyword on the current page
    collection = finder.Find(test[i]);

    // Iterate through each found text fragment
    for (let m = 0; m < collection.length; m++) {
      let find = collection[m];
      // Create a PdfLaunchAction to launch a file when the annotation is clicked
      let launchAction = spirepdf.PdfLaunchAction.Create({
        fileName: inputFileName,
        path: spirepdf.PdfFilePathType.Relative
      });

      // Set the launch action to open the file in a new window
      launchAction.IsNewWindow = true;

      // Get the position and size of the found text fragment
      let rect = spirepdf.RectangleF.Create({
        x: find.Positions[0].X,
        y: find.Positions[0].Y,
        width: find.Sizes[0].Width,
        height: find.Sizes[0].Height
      });

      // Create a PdfActionAnnotation with the launch action and the annotation rectangle
      let annotation = spirepdf.PdfActionAnnotation.Create(rect, launchAction);

      // Add the annotation to the current page
      page.Annotations.Add(annotation);
    }
  }
}
```

---

# Spire.PDF JavaScript Links
## Add various types of links to PDF document
```javascript
// Create a new PDF document
let doc = spirepdf.PdfDocument.Create();

// Set margins for the document
let unitCvtr = spirepdf.PdfUnitConvertor.Create();
let margin = spirepdf.PdfMargins.Create();
margin.Top = unitCvtr.ConvertUnits(2.54, spirepdf.PdfGraphicsUnit.Centimeter, spirepdf.PdfGraphicsUnit.Point);
margin.Bottom = margin.Top;
margin.Left = unitCvtr.ConvertUnits(3.17, spirepdf.PdfGraphicsUnit.Centimeter, spirepdf.PdfGraphicsUnit.Point);
margin.Right = margin.Left;

// Create a new page with specified size and margins
let page = doc.Pages.Add({ size: spirepdf.PdfPageSize.A4(), margins: margin });
let y = 100;
let x = 10;

// Set font and label for the first text link
let trueTypeFont1 = wasmModule.PdfTrueTypeFont.Create({
    fontFile: "Lucida Sans Unicode.ttf",
    size: 14,
    style: spirepdf.PdfFontStyle.Regular
});
let font = trueTypeFont1;
let label = "Simple Text Link: ";

// Draw the label on the page
let format = spirepdf.PdfStringFormat.Create();
format.MeasureTrailingSpaces = true;
page.Canvas.DrawString({ s: label, font: font, brush: spirepdf.PdfBrushes.get_Orange(), x: 0, y: y, format: format });
x = font.MeasureString({ text: label, format: format }).Width;

// Set font and URL for the first text link
let trueTypeFont2 = wasmModule.PdfTrueTypeFont.Create({
    fontFile: "Lucida Sans Unicode.ttf",
    size: 14,
    style: spirepdf.PdfFontStyle.Underline
});
let font1 = trueTypeFont2;
let url1 = "http://www.e-iceblue.com";

// Draw the URL on the page
page.Canvas.DrawString({ s: url1, font: font1, brush: spirepdf.PdfBrushes.get_CadetBlue(), x: x, y: y });
y = y + font1.MeasureString({ text: url1 }).Height + 25;

// Set font and label for the second web link
label = "Web Link: ";
page.Canvas.DrawString({ s: label, font: font, brush: spirepdf.PdfBrushes.get_Orange(), x: 0, y: y, format: format });
x = font.MeasureString({ text: label, format: format }).Width;

// Set text and properties for the second web link
let text = "E-iceblue home";
let link2 = spirepdf.PdfTextWebLink.Create();
link2.Text = text;
link2.Url = url1;
link2.Font = font1;
link2.Brush = spirepdf.PdfBrushes.get_CadetBlue();

// Draw the second web link on the page
link2.DrawTextWebLink({ graphics: page.Canvas, location: spirepdf.PointF.Create({ x: x, y: y }) });
y = y + font1.MeasureString({ text: text }).Height + 30;

// Set font and label for the URI annotation
label = "URI Annotation: ";
page.Canvas.DrawString({ s: label, font: font, brush: spirepdf.PdfBrushes.get_Orange(), x: 0, y: y, format: format });
x = font.MeasureString({ text: label, format: format }).Width;
text = "Google";
let location = spirepdf.PointF.Create({ x: x, y: y });
let size = font1.MeasureString({ text: text });
let linkBounds = spirepdf.RectangleF.Create({ location: location, size: size });

// Create a URI annotation and set its properties
let link3 = spirepdf.PdfUriAnnotation.Create({ rectangle: linkBounds });
link3.Border = spirepdf.PdfAnnotationBorder.Create({ borderWidth: 0 });
link3.Uri = "http://www.google.com";

// Add the URI annotation to the page's annotations collection
page.Annotations.Add(link3);

// Draw the text for the URI annotation
page.Canvas.DrawString({ s: text, font: font1, brush: spirepdf.PdfBrushes.get_CadetBlue(), x: x, y: y });
y = y + size.Height + 30;

// Set font and label for the URI annotation with an action
label = "URI Annotation Action: ";
page.Canvas.DrawString({ s: label, font: font, brush: spirepdf.PdfBrushes.get_Orange(), x: 0, y: y, format: format });
x = font.MeasureString({ text: label, format: format }).Width;
text = "JavaScript Action (Click Me)";
location = spirepdf.PointF.Create({ x: x - 2, y: y - 2 });
size = font1.MeasureString({ text: text });
size = spirepdf.SizeF.Create({ width: size.Width + 5, height: size.Height + 5 });
linkBounds = spirepdf.RectangleF.Create({ location: location, size: size });

// Create a URI annotation with border and color
let link4 = spirepdf.PdfUriAnnotation.Create({ rectangle: linkBounds });
link4.Border = spirepdf.PdfAnnotationBorder.Create({ borderWidth: 0.75 });
link4.Color = spirepdf.PdfRGBColor.Create({ color: spirepdf.Color.get_CadetBlue() });

// Define JavaScript action script
let script = "app.alert({" +
  "cMsg: \"Hello.\"," +
  "nIcon: 3," +
  "cTitle: \"JavaScript Action\"" +
  "});";

// Create a JavaScript action and assign it to the URI annotation
let action = spirepdf.PdfJavaScriptAction.Create(script);
link4.Action = action;

// Add the URI annotation with the JavaScript action to the page's annotations collection
page.Annotations.Add(link4);

// Draw the text for the URI annotation with the JavaScript action
page.Canvas.DrawString({ s: text, font: font1, brush: spirepdf.PdfBrushes.get_CadetBlue(), x: x, y: y });
y = y + size.Height + 30;

// Create a forum link
label = "Need Help:  ";
page.Canvas.DrawString({ s: label, font: font, brush: spirepdf.PdfBrushes.get_Orange(), x: 0, y: y, format: format });
x = font.MeasureString({ text: label, format: format }).Width;
text = "Go to forum to ask questions";
link2 = spirepdf.PdfTextWebLink.Create();
link2.Text = text;
link2.Url = "https://www.e-iceblue.com/forum/components-f5.html";
link2.Font = font1;
link2.Brush = spirepdf.PdfBrushes.get_CadetBlue();
link2.DrawTextWebLink({ graphics: page.Canvas, location: spirepdf.PointF.Create({ x: x, y: y }) });
y = y + font1.MeasureString({ text: text }).Height + 30;

// Create an email link
label = "Contct us:  ";
page.Canvas.DrawString({ s: label, font: font, brush: spirepdf.PdfBrushes.get_Orange(), x: 0, y: y, format: format });
x = font.MeasureString({ text: label, format: format }).Width;
text = "Send an email";
link2 = spirepdf.PdfTextWebLink.Create();
link2.Text = text;
link2.Url = "mailto:support@e-iceblue.com";
link2.Font = font1;
link2.Brush = spirepdf.PdfBrushes.get_CadetBlue();
link2.DrawTextWebLink({ graphics: page.Canvas, location: spirepdf.PointF.Create({ x: x, y: y }) });
```

---

# spire.pdf javascript goto action
## add GoToAction to navigate to specific page when opening PDF
```javascript
// Create a PdfDestination object with specific page (2), location (0, 100), and zoom factor (50%)
let dest = spirepdf.PdfDestination.Create({ pageNumber: 2, location: spirepdf.PointF.Create({ x: 0, y: 100 }), zoom: 0.5 });

// Create a PdfGoToAction object with the destination
let action = spirepdf.PdfGoToAction.Create({ destination: dest });

// Set the open action of the document to the created action
doc.AfterOpenAction = action;
```

---

# spire.pdf javascript pdf/a conversion
## add attachments to pdf/a document
```javascript
// Convert the input PDF file to PDF/A-1b standard and save it to the memory stream
let converter = spirepdf.PdfStandardsConverter.Create({ filePath: inputFileName });
const toA1B_result = "SampleB_2_to_A1B_result.pdf";
converter.ToPdfA1B({ filePath: toA1B_result });
converter.Dispose();

// Create a new PDF document
let newDoc = spirepdf.PdfDocument.Create();

// Load the converted PDF document from the memory stream
newDoc.LoadFromFile({ fileName: toA1B_result });

// Read the data of the first attachment file into a byte array
let data = wasmModule.FS.readFile(inputImageName1)
// Create a PdfAttachment object with the attachment file name and data
let attach1 = spirepdf.PdfAttachment.Create({ fileName: "attachment1.png", data: data });

// Read the data of the second attachment file into a byte array
let data2 = wasmModule.FS.readFile(inputFileName2)
// Create a PdfAttachment object with the attachment file name and data
let attach2 = spirepdf.PdfAttachment.Create({ fileName: "attachment2.pdf", data: data2 });

// Add the attachments to the new PDF document
newDoc.Attachments.Add({ attachment: attach1 });
newDoc.Attachments.Add({ attachment: attach2 });

// Save the document to the specified path
newDoc.SaveToFile({ fileName: outputFileName });
newDoc.Close();
```

---

# PDF to DOCX Conversion with Property Settings
## Demonstrates how to set properties of Word document when converting PDF to DOCX format
```javascript
// Create an instance of the PdfToDocConverter class with the specified input file path
let converter = spirepdf.PdfToDocConverter.Create({ filePath: inputFileName });

// Set various properties for the converted Word document
// Set the title of the Word document to "PDFTODOCX"
converter.DocxOptions.Title = "PDFTODOCX";

// Set the subject of the Word document to "Set document properties."
converter.DocxOptions.Subject = "Set document properties.";

// Set the tags of the Word document to "Test Tags"
converter.DocxOptions.Tags = "Test Tags";

// Set the categories of the Word document to "PDF"
converter.DocxOptions.Categories = "PDF";

// Set the comments of the Word document to "This document is just for testing the properties"
converter.DocxOptions.Commments = "This document is just for testing the properties";

// Set the authors of the Word document to "E-iceblue Support Team"
converter.DocxOptions.Authors = "E-iceblue Support Team";

// Set the last saved by of the Word document to "E-iceblue Support Team"
converter.DocxOptions.LastSavedBy = "E-iceblue Support Team";

// Set the revision version of the Word document to 8
converter.DocxOptions.Revision = 8;

// Set the version of the Word document to "csharp V4.0"
converter.DocxOptions.Version = "csharp V4.0";

// Set the program name of the Word document to "Spire.Pdf for .NET"
converter.DocxOptions.ProgramName = "Spire.Pdf for .NET";

// Set the company of the Word document to "E-iceblue"
converter.DocxOptions.Company = "E-iceblue";

// Set the manager of the Word document to "E-iceblue"
converter.DocxOptions.Manager = "E-iceblue";

// Define the output file name
const outputFileName = "ConvertToWordSettingProperties.docx";
converter.SaveToDocx({fileName: outputFileName});
```

---

# spire.pdf javascript grayscale conversion
## convert PDF to grayscale document
```javascript
// Create an instance of the PdfToDocConverter class with the specified input file path
let converter = spirepdf.PdfGrayConverter.Create({ filePath: inputFileName });

// Define the output file name
const outputFileName = "ConvertToGrayPdf.pdf";
converter.ToGrayPdf({ filePath: outputFileName });
```

---

# PDF to OFD Conversion
## Convert PDF document to OFD format
```javascript
// Create a new PDF document object
let doc = wasmModule.PdfDocument.Create();

// Load an existing PDF document from VFS
doc.LoadFromFile({ fileName: inputFileName });

// Define the output file name
const outputFileName = "ConvertToOFD.ofd";

// Save the document to the specified path
doc.SaveToFile({ fileName: outputFileName, fileFormat: spirepdf.FileFormat.OFD });
doc.Close();
```

---

# Spire.PDF JavaScript Conversion
## Convert encrypted PDF to PDF/A format
```javascript
// Create a PdfStandardsConverter 
let converter = spirepdf.PdfStandardsConverter.Create({ filePath: inputFileName, password: "test" });

// Define the output file name
const outputFileName = "EncryptedPDFToPDFA.pdf";

// Convert file to PdfA2A
converter.ToPdfA2A({ filePath: outputFileName });

// Dispose the converter
converter.Dispose();
```

---

# Spire.PDF JavaScript OFD to PDF Conversion
## Convert OFD documents to PDF format
```javascript
// Create a new OfdConverter object with the input OFD file path
let converter = spirepdf.OfdConverter.Create({ fileName: inputFileName });

// Define the output file name
const outputFileName = "OFDToPDF.pdf";

// Convert file to pdf
converter.ToPdf({ fileName: outputFileName });
converter.Dispose();
```

---

# PDF/A to PDF Conversion
## Convert PDF/A documents to standard PDF format
```javascript
// Create a document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

// Create a new PDF document to draw content on a new file
let newDoc = wasmModule.PdfNewDocument.Create();
newDoc.CompressionLevel = wasmModule.PdfCompressionLevel.None;

// Iterate through each page in the original document
for (let i = 0; i < doc.Pages.Count; i++) {
    let page = doc.Pages.get_Item(i);
    // Get the size of the current page
    let size = page.Size;

    // Add a new page to the new document with the same size and no margins
    let p = newDoc.Pages.Add({size: size, margins: wasmModule.PdfMargins.Create()});

    // Draw the contents of the original page onto the new page
    let template = page.CreateTemplate();
    let layoutWidget = new wasmModule.PdfLayoutWidget(template.H);
    layoutWidget.Draw({page: p, x: 0, y: 0});
}

// Save the document to the specified path
doc.SaveToFile({fileName: outputFileName});
doc.Close();
```

---

# spire.pdf javascript conversion
## convert PDF document to Excel document
```javascript
// Create a document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

// Define the output file name
const outputFileName = "PdfToExcel_out.xlsx";

// Save the document to the specified path
doc.SaveToFile({fileName: outputFileName, fileFormat: wasmModule.FileFormat.XLSX});
doc.Close();
```

---

# PDF to Excel Conversion with Options
## Set conversion options when converting PDF files to Excel files
```javascript
// Create a document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);
doc.ConvertOptions.SetPdfToXlsxOptions(
  wasmModule.XlsxLineLayoutOptions.Create({convertToMultipleSheet: false, rotatedText: true, splitCell: true}));

// Define the output file name
const outputFileName = "PdfToExcelOptions_out.xlsx";

// Save the document to the specified path
doc.SaveToFile({fileName: outputFileName,fileFormat: wasmModule.FileFormat.XLSX});
doc.Close();
```

---

# PDF Hyperlink Removal
## Remove hyperlinks from a PDF document
```javascript
// Create a document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

// Get the first page
let page = doc.Pages.get_Item(0);
// Get the annotation collection
let widgetCollection = page.AnnotationsWidget;
// Verify whether widgetCollection is null or not
if (widgetCollection.Count > 0) {
    for (let i = 0; i < widgetCollection.Count; i++) {
        let annotation = widgetCollection.get_Item(i)
        // Get the TextWebLink Annotation
        if (annotation instanceof wasmModule.PdfTextWebLinkAnnotationWidget) {
            let link = annotation;
            // Remove the TextWebLink annotation
            widgetCollection.Remove(link);
        }
    }
}
```

---

# SVG to PDF Conversion
## Convert SVG file to PDF format using Spire.PDF for JavaScript
```javascript
// Load the sample file
let inputFileName = "template.svg";

//Create a document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromSvg({fileName: inputFileName});

// Define the output file name
const outputFileName = "SVgToPDF_result.pdf";

// Save the document to the specified path
doc.SaveToFile({fileName: outputFileName});
doc.Close();
```

---

# Text to PDF Conversion
## Convert text content to PDF document using Spire.PDF
```javascript
//Create a document
let doc = wasmModule.PdfDocument.Create();
// Add a section to the document.
let section = doc.Sections.Add();

// Add a page to the section.
let page = section.Pages.Add();

// Create a PdfFont object using the Helvetica font with a size of 11.
let font = wasmModule.PdfFont.Create({fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 11});

// Create a PdfStringFormat object for text formatting.
let format = wasmModule.PdfStringFormat.Create();
format.LineSpacing = 20;

// Create a PdfBrush object for text color.
let brush = wasmModule.PdfBrushes.get_Black();

// Create a PdfTextLayout object for text layout options.
let textLayout = wasmModule.PdfTextLayout.Create();
textLayout.Break = wasmModule.PdfLayoutBreakType.FitPage;
textLayout.Layout = wasmModule.PdfLayoutType.Paginate;

// Define the bounds of the text widget on the page.
let bounds = wasmModule.RectangleF.Create({
    location: wasmModule.PointF.Create({x: 10, y: 20}),
    size: page.Canvas.ClientSize
});

// Create a PdfTextWidget object with the specified text, font, and brush.
let textWidget = wasmModule.PdfTextWidget.Create({text: text, font: font, brush: brush});
textWidget.StringFormat = format;

// Draw the text widget on the page using the specified bounds and text layout options.
let layoutWidget = new wasmModule.PdfLayoutWidget(textWidget.H);
layoutWidget.Draw({page: page, layoutRectangle: bounds, format: textLayout});
```

---

# PDF to DOC Conversion
## Convert PDF document to DOC document using Spire.PDF for JavaScript
```javascript
// Create a document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

// Define the output file name
const outputFileName = "ToDoc_result.doc";

// Save the document to the specified path
doc.SaveToFile({fileName: outputFileName, fileFormat: wasmModule.FileFormat.DOC});
doc.Close();
```

---

# PDF to DOCX Conversion
## Convert PDF document to DOCX format using Spire.PDF for JavaScript
```javascript
// Create a document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

// Define the output file name
const outputFileName = "ToDocx_result.docx";

// Save the document to the specified path
doc.SaveToFile({fileName: outputFileName, fileFormat: wasmModule.FileFormat.DOCX});
doc.Close();
```

---

# PDF to DOCX Conversion
## Convert PDF document to DOCX format using flow method
```javascript
//Create a PdfToWordConverter
let converter = wasmModule.PdfToWordConverter.Create({filePath: inputFileName});

// Define the output file name
const outputFileName = "ToDocxNew_result.docx";
converter.SaveToDocx({outfile: outputFileName});
```

---

# PDF to Image Conversion
## Convert PDF document pages to PNG images
```javascript
// Create a document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

// Save to images
for (let i = 0; i < doc.Pages.Count; i++) {
  let outFileName = `ToImage-img-${i}.png`;            
  let pdfstream = doc.SaveAsImage({pageIndex: i});
  pdfstream.Save(outFileName);              
}
```

---

# PDF Linearization Conversion
## Convert PDF to linearized PDF format for fast web viewing
```javascript
// Create PdfToLinearizedPdfConverter
let converter = wasmModule.PdfToLinearizedPdfConverter.Create({filePath:inputFileName});
converter.ToLinearizedPdf({filePath:outputFileName});
```

---

# PDF to PCL Conversion
## Convert PDF document to PCL (Printer Command Language) format
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

// Define the output file name
const outputFileName = "oPCL_result.pcl";

// Save the document to PCL format
doc.SaveToFile({fileName: outputFileName});
doc.Close();
```

---

# PDF to PDF/A Conversion
## Convert PDF document to PDF/A-1B standard
```javascript
// Create PdfStandardsConverter
let converter = wasmModule.PdfStandardsConverter.Create({filePath: inputFileName});
//also supports ToPdfA1B ToPdfA1A ToPdfA2A ToPdfA3A ToPdfA3B ToPdfX1A2001
converter.ToPdfA1B({filePath: outputFileName});
```

---

# PDF to PDF/A-2B Conversion
## Convert PDF documents to PDF/A-2B format using JavaScript
```javascript
// Convert to PDFA file
let converter = wasmModule.PdfStandardsConverter.Create({filePath: "input.pdf"});
// Also supports ToPdfA1B ToPdfA1A ToPdfA2A ToPdfA3A ToPdfA3B ToPdfX1A2001
converter.ToPdfA2B({filePath: "output.pdf"});
```

---

# spire.pdf javascript conversion
## convert PDF to PostScript
```javascript
//Create a document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

// Define the output file name
const outputFileName = "ToPostScript_result.ps";

// Save the document to the specified path
doc.SaveToFile({fileName: outputFileName});
doc.Close();
```

---

# Spire.PDF JavaScript Conversion
## Convert PDF to SVG document
```javascript
// Create a document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

// Define the output file name
const outputFileName = "ToSVG_result.svg";

// Save the document to the specified path
doc.SaveToFile({fileName: outputFileName,fileFormat: wasmModule.FileFormat.SVG});
doc.Close();
```

---

# spire.pdf javascript conversion
## convert PDF pages to images with transparent background
```javascript
//Create a PDF document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

// Set options for converting to transparent background images
doc.ConvertOptions.SetPdfToImageOptions(0);

// Save the first page as an image
let pdfstream = doc.SaveAsImage({pageIndex: 0});
pdfstream.Save(outputFileName);

// Clean up resources
pdfstream.Dispose();
doc.Close();
```

---

# PDF to XLSX Conversion
## Convert PDF document to XLSX format
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

// Define the output file name
const outputFileName = "ToXLSX_result.xlsx";

// Save the document to the specified path
doc.SaveToFile({fileName: outputFileName,fileFormat: wasmModule.FileFormat.XLSX});
doc.Close();
```

---

# PDF to XPS Conversion
## Convert PDF document to XPS format using Spire.PDF
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

// Define the output file name
const outputFileName = "ToXPS_result.xps";

// Save the document to XPS format
doc.SaveToFile({fileName: outputFileName, fileFormat: wasmModule.FileFormat.XPS});
doc.Close();
```

---

# PDF to HTML Conversion with Embedded Images
## Demonstrates how to convert PDF to HTML while embedding images directly in the HTML output
```javascript
// Create a document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

// Set the conversion options to embed images in HTML
doc.ConvertOptions.SetPdfToHtmlOptions(true, true);

// Define the output file name
const outputFileName = "ToHTMLWithEmbedImages_out.html";

// Save the document to the specified path
doc.SaveToFile({fileName: outputFileName, fileFormat: wasmModule.FileFormat.HTML});
doc.Close();
```

---

# PDF to HTML Conversion
## Convert PDF to HTML with embedded SVG
```javascript
// Create a document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

// Set the boolean option to embed SVG when converting to HTML
doc.ConvertOptions.SetPdfToHtmlOptions({useEmbeddedSvg: true});

// Define the output file name
const outputFileName = "ToHTMLWithEmbedingSVG_result.html";

// Save the document to the specified path
doc.SaveToFile({fileName: outputFileName,fileFormat: wasmModule.FileFormat.HTML});
doc.Close();
```

---

# Spire.PDF JavaScript Conversion
## Convert PDF to HTML
```javascript
// Create a document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

// Define the output file name
const outputFileName = "ToHTML_result.html";

// Save the document to the specified path
doc.SaveToFile({fileName: outputFileName, fileFormat: wasmModule.FileFormat.HTML});
doc.Close();
```

---

# PDF to HTML Conversion Split by Pages
## Convert PDF document to HTML files with each page saved as a separate HTML file
```javascript
// Create a document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

// Set the conversion options to split the PDF into HTML files based on pages
// Here, each page will be converted to a separate HTML file
doc.ConvertOptions.SetPdfToHtmlOptions({useEmbeddedSvg: false, useEmbeddedImg: true, maxPageOneFile: 1});

// Define the output file name
const outputFileName = "ToHTMLFilesSplittedByPages_result.html";

// Save the document to the specified path
doc.SaveToFile({fileName: outputFileName, fileFormat: wasmModule.FileFormat.HTML});
doc.Close();
```

---

# PDF to HTML Stream Conversion
## Convert PDF document to HTML stream using Spire.PDF for JavaScript
```javascript
// Create a document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

let outputName = "ToHtmlStream_result.html";

// Create a new memory stream
let ms = wasmModule.Stream.CreateByFile(outputName);

// Save the PDF document to an HTML stream
doc.SaveToStream({stream: ms, fileformat: wasmModule.FileFormat.HTML});

// Write the content of the memory stream to an HTML file
wasmModule.FS.writeFile(outputName, ms.ToArray());
ms.Close();
doc.Close();
```

---

# PDF Page Deletion
## Delete a specific page from a PDF document
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

// Delete the fifth page
doc.Pages.RemoveAt(4);
```

---

# Spire.PDF JavaScript Text Extraction
## Extract text from PDF document pages
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

// Create a buffer to store the extracted text
let sbuffer = "";

// Iterate through each page in the document
for (let i = 0; i < doc.Pages.Count; i++) {
    let page = doc.Pages.get_Item(i);

    // Create a PdfTextExtractor object for the page
    let pdfTextExtractor = wasmModule.PdfTextExtractor.Create(page)

    // Create PdfTextExtractOptions object
    let pdfTextExtractOptions = wasmModule.PdfTextExtractOptions.Create();
    pdfTextExtractOptions.IsExtractAllText = true;

    // Extract the text from the page
    sbuffer += pdfTextExtractor.ExtractText(pdfTextExtractOptions) + "\n";
}

doc.Close();
```

---

# Spire.PDF JavaScript Page Count
## Get the number of pages in a PDF document
```javascript
//Create a PDF document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

//Get the page count
let count = (doc.Pages.Count).toString();
doc.Close();
```

---

# spire.pdf javascript page info
## get PDF page information including dimensions and rotation
```javascript
//Create a PDF document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

let page = doc.Pages.get_Item(0)

//Get the size of page MediaBox based on "point"
let MediaBoxWidth = page.MediaBox.Width
let MediaBoxHeight = page.MediaBox.Height
let MediaBoxX = page.MediaBox.X
let MediaBoxY = page.MediaBox.Y
//Get the size of page BleedBox based on "point"
let BleedBoxWidth = page.BleedBox.Width
let BleedBoxHeight = page.BleedBox.Height
let BleedBoxX = page.BleedBox.X
let BleedBoxY = page.BleedBox.Y
//Get the size of page CropBox based on "point"
let CropBoxWidth = page.CropBox.Width
let CropBoxHeight = page.CropBox.Height
let CropBoxX = page.CropBox.X
let CropBoxY = page.CropBox.Y
//Get the size of page ArtBox based on "point"
let ArtBoxWidth = page.ArtBox.Width
let ArtBoxHeight = page.ArtBox.Height
let ArtBoxX = page.ArtBox.X
let ArtBoxY = page.ArtBox.Y
//Get the size of page TrimBox based on "point"
let TrimBoxWidth = page.TrimBox.Width
let TrimBoxHeight = page.TrimBox.Height
let TrimBoxX = page.TrimBox.X
let TrimBoxY = page.TrimBox.Y
//Get the actual size of page
let actualSizeW = page.ActualSize.Width
let actualSizeH = page.ActualSize.Height
//Gets the rotation angle of the current page
let rotationAngle = page.Rotation
let rotation = rotationAngle.toString();
//Create content to save
let content = "";
//Add page information string to content
content += "MediaBox width: " + MediaBoxWidth.toString() + "pt, height: " + MediaBoxHeight.toString() +
    "pt, RectangleF X: " + MediaBoxX.toString() + "pt, RectangleF Y: " + MediaBoxY.toString() + "pt." + "\n";
content += "BleedBox width: " + BleedBoxWidth.toString() + "pt, height: " + BleedBoxHeight.toString() +
    "pt, RectangleF X: " + BleedBoxX.toString() + "pt, RectangleF Y: " + BleedBoxY.toString() + "pt." + "\n";
content += "CropBox width: " + CropBoxWidth.toString() + "pt, height: " + CropBoxHeight.toString() +
    "pt, RectangleF X: " + CropBoxX.toString() + "pt, RectangleF Y: " + CropBoxY.toString() + "pt." + "\n";
content += "ArtBox width: " + ArtBoxWidth.toString() + "pt, height: " + ArtBoxHeight.toString() +
    "pt, RectangleF X: " + ArtBoxX.toString() + "pt, RectangleF Y: " + ArtBoxY.toString() + "pt." + "\n";
content += "TrimBox width: " + TrimBoxWidth.toString() + "pt, height: " + TrimBoxHeight.toString() +
    "pt, RectangleF X: " + TrimBoxX.toString() + "pt, RectangleF Y: " + TrimBoxY.toString() + "pt." + "\n";
content += "The actual size of the current page width: " + actualSizeW.toString() + "\n";
content += "The actual size of the current page height: " + actualSizeH.toString() + "\n";
content += "The rotation angle of the current page: " + rotation;

doc.Close();
```

---

# PDF Page Label Extraction
## Extract page labels from a PDF document
```javascript
//Create a PDF document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

//Create a buffer to store the extracted text
let sbuffer = "";

//Iterate through each page in the document
for (let i=0;i<doc.Pages.Count;i++) {
    let page = doc.Pages.get_Item(i);
    sbuffer +="The page label of page "+(i+1).toString()+" is \""+page.PageLabel+"\"" + "\n";
}

doc.Close();
```

---

# PDF Page Size Retrieval
## Get PDF page size in different units (points, pixels, inches, and centimeters)
```javascript
// Assuming a PDF document is already loaded
let page = doc.Pages.get_Item(0);

// Get the width of page based on "point"
let pointWidth = page.Size.Width;
// Get the height of page
let pointHeight = page.Size.Height;
// Create PdfUnitConvertor to convert the unit
let unitCvtr = wasmModule.PdfUnitConvertor.Create();
// Convert the size with "pixel"
let pixelWidth = unitCvtr.ConvertUnits(
    pointWidth, wasmModule.PdfGraphicsUnit.Point, wasmModule.PdfGraphicsUnit.Pixel);
let pixelHeight = unitCvtr.ConvertUnits(
    pointHeight, wasmModule.PdfGraphicsUnit.Point, wasmModule.PdfGraphicsUnit.Pixel);
// Convert the size with "inch"
let inchWidth = unitCvtr.ConvertUnits(
    pointWidth, wasmModule.PdfGraphicsUnit.Point, wasmModule.PdfGraphicsUnit.Inch);
let inchHeight = unitCvtr.ConvertUnits(
    pointHeight, wasmModule.PdfGraphicsUnit.Point, wasmModule.PdfGraphicsUnit.Inch);
// Convert the size with "centimeter"
let centimeterWidth = unitCvtr.ConvertUnits(
    pointWidth, wasmModule.PdfGraphicsUnit.Point, wasmModule.PdfGraphicsUnit.Centimeter);
let centimeterHeight = unitCvtr.ConvertUnits(
    pointHeight, wasmModule.PdfGraphicsUnit.Point, wasmModule.PdfGraphicsUnit.Centimeter);
```

---

# PDF Page Insertion
## Insert an empty page into a PDF document
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

// Insert a blank page as the second page
doc.Pages.Insert(1)
```

---

# PDF Page Insertion
## Insert empty page at the end of PDF document
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

// Add an empty page at the end
doc.Pages.Add(wasmModule.PdfPageSize.A4(), wasmModule.PdfMargins.Create())

// Save the document
doc.SaveToFile({fileName: outputFileName});
doc.Close();
```

---

# PDF Page Labels Management
## Add and change page labels in PDF document
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

doc.PageLabels = wasmModule.PdfPageLabels.Create();
doc.PageLabels.AddRange({
  indexOfFirstPage: 0, 
  numberStyle: wasmModule.PdfPageLabels.Decimal_Arabic_Numerals_Style, 
  prefix: "label test"
});

// Save the document
doc.SaveToFile({fileName: outputFileName});
doc.Close();
```

---

# PDF Page Setup
## Demonstrates how to configure page settings in PDF documents including margins, size, orientation, and rotation
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Set the margin
let unitCvtr = wasmModule.PdfUnitConvertor.Create();
let margin = wasmModule.PdfMargins.Create();
margin.Top = unitCvtr.ConvertUnits(2.54, wasmModule.PdfGraphicsUnit.Centimeter, wasmModule.PdfGraphicsUnit.Point);
margin.Bottom = margin.Top;
margin.Left = unitCvtr.ConvertUnits(3.17, wasmModule.PdfGraphicsUnit.Centimeter, wasmModule.PdfGraphicsUnit.Point);
margin.Right = margin.Left;

// Create one page
let page = doc.Pages.Add({size: wasmModule.PdfPageSize.A4(), margins: margin});
page.BackgroundColor = wasmModule.Color.get_Chocolate();

// Create another page with different background color
page = doc.Pages.Add({size: wasmModule.PdfPageSize.A4(), margins: margin});
page.BackgroundColor = wasmModule.Color.get_Coral();

// Create a page with A3 size, landscape orientation and 180 degree rotation
page = doc.Pages.Add(wasmModule.PdfPageSize.A3(), margin, wasmModule.PdfPageRotateAngle.RotateAngle180, wasmModule.PdfPageOrientation.Landscape);
page.BackgroundColor = wasmModule.Color.get_LightPink();

// Create section with A4 size and custom margins
let section = doc.Sections.Add();
page = section.Pages.Add();
section.PageSettings.Size = wasmModule.PdfPageSize.A4();
section.PageSettings.Margins = margin;

// Set background color for a page
page = section.Pages.Add();
page.BackgroundColor = wasmModule.Color.get_LightSkyBlue();

// Create section with landscape orientation
section = doc.Sections.Add();
section.PageSettings.Orientation = wasmModule.PdfPageOrientation.Landscape;
page = section.Pages.Add();
section.PageSettings.Size = wasmModule.PdfPageSize.A4();
section.PageSettings.Margins = margin;

// Create section with 90 degree rotation
section = doc.Sections.Add();
page = section.Pages.Add();
section.PageSettings.Size = wasmModule.PdfPageSize.A4();
section.PageSettings.Margins = margin;
section.PageSettings.Rotate = wasmModule.PdfPageRotateAngle.RotateAngle90;

// Create section with 180 degree rotation
section = doc.Sections.Add();
page = section.Pages.Add();
section.PageSettings.Size = wasmModule.PdfPageSize.A4();
section.PageSettings.Margins = margin;
section.PageSettings.Rotate = wasmModule.PdfPageRotateAngle.RotateAngle180;
```

---

# PDF Pagination
## Create paginated PDF document with headers, footers, and page numbers
```javascript
// Create a PDF document
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

// Create fonts
let font_regular = wasmModule.PdfTrueTypeFont.Create({fontFile:inputArialFont,size: 8,style: wasmModule.PdfFontStyle.Regular});
let font_bold = wasmModule.PdfTrueTypeFont.Create({fontFile:inputArialFont,size: 8,style: wasmModule.PdfFontStyle.Bold});

// Draw cover and content
DrawCover(doc.Sections.Add(), margin,font_regular,font_bold,inputImageFile,inputImageFile1,inputFileName2);
DrawContent(doc.Sections.Add(), margin,font_regular,inputImageFile,inputImageFile1,content);
DrawPageNumber(doc.Sections.get_Item(1), margin,
    1, doc.Sections.get_Item(1).Pages.Count,font_regular);

function DrawPageHeaderAndFooter(page, margin, isCover,font_regular,inputImageFile,inputImageFile1) {
    page.Canvas.SetTransparency({alpha:0.5});
    // Icon
    let headerImage = wasmModule.PdfImage.FromFile(inputImageFile);
    let footerImage = wasmModule.PdfImage.FromFile(inputImageFile1);
    page.Canvas.DrawImage(headerImage, wasmModule.PointF.Create({x:0.0,y: 0.0}));
    page.Canvas.DrawImage(footerImage, wasmModule.PointF.Create({x:
        0.0, y:page.Canvas.ClientSize.Height - footerImage.PhysicalDimension.Height}));
    if (isCover) {
        page.Canvas.SetTransparency({alpha:1.0});
    }

    let brush = wasmModule.PdfBrushes.get_Black();
    let pen = wasmModule.PdfPen.Create({brush:brush, width:0.75});
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
    let format2 = wasmModule.PdfStringFormat.Create();
    format2.LineSpacing = font_regular.Height * 1.4;
    format2.MeasureTrailingSpaces = true;

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

        // Draw text
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
```

---

# PDF Page Size Reset
## Demonstrates how to reset page size of PDF document by scaling each page
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

// Set the margins
let margins = wasmModule.PdfMargins.Create();
// Create a new pdf document
let newDoc = wasmModule.PdfDocument.Create();
let scale = 0.8;

for (let i = 0; i < doc.Pages.Count; i++) {
    let page = doc.Pages.get_Item(i);
    // Use same scale to set width and height
    let width = page.Size.Width * scale;
    let height = page.Size.Height * scale;
    // Add new page with expected width and height
    let newPage = newDoc.Pages.Add({
        size: wasmModule.SizeF.Create({width: width, height: height}),
        margins: margins
    });
    newPage.Canvas.ScaleTransform(scale, scale);
    // Copy content of original page into new page
    newPage.Canvas.DrawTemplate({
        template: page.CreateTemplate(),
        location: wasmModule.PointF.Create()
    });
}
```

---

# Spire.PDF JavaScript Page Rotation
## Rotate an existing PDF page
```javascript
//Create a PDF document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile(inputFileName);

//Get the first page of the loaded PDF file
let page = doc.Pages.get_Item(0);
//Get the original rotation angle
let rotation = page.Rotation.value
//Set the angle
rotation += wasmModule.PdfPageRotateAngle.RotateAngle270.value;
//Rotate the PDF page
page.Rotation = rotation;
```

---

# spire.pdf javascript rotation
## rotate a new PDF page
```javascript
// Create a new PDF document
let doc = wasmModule.PdfDocument.Create();

// Create PdfSection
let section = doc.Sections.Add()

// Set "A4" for Pdf page
section.PageSettings.Size = wasmModule.PdfPageSize.A4()

// Set rotating angle
section.PageSettings.Rotate = wasmModule.PdfPageRotateAngle.RotateAngle90

// Add the page
let page = section.Pages.Add()
```

---

# PDF Page Orientation Setting
## Set PDF page orientation based on image dimensions
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Add a section
let section = doc.Sections.Add()

// Load an image
let image = wasmModule.PdfImage.FromFile(inputFileName)
// Check whether the width of the image file is greater than default page width or not
if (image.PhysicalDimension.Width > section.PageSettings.Size.Width) {
    // Set the orientation as landscape
    section.PageSettings.Orientation = wasmModule.PdfPageOrientation.Landscape;
} else {
    section.PageSettings.Orientation = wasmModule.PdfPageOrientation.Portrait;
}
// Add a new page with orientation Landscape
let page = section.Pages.Add();
// Draw the image on the page
page.Canvas.DrawImage({image:image, point:wasmModule.PointF.Create({x:0,y:0})});
```

---

# PDF Tab Order Setting
## Set tab order in PDF document using structure method
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

doc.LoadFromFile({fileName: inputFileName});

// Set using document structure
doc.FileInfo.IncrementalUpdate = false;
let page = doc.Pages.get_Item(0);
// Set the tab order of the page using the structure method
page.SetTabOrder(wasmModule.TabOrder.Structure); 
```

---

# Spire.PDF JavaScript Page Splitting
## Split a PDF page into multiple pages
```javascript
// Load the document from disk
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile({fileName: inputFileName});

// Get the first page
let page = doc.Pages.get_Item(0);
// Create a new Pdf
let newPdf = wasmModule.PdfDocument.Create();
// Remove all the margins
newPdf.PageSettings.Margins.All = 0.0;
// Set the page size of new Pdf
newPdf.PageSettings.Width = page.Size.Width;
newPdf.PageSettings.Height = page.Size.Height / 2;
// Add a new page
let newPage = newPdf.Pages.Add();
// Specify the text layout settings for drawing the page content
let format = wasmModule.PdfTextLayout.Create();
format.Break = wasmModule.PdfLayoutBreakType.FitPage;
format.Layout = wasmModule.PdfLayoutType.Paginate;
// Draw the page in the new page
page.CreateTemplate().Draw(newPage, wasmModule.PointF.Create({x:0,y:0}), format);
```

---

# PDF Document Splitting
## Split PDF document by particular page
```javascript
//Create a PDF document
let doc = wasmModule.PdfDocument.Create();

doc.LoadFromFile({fileName: inputFileName});

//Set using document structure
doc.FileInfo.IncrementalUpdate = false;
let page = doc.Pages.get_Item(0);
page.SetTabOrder(wasmModule.TabOrder.Structure);

// Define the output file name
const outputFileName = "SplitFileByParticularPage_result.pdf";

// Save the document to the specified path
doc.SaveToFile({fileName: outputFileName});
doc.Close();
```

---

# PDF Page Content Zooming
## Zoom PDF page contents to fit a different page size
```javascript
// Load the document from disk
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile({fileName: inputFileName});

// Create a new document
let newDoc = wasmModule.PdfDocument.Create();
for (let i=0;i<doc.Pages.Count;i++) {
    let page = doc.Pages.get_Item(i);

    // Add new page with 'A3' size
    let newPage = newDoc.Pages.Add(wasmModule.PdfPageSize.A3(), wasmModule.PdfMargins.Create(0.0,0.0))
    // Zoom content to the new page
    newPage.Canvas.ScaleTransform(newPage.ActualSize.Width / page.ActualSize.Width, (newPage.ActualSize.Height / page.ActualSize.Height))
    // Draw the page to new page
    newPage.Canvas.DrawTemplate(page.CreateTemplate(), wasmModule.PointF.Create(0.0, 0.0))
}
```

---

# Spire.PDF JavaScript Layers
## Add layers to PDF document
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

let page = doc.Pages.Add();
// Create a new layer named "red line" with visibility set to "On"
let layer = doc.Layers.AddLayer({name:"red line",state:wasmModule.PdfVisibility.On});
// Create a graphics context for drawing on the specified page's canvas using the created layer
let pcA = layer.CreateGraphics(page.Canvas)
// Draw a red line on the graphics context using a pen with thickness 2, starting from (100, 350) to (300, 350)
pcA.DrawLine({pen:wasmModule.PdfPen.Create({brush:wasmModule.PdfBrushes.get_Red(), width:2.0}), point1:spirepdf.PointF.Create({x:100,y:350}), point2:spirepdf.PointF.Create({x:300,y:350})})
//create a layer named "blue line"
layer = doc.Layers.AddLayer({name:"blue line"});
let pcB = layer.CreateGraphics(page.Canvas)
pcA.DrawLine({pen:wasmModule.PdfPen.Create({brush:wasmModule.PdfBrushes.get_Blue(), width:2.0}), point1:spirepdf.PointF.Create({x:100,y:400}), point2:spirepdf.PointF.Create({x:300,y:400})})
//create a layer named "green line"
layer = doc.Layers.AddLayer({name:"green line"});
let pcC = layer.CreateGraphics(page.Canvas)
pcA.DrawLine({pen:wasmModule.PdfPen.Create({brush:wasmModule.PdfBrushes.get_Green(), width:2.0}), point1:spirepdf.PointF.Create({x:100,y:450}), point2:spirepdf.PointF.Create({x:300,y:450})})
```

---

# PDF Booklet Creation
## Create a booklet in PDF document with specified dimensions and duplex printing mode
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();
// Set the width and height for the booklet, which is double the width of A4 size and the same height as A4
let width = wasmModule.PdfPageSize.A4().Width * 2
let height = wasmModule.PdfPageSize.A4().Height
// Create a booklet by using the CreateBooklet method with the specified source PDF, width, height, and duplex printing mode (true)
doc.CreateBooklet({fileName: 'source.pdf', width: width, height: height, bothSides: true});
```

---

# Spire.PDF JavaScript Version Change
## Change PDF document version
```javascript
//Create a PDF document
let doc = wasmModule.PdfDocument.Create();

doc.LoadFromFile(inputFileName);

//Change the pdf version
doc.FileInfo.Version = wasmModule.PdfVersion.Version1_6
```

---

# PDF Document Compression
## How to compress a PDF document using Spire.PDF for JavaScript
```javascript
let compressor = wasmModule.PdfCompressor.Create({filePath: inputFileName});
// Enable compression of PDF content
compressor.Options.CompressContents = true;
// Configure text compression options
compressor.Options.TextCompressionOptions.UnembedFonts = true;
compressor.Options.TextCompressionOptions.CompressFonts = true;
// Configure image compression options
compressor.Options.ImageCompressionOptions.ResizeImages = true;
compressor.Options.ImageCompressionOptions.CompressImage = true;
compressor.Options.ImageCompressionOptions.ImageQuality = wasmModule.ImageQuality.High;
```

---

# PDF Document Compression
## Compress PDF document using PdfCompressor with image compression options
```javascript
// Create a new instance of PdfCompressor with the specified input PDF file path
let compressor = wasmModule.PdfCompressor.Create({filePath: inputFileName});

// Set compression options for the compressor
compressor.Options.ImageCompressionOptions.ResizeImages = true;
compressor.Options.ImageCompressionOptions.ImageQuality = wasmModule.ImageQuality.Low;

// Define the output file name
const outputFileName = "CompressDocumentSecondApproach_result.pdf";

// Save the document to the specified path
compressor.CompressToFile(outputFileName);
```

---

# spire.pdf javascript multilayer
## create PDF with text and image layers
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Creates a page
let page = doc.Pages.Add();
// Create text
let text = "Welcome to evaluate Spire.PDF for javascript !";
let format1 = wasmModule.PdfStringFormat.Create({alignment:wasmModule.PdfTextAlignment.Left});
let brush = wasmModule.PdfSolidBrush.Create({color:wasmModule.Color.get_Black()});
let font = wasmModule.PdfTrueTypeFont.Create({fontFile: "/Library/Fonts/ARIALUNI.TTF",size: 15,style: wasmModule.PdfFontStyle.Regular});

let x = 50.0;
let y = 50.0;
// Draw text layer
page.Canvas.DrawString({s:text, font:font, brush:brush,point:wasmModule.PointF.Create({x:x,y:y}), format:format1});
// Get the size of text
let size = font.MeasureString({text:"Welcome to  evaluate",format: format1});
let size2 = font.MeasureString({text:"Spire.PDF for Python",format: format1});

// Loads an image
let image = wasmModule.PdfImage.FromFile(imgFileName);
// Draw image layer
page.Canvas.DrawImage({image:image,point: wasmModule.PointF.Create({x:x + size.Width,y: y}),size:size2});

// Save the document to the specified path
doc.SaveToFile({fileName: outputFileName});
doc.Close();
```

---

# PDF/A Standards Conversion with SpirePDF
## Convert PDF to various PDF/A standards (A1B, A1A, A2A, A2B, A3A, A3B) and PDF/X-1a:2001 format
```javascript
// Create a new PDF document
let doc = wasmModule.PdfDocument.Create();
let page = doc.Pages.Add({
  size: wasmModule.PdfPageSize.A4(),
  margins: wasmModule.PdfMargins.Create({ margin: 40 }),
});

// Convert to PDFA-1B
function CreatePDFA1WithwasmModule_A1B() {
  let converter = wasmModule.PdfStandardsConverter.Create({
    filePath: "CreatePDFA1WithwasmModule_Temp.pdf",
  });
  converter.ToPdfA1B({ filePath: "CreatePDFA1WithwasmModule_A1B.pdf" });
}

// Convert to PDFA-1A
function CreatePDFA1WithwasmModule_A1A() {
  let converter = wasmModule.PdfStandardsConverter.Create({
    filePath: "CreatePDFA1WithwasmModule_Temp.pdf",
  });
  converter.ToPdfA1A({ filePath: "CreatePDFA1WithwasmModule_A1A.pdf" });
}

// Convert to PDFA-2A
function CreatePDFA1WithwasmModule_A2A() {
  let converter = wasmModule.PdfStandardsConverter.Create({
    filePath: "CreatePDFA1WithwasmModule_Temp.pdf",
  });
  converter.ToPdfA2A({ filePath: "CreatePDFA1WithwasmModule_A2A.pdf" });
}

// Convert to PDFA-2B
function CreatePDFA1WithwasmModule_A2B() {
  let converter = wasmModule.PdfStandardsConverter.Create({
    filePath: "CreatePDFA1WithwasmModule_Temp.pdf",
  });
  converter.ToPdfA2B({ filePath: "CreatePDFA1WithwasmModule_A2B.pdf" });
}

// Convert to PDFA-3A
function CreatePDFA1WithwasmModule_A3A() {
  let converter = wasmModule.PdfStandardsConverter.Create({
    filePath: "CreatePDFA1WithwasmModule_Temp.pdf",
  });
  converter.ToPdfA3A({ filePath: "CreatePDFA1WithwasmModule_A3A.pdf" });
}

// Convert to PDFA-3B
function CreatePDFA1WithwasmModule_A3B() {
  let converter = wasmModule.PdfStandardsConverter.Create({
    filePath: "CreatePDFA1WithwasmModule_Temp.pdf",
  });
  converter.ToPdfA3B({ filePath: "CreatePDFA1WithwasmModule_A3B.pdf" });
}

// Convert to PDF/X-1a:2001
function CreatePDFA1WithwasmModule_X1A2001() {
  let converter = wasmModule.PdfStandardsConverter.Create({
    filePath: "CreatePDFA1WithwasmModule_Temp.pdf",
  });
  converter.ToPdfX1A2001({ filePath: "CreatePDFA1WithwasmModule_X1A2001.pdf" });
}
```

---

# Spire.PDF JavaScript Portfolio Creation
## Create a PDF portfolio by combining multiple PDF files
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Load an existing PDF document from a file
doc.LoadFromFile({fileName: inputFileName});

let files = ["SampleB_2.pdf","SampleB_3.pdf"]
// Iterate through the files array and add each file to the document's collection
for (let i = 0; i < files.length; i++) {
  // Add the current file to the subfolder
  doc.Collection.Folders.AddFile({filePath: files[i]});
}

// Save the document to the specified path
doc.SaveToFile({fileName: outputFileName});
doc.Close();
```

---

# Spire.PDF JavaScript Two-Column Document
## Create a PDF document with two columns of text
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Creates a new page
let page = doc.Pages.Add()

// Get width and height of page
let pageWidth = page.GetClientSize().Width;
let pageHeight = page.GetClientSize().Height;
let format = wasmModule.PdfStringFormat.Create({alignment:wasmModule.PdfTextAlignment.Justify});
let brush = wasmModule.PdfBrushes.get_Black()
let font = wasmModule.PdfFont.Create({fontFamily:wasmModule.PdfFontFamily.TimesRoman, size:20});

// Draw text in two columns
let rect1 = wasmModule.RectangleF.Create({x: 0, y: 20, width: pageWidth / 2 - 8, height: pageHeight});
let rect2 = wasmModule.RectangleF.Create({x: pageWidth / 2 + 8, y: 20, width: pageWidth / 2 - 8, height: pageHeight});
page.Canvas.DrawString({s:s1, font:font, brush:brush,layoutRectangle:rect1, format:format});
page.Canvas.DrawString({s:s2, font:font, brush:brush,layoutRectangle:rect2, format:format});
```

---

# Spire.PDF JavaScript Custom Properties
## Add custom document properties to PDF
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Load an existing PDF document from a file
doc.LoadFromFile({fileName: inputFileName});

// Custom document properties
doc.DocumentInformation.SetCustomProperty("Company", "E-iceblue")
doc.DocumentInformation.SetCustomProperty("Component", "Spire.PDF for .NET")
doc.DocumentInformation.SetCustomProperty("Name", "Daisy")
doc.DocumentInformation.SetCustomProperty("Team", "SalesTeam")
```

---

# spire.pdf javascript layer management
## delete layer from pdf document
```javascript
//Create a PDF document
let doc = wasmModule.PdfDocument.Create();

doc.LoadFromFile({fileName: inputFileName});

//Delete the "red line" layer
doc.Layers.RemoveLayer({name:"red line"})
```

---

# PDF Document Properties Extraction
## Extract document properties from a PDF file
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Load the PDF file
doc.LoadFromFile({fileName: inputFileName});

// Get document information
let docInfo = doc.DocumentInformation;
let outContent = "";
outContent += "Author:" + docInfo.Author + "\n";
outContent += "Creation Date: " + docInfo.CreationDate.ToString() + "\n";
outContent += "Keywords: " + docInfo.Keywords + "\n";
outContent += "Modify Date: " + docInfo.ModificationDate.ToString() + "\n";
outContent += "Subject: " + docInfo.Subject + "\n";
outContent += "Title: " + docInfo.Title + "\n";

// Close the document
doc.Close();
```

---

# spire.pdf javascript viewer preference
## get PDF document viewer preferences
```javascript
//Create a PDF document
let doc = wasmModule.PdfDocument.Create();

doc.LoadFromFile({fileName: inputFileName});
let outContent = "";
let viewer = doc.ViewerPreferences
outContent+="Whether the documents window position is in the center: "+"\n";
outContent+="CenterWindow: " + viewer.CenterWindow.toString()+"\n";
outContent+="Document displaying mode, i.e. show thumbnails, full-screen, show attachment panel: "+"\n";
outContent+="PageMode: " + viewer.PageMode.toString()+"\n";
outContent+="The page layout, i.e. single page, one column: "+"\n";
outContent+="PageLayout: " + viewer.PageLayout.toString()+"\n";
outContent+="Whether window's title bar should display document title: "+"\n";
outContent+="DisplayTitle: " + viewer.DisplayTitle.toString()+"\n";
outContent+="Whether to resize the document's window to fit the size of the firstdisplayed page: "+"\n";
outContent+="FitWindow: " + viewer.FitWindow.toString()+"\n";
outContent+="Whether to hide menu bar of the viewer application: "+"\n";
outContent+="HideMenubar: " + viewer.HideMenubar.toString()+"\n";
outContent+="Whether to hide tool bar of the viewer application: "+"\n";
outContent+="HideToolbar: " + viewer.HideToolbar.toString()+"\n";
outContent+="Whether to hide UI elements like scroll bars and leave only the page contents displayed: "+"\n";
outContent+="HideWindowUI: " + viewer.HideWindowUI.toString()+"\n";
```

---

# spire.pdf javascript get zoom factor
## get zoom factor from pdf document
```javascript
//Create a PDF document
let doc = wasmModule.PdfDocument.Create();

doc.LoadFromFile({fileName: inputFileName});
let action = wasmModule.PdfGoToAction.Create({destination:doc.AfterOpenAction})
// Get the zoom factor value from the destination of the action
let zoomvalue = action.Destination.Zoom
let result = "The zoom factor of the document is "+ (zoomvalue*100).toFixed(2).toString() +"%."+"\n";
```

---

# spire.pdf javascript layers
## make all pdf layers invisible
```javascript
//Create a PDF document
let doc = wasmModule.PdfDocument.Create();

doc.LoadFromFile({fileName: inputFileName});

for (let i=0;i<doc.Layers.Count;i++)
{
    //Set all the Pdf layers invisible.
    doc.Layers.get_Item(i).Visibility = wasmModule.PdfVisibility.Off
}

doc.Close();
```

---

# Spire.PDF JavaScript Layers
## Make particular PDF layers invisible
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

doc.LoadFromFile({fileName: inputFileName});
// Set the first layer invisible
doc.Layers.get_Item(0).Visibility = wasmModule.PdfVisibility.Off

// Set the layer named "blue line" invisible
doc.Layers.get_Item({name:"blue line"}).Visibility = wasmModule.PdfVisibility.Off
```

---

# PDF Portfolio Check
## Determine if a PDF document is a portfolio
```javascript
//Create a PDF document
let doc = wasmModule.PdfDocument.Create();

doc.LoadFromFile({fileName: inputFileName});
let result = ""

//Judge whether the document is portfolio or not.
let value = doc.IsPortfolio
if(value)
{
    result="The document is portfolio"
}
else
{
    result="The document is not portfolio"
}

doc.Close();
```

---

# Spire.PDF JavaScript Document Merge
## Merge multiple PDF documents into one
```javascript
//Pdf document list
let files = [inputFileName1, inputFileName2, inputFileName3]
//Open pdf documents
let docs = Array.from({length: files.length}, () => null);
let i = 0;
while (i < files.length) {
    docs[i] = wasmModule.PdfDocument.Create();
    docs[i].LoadFromFile({fileName: files[i]});
    i++;
}
//Append document
docs[0].AppendPage({doc:docs[1]})
//Import page
for (let i = 0; i < docs[2].Pages.Count; i += 2) {
    docs[0].InsertPage({ldDoc:docs[2], pageIndex:i});
}

// Save the document to the specified path
docs[0].SaveToFile({fileName: outputFileName});
docs[0].Close();
```

---

# PDF Document Merger
## Merge multiple PDF documents into one file
```javascript
let inputFiles = [inputFileName1, inputFileName2, inputFileName3];
let mergeOp = spirepdf.MergerOptions.Create();

// Define the output file name
const outputFileName = "MergePdfsByFile_result.pdf";

// Merge the PDF files
spirepdf.PdfMerger.Merge({inputFiles: inputFiles, outputFile: outputFileName, pdfMergeOptions: mergeOp});
```

---

# PDF Page Margin Modification
## Modify page margins of a PDF document
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

doc.LoadFromFile({fileName: inputFileName});

// Creates a new pdf document
let newDoc = wasmModule.PdfDocument.Create();

// Defines the page margins of the new document
let top = 50.0
let bottom = 50.0
let left = 50.0
let right = 50.0
for (let i=0;i<doc.Pages.Count;i++)
{
    let page = doc.Pages.get_Item(i);
    // Adds a new page to the new document and set the page size to be the same as the source document
    let newPage = newDoc.Pages.Add(page.Size, wasmModule.PdfMargins.Create(0.0));
    newPage.Canvas.ScaleTransform((page.ActualSize.Width - left - right) / page.ActualSize.Width, (page.ActualSize.Height - top - bottom) / page.ActualSize.Height);
    newPage.Canvas.DrawTemplate(page.CreateTemplate(), wasmModule.PointF.Create(left, top));
}
```

---

# spire.pdf javascript document properties
## set PDF document information and file properties
```javascript
//Set document info
doc.DocumentInformation.Author = "E-iceblue"
doc.DocumentInformation.Creator = "E-iceblue"
doc.DocumentInformation.Keywords = "pdf, demo, document information"
doc.DocumentInformation.Producer = "Spire.Pdf"
doc.DocumentInformation.Subject = "Demo of Spire.Pdf"
doc.DocumentInformation.Title = "Document Information"

//File info
doc.FileInfo.CrossReferenceType = wasmModule.PdfCrossReferenceType.CrossReferenceStream
doc.FileInfo.IncrementalUpdate = false
```

---

# PDF Page Rearrangement
## Rearrange the order of pages in a PDF document
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

// Load the PDF file
doc.LoadFromFile({fileName: inputFileName});

// Rearrange the page order
doc.Pages.ReArrange([1, 0])

// Save the document
doc.SaveToFile({fileName: outputFileName});
doc.Close();
```

---

# PDF Page Margins Removal
## Remove margins from PDF pages by creating a new document with adjusted dimensions
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile({fileName: inputFileName});

// Create a new pdf document
let newDoc = wasmModule.PdfDocument.Create();

// Get page margins of source pdf page
let margins = doc.PageSettings.Margins
let top = margins.Left
let bottom = margins.Bottom
let left = margins.Left
let right = margins.Right

for (let i = 0; i < doc.Pages.Count; i++) {
    let page = doc.Pages.get_Item(i);

    // Add a new page to the new document with adjusted size
    let newPage = newDoc.Pages.Add(
        wasmModule.SizeF.Create(page.Size.Width - left - right, page.Size.Height - top - bottom), 
        wasmModule.PdfMargins.Create(0.0)
    );
    
    // Draw the content of the source page onto the new document page with margin offset
    newPage.Canvas.DrawTemplate(page.CreateTemplate(), wasmModule.PointF.Create(-left, -top));
}
```

---

# Spire.PDF JavaScript Expiry Date
## Set expiry date for PDF document using JavaScript action
```javascript
//Create a PDF document
let doc = wasmModule.PdfDocument.Create();

doc.LoadFromFile({fileName: inputFileName});

// Define a JavaScript code snippet to check if the document has expired
let javaScript = "var rightNow = new Date();" + "var endDate = new Date('October 20, 2015 23:59:59');" + "if(rightNow.getTime() > endDate)" + "app.alert('This document has expired, please contact us for a new one.',1);" + "this.closeDoc();";
   
// Create a new PdfJavaScriptAction object with the defined JavaScript code
let js = wasmModule.PdfJavaScriptAction.Create(javaScript);
doc.AfterOpenAction = js;

// Save the document to the specified path
doc.SaveToFile({fileName: outputFileName});
doc.Close();
```

---

# PDF Layer Print Properties
## Set print properties for PDF layers
```javascript
// Create a layer named "red line" within the Pdf document
let layer = doc.Layers.AddLayer({name: "red line", state: wasmModule.PdfVisibility.On});

// Set the print state of the layer as "Nerver"
layer.PrintState = wasmModule.LayerPrintState.Nerver;

// Draw a red line on the layer using the graphics of the page canvas
let pcA = layer.CreateGraphics(page.Canvas);
let pdfRGBColor = wasmModule.PdfRGBColor.Create({red: 255, green: 0, blue: 0});
let pen = wasmModule.PdfPen.Create({pdfRgbColor: pdfRGBColor, width: 2});
pcA.DrawLine({
    pen: pen,
    point1: wasmModule.PointF.Create({x: 100, y: 350}),
    point2: wasmModule.PointF.Create({x: 300, y: 350})
});
```

---

# Spire.PDF JavaScript Magnification
## Set PDF magnification to fit height
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

doc.LoadFromFile({fileName: inputFileName});

// Get the first page
let page = doc.Pages.get_Item(0);

// Create a PdfDestination with specific page, location
let dest = wasmModule.PdfDestination.Create({page: page, location: wasmModule.PointF.Create({x: -40, y: -40})});

// Set the Magnification to Fit-Height
dest.Mode = wasmModule.PdfDestinationMode.FitV;

// Create GoToAction with dest
let gotoaction = wasmModule.PdfGoToAction.Create({destination: dest});

// Set open action
doc.AfterOpenAction = gotoaction;
doc.ViewerPreferences.PageMode = wasmModule.PdfPageMode.UseOutlines;
```

---

# PDF XMP Metadata Setting
## Set XMP metadata properties for a PDF document
```javascript
// Create a PDF document
let doc = wasmModule.PdfDocument.Create();

doc.LoadFromFile({fileName: inputFileName});

// Set XMP metadata for the document
doc.DocumentInformation.Author = "E-iceblue";
doc.DocumentInformation.Creator = "Spire.PDF";
doc.DocumentInformation.Keywords = "XMP";
doc.DocumentInformation.Producer = "E-icenlue Co,.Ltd";
doc.DocumentInformation.Subject = "XMP Metadata";
doc.DocumentInformation.Title = "Set XMP Metadata in PDF";

// Save the document to the specified path
doc.SaveToFile({fileName: outputFileName});
doc.Close();
```

---

# PDF Zoom Factor Setting
## Set zoom factor for PDF document
```javascript
//Create a PDF document
let doc = wasmModule.PdfDocument.Create();

doc.LoadFromFile({fileName: inputFileName});

//Get the first page
let page = doc.Pages.get_Item(0);
//Set pdf destination
let dest = wasmModule.PdfDestination.Create({page:page});
dest.Mode = wasmModule.PdfDestinationMode.Location;
dest.Location = wasmModule.PointF.Create(-40.0, -40.0);
dest.Zoom = 0.6;
//Set action
let gotoAction = wasmModule.PdfGoToAction.Create({destination:dest});
doc.AfterOpenAction = gotoAction;

// Save the document to the specified path
doc.SaveToFile({fileName: outputFileName});
doc.Close();
```

---

# PDF Document Splitting
## Split a PDF document into multiple PDF documents
```javascript
//Create a PDF document
let doc = wasmModule.PdfDocument.Create();

doc.LoadFromFile({fileName: inputFileName});

const outFileName = "SplitDocument_result-{0}.pdf";

//Split document
doc.Split(outFileName)
```

---

# Spire.PDF JavaScript Template
## Create and configure PDF document templates with headers, footers, and section templates

```javascript
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
```

---

# spire.pdf javascript page transitions
## demonstrates how to work with PDF page transitions
```javascript
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

// Add pages to the section
let page = section.Pages.Add();
page.BackgroundColor = wasmModule.Color.get_Red();
page = section.Pages.Add();
page.BackgroundColor = wasmModule.Color.get_Green();
page = section.Pages.Add();
page.BackgroundColor = wasmModule.Color.get_Blue();

// Create a new section with different transition settings
section = doc.Sections.Add();
section.PageSettings.Size = wasmModule.PdfPageSize.A4();
section.PageSettings.Margins = margin;
// Configure the transition settings for this section's pages
section.PageSettings.Transition.Duration = 2.0;
section.PageSettings.Transition.Style = wasmModule.PdfTransitionStyle.Box;
section.PageSettings.Transition.PageDuration = 1.0;

// Add pages to the section
page = section.Pages.Add();
page.BackgroundColor = wasmModule.Color.get_Orange();
page = section.Pages.Add();
page.BackgroundColor = wasmModule.Color.get_Brown();
page = section.Pages.Add();
page.BackgroundColor = wasmModule.Color.get_Navy();

// Create another section with different transition settings
section = doc.Sections.Add();
section.PageSettings.Size = wasmModule.PdfPageSize.A4();
section.PageSettings.Margins = margin;
// Configure the transition settings for this section's pages
section.PageSettings.Transition.Duration = 2.0;
section.PageSettings.Transition.Style = wasmModule.PdfTransitionStyle.Split;
section.PageSettings.Transition.Dimension = wasmModule.PdfTransitionDimension.Vertical;
section.PageSettings.Transition.Motion = wasmModule.PdfTransitionMotion.Inward;
section.PageSettings.Transition.PageDuration = 1.0;

// Add pages to the section
page = section.Pages.Add();
page.BackgroundColor = wasmModule.Color.get_Orange();
page = section.Pages.Add();
page.BackgroundColor = wasmModule.Color.get_Brown();
page = section.Pages.Add();
page.BackgroundColor = wasmModule.Color.get_Navy();
```

---

# Spire.PDF JavaScript Viewer Preferences
## Set PDF document view preferences
```javascript
//Create a PDF document
let doc = wasmModule.PdfDocument.Create();
doc.LoadFromFile({fileName: inputFileName});

//Set view reference
// Center the window of the PDF viewer
doc.ViewerPreferences.CenterWindow = true
// Do not display the title of the PDF document in the viewer
doc.ViewerPreferences.DisplayTitle = false
// Do not fit the content of the PDF document to the window of the viewer
doc.ViewerPreferences.FitWindow = false
// Hide the menu bar of the PDF viewer
doc.ViewerPreferences.HideMenubar = true
// Hide the toolbar of the PDF viewer
doc.ViewerPreferences.HideToolbar = true
// Display the PDF document as a single page
doc.ViewerPreferences.PageLayout = wasmModule.PdfPageLayout.SinglePage
```

---

