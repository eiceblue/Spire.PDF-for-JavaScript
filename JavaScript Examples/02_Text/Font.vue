<template>
  <span>The following example demonstrates how to work with font in PDF document.</span>
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
 
        // Create a pdf instance
        let doc = wasmModule.PdfDocument.Create();

        //Get one page
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

        let fontFamilies = ["Helvetica", "Courier", "TimesRoman", "Symbol", "ZapfDingbats"];
        let y = 100;
        for (let i = 0; i < fontFamilies.length; i++) {
            let text = "Font Family:" + fontFamilies[i];

            let x1 = 40;
            y = 100 + i * 16;

            //Define font
            let font1 = wasmModule.PdfFont.Create({fontFamily: wasmModule.PdfFontFamily.Courier, size: 14});
            let font2 = wasmModule.PdfFont.Create({fontFamily: fontFamilies[i], size: 14});

            //Measure the width of text
            let x2 = x1 + 10 + font1.MeasureString({text: text}).Width;

            //Draw text s,
            page.Canvas.DrawString({s: text, font: font1, brush: brush, x: x1, y: y});
            page.Canvas.DrawString({s: text, font: font2, brush: brush, x: x2, y: y});
        }

        //True type font - embedded
        let trueTypeFont = wasmModule.PdfFont.Create({
            fontFamily: wasmModule.PdfFontFamily.Helvetica,
            size: 15,
            style: wasmModule.PdfFontStyle.Bold
        });
        y = y + 26;
        page.Canvas.DrawString({s: "Font Family: Arial - Embedded", font: trueTypeFont, brush: brush, x: 40, y: y});

        //Right to left
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

        //Define the format of string
        let format = wasmModule.PdfStringFormat.Create({alignment: wasmModule.PdfTextAlignment.Right});
        format.RightToLeft = true;

        //Draw text  s, font, brush, layoutRectangle, format
        page.Canvas.DrawString({s: arabicText, font: ARIALUNIFont, brush: brush, layoutRectangle: rctg, format: format});

        //True type font - not embedded
        trueTypeFont = wasmModule.PdfFont.Create({
            fontFamily: wasmModule.PdfFontFamily.Helvetica,
            size: 14,
            style: wasmModule.PdfFontStyle.Italic
        });
        y = y + 26;
        page.Canvas.DrawString({s: "Font Family: Batang - Not Embedded", font: trueTypeFont, brush: brush, x: 40, y: y});

        //Cjk font
        y = y + 36;
        let cjkFont = wasmModule.PdfCjkStandardFont.Create({
            fontFamily: wasmModule.PdfCjkFontFamily.MonotypeHeiMedium,
            size: 14
        });
        page.Canvas.DrawString({s: "How to say 'Font' in Chinese? \u5B57\u4F53", font: cjkFont, brush: brush, x: 40, y: y});

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

        y = y + 36;
        cjkFont = wasmModule.PdfCjkStandardFont.Create({
            fontFamily: wasmModule.PdfCjkFontFamily.HanyangSystemsShinMyeongJoMedium,
            size: 14
        });
        page.Canvas.DrawString({s: "How to say 'Font' in Korean? \uAE00\uAF34", font: cjkFont, brush: brush, x: 40, y: y});

        // Define the output file name
        const outputFileName = "Font_out.pdf";

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
