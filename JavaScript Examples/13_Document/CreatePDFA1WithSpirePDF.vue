<template>
  <span
    >The following example demonstrates how to convert PDF document to Pdf_A1B/Pdf_A1A/Pdf_A2A/Pdf_A2B/Pdf_A3A/Pdf_A3B/Pdf_X1A2001 document.</span
  >
  <el-button @click="startProcessing">Start</el-button>
  <a v-if="downloadUrlA1B" :href="downloadUrlA1B" :download="downloadNameA1B">
    Click here to download the generated A1B file
  </a>
  <a v-if="downloadUrlA1A" :href="downloadUrlA1A" :download="downloadNameA1A">
    Click here to download the generated A1A file
  </a>
  <a v-if="downloadUrlA2A" :href="downloadUrlA2A" :download="downloadNameA2A">
    Click here to download the generated A2A file
  </a>
  <a v-if="downloadUrlA2B" :href="downloadUrlA2B" :download="downloadNameA2B">
    Click here to download the generated A2B file
  </a>
  <a v-if="downloadUrlA3A" :href="downloadUrlA3A" :download="downloadNameA3A">
    Click here to download the generated A3A file
  </a>
  <a v-if="downloadUrlA3B" :href="downloadUrlA3B" :download="downloadNameA3B">
    Click here to download the generated A3B file
  </a>
  <a v-if="downloadUrlX1A2001" :href="downloadUrlX1A2001" :download="downloadNameX1A2001">
    Click here to download the generated X1A2001 file
  </a>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    const downloadUrlA1B = ref(null);
    const downloadNameA1B = ref("");

    const downloadUrlA1A = ref(null);
    const downloadNameA1A = ref("");

    const downloadUrlA2A = ref(null);
    const downloadNameA2A = ref("");

    const downloadUrlA2B = ref(null);
    const downloadNameA2B = ref("");

    const downloadUrlA3A = ref(null);
    const downloadNameA3A = ref("");

    const downloadUrlA3B = ref(null);
    const downloadNameA3B = ref("");

    const downloadUrlX1A2001 = ref(null);
    const downloadNameX1A2001 = ref("");
function DrawPage(page) {
      let pageWidth = page.Canvas.ClientSize.Width;
      let y = 0;

      //Title
      y = y + 5;
      let brush2 = wasmModule.PdfSolidBrush.Create({
        color: wasmModule.Color.get_Black(),
      });

      let font2 = wasmModule.PdfTrueTypeFont.Create({
        fontName: "Arial",
        emSize: 16,
        style: wasmModule.PdfFontStyle.Bold,
        unicode: true,
      });

      let format2 = wasmModule.PdfStringFormat.Create({
        alignment: wasmModule.PdfTextAlignment.Center,
      });
      format2.CharacterSpacing = 1;
      let text = "Summary of Science";
      page.Canvas.DrawString({
        s: text,
        font: font2,
        brush: brush2,
        x: pageWidth / 2,
        y: y,
        format: format2,
      });
      let size = font2.MeasureString({ text: text, format: format2 });
      y = y + size.Height + 6;

      //Icon
      let image = wasmModule.PdfImage.FromFile("Wikipedia_Science.png");

      page.Canvas.DrawImage({
        image: image,
        point: wasmModule.PointF.Create({
          x: pageWidth - image.PhysicalDimension.Width,
          y: y,
        }),
      });
      let imageLeftSpace = pageWidth - image.PhysicalDimension.Width - 2;
      let imageBottom = image.PhysicalDimension.Height + y;

      let font3 = wasmModule.PdfTrueTypeFont.Create({
        fontName: "Arial",
        emSize: 9,
        style: wasmModule.PdfFontStyle.Regular,
        unicode: true,
      });

      let format3 = new wasmModule.PdfStringFormat.Create();
      format3.ParagraphIndent = font3.Size * 2;
      format3.MeasureTrailingSpaces = true;
      format3.LineSpacing = font3.Size * 1.5;
      let text1 = "(All text and picture from ";
      let text2 = "Wikipedia";
      let text3 = ", the free encyclopedia)";
      page.Canvas.DrawString({
        s: text1,
        font: font3,
        brush: brush2,
        x: 0,
        y: y,
        format: format3,
      });

      size = font3.MeasureString({ text: text1, format: format3 });
      let x1 = size.Width;
      format3.ParagraphIndent = 0;

      let font4 = wasmModule.PdfTrueTypeFont.Create({
        fontName: "Arial",
        emSize: 9,
        style: wasmModule.PdfFontStyle.Underline,
        unicode: true,
      });
      let brush3 = wasmModule.PdfBrushes.get_Blue();
      page.Canvas.DrawString({
        s: text2,
        font: font4,
        brush: brush3,
        x: x1,
        y: y,
        format: format3,
      });
      size = font4.MeasureString({ text: text2, format: format3 });
      x1 = x1 + size.Width;

      page.Canvas.DrawString({
        s: text3,
        font: font3,
        brush: brush2,
        x: x1,
        y: y,
        format: format3,
      });
      y = y + size.Height;

      //Content
      let format4 = wasmModule.PdfStringFormat.Create();
      const decoder = new TextDecoder('utf-8');
      text =decoder.decode(FS.readFile("Summary_of_Science.txt"));
      console.log(text)
      let font5 = wasmModule.PdfTrueTypeFont.Create({
        fontName: "Arial",
        emSize: 10,
        style: wasmModule.PdfFontStyle.Regular,
        unicode: true,
      });

      format4.LineSpacing = font5.Size * 1.5;
      let textLayouter = wasmModule.PdfStringLayouter.Create();
      let imageLeftBlockHeight = imageBottom - y;
      let result = textLayouter.Layout(
        text,
        font5,
        format4,
        wasmModule.SizeF.Create({
          width: imageLeftSpace,
          height: imageLeftBlockHeight,
        })
      );
      if (result.ActualSize.Height < imageBottom - y) {
        imageLeftBlockHeight = imageLeftBlockHeight + result.LineHeight;
        result = textLayouter.Layout(
          text,
          font5,
          format4,
          wasmModule.SizeF.Create({
            width: imageLeftSpace,
            height: imageLeftBlockHeight,
          })
        );
      }
      //foreach (LineInfo line in result.Lines)
      for (let i = 0; i < result.Lines.length; i++) {
        let line = result.Lines[i];
        page.Canvas.DrawString({
          s: line.Text,
          font: font5,
          brush: brush2,
          x: 0,
          y: y,
          format: format4,
        });
        y = y + result.LineHeight;
      }

      let textWidget = wasmModule.PdfTextWidget.Create({
        text: result.Remainder,
        font: font5,
        brush: brush2,
      });
      let textLayout = wasmModule.PdfTextLayout.Create();

      textLayout.Break = wasmModule.PdfLayoutBreakType.FitPage;
      textLayout.Layout = wasmModule.PdfLayoutType.Paginate;
      let point1 = wasmModule.PointF.Create({ x: 0, y: y });
      let bounds = wasmModule.RectangleF.Create({
        location: point1,
        size: page.Canvas.ClientSize,
      });
      textWidget.StringFormat = format4;
      textWidget.Draw({
        page: page,
        layoutRectangle: bounds,
        format: textLayout,
      });
    }
    
    // to A1B
    function CreatePDFA1WithwasmModule_A1B() {
      // Create a new PDF document
      let doc = wasmModule.PdfDocument.Create();
      let page = doc.Pages.Add({
        size: wasmModule.PdfPageSize.A4(),
        margins: wasmModule.PdfMargins.Create({ margin: 40 }),
      });
      DrawPage(page);
      const outFileNameTemp = "CreatePDFA1WithwasmModule_Temp.pdf";
      doc.SaveToFile({ fileName: outFileNameTemp });
      doc.Close();

      const outFileName = "CreatePDFA1WithwasmModule_A1B.pdf";

      let converter = wasmModule.PdfStandardsConverter.Create({
        filePath: outFileNameTemp,
      });
      converter.ToPdfA1B({ filePath: outFileName });
      // Read the saved file and convert to a Blob object
      const modifiedFileArray = wasmModule.FS.readFile(outFileName);
      const modifiedFile = new Blob([modifiedFileArray], {
        type: "application/pdf",
      });

      // Download the file
      downloadNameA1B.value = outFileName;
      downloadUrlA1B.value = URL.createObjectURL(modifiedFile);
    }
    // to A1A
function CreatePDFA1WithwasmModule_A1A() {
   
    const inputFileName = "CreatePDFA1WithwasmModule_Temp.pdf";

    const outFileName = "CreatePDFA1WithwasmModule_A1A.pdf";
    let converter = wasmModule.PdfStandardsConverter.Create({filePath: inputFileName});
    // Convert the input PDF file to PDFA-1A format
    converter.ToPdfA1A({filePath: outFileName});
 
      // Read the saved file and convert to a Blob object
      const modifiedFileArray = wasmModule.FS.readFile(outFileName);
      const modifiedFile = new Blob([modifiedFileArray], {
        type: "application/pdf",
      });

      // Download the file
      downloadNameA1A.value = outFileName;
      downloadUrlA1A.value = URL.createObjectURL(modifiedFile);

}
    // to A2A
    function CreatePDFA1WithwasmModule_A2A() {
      const inputFileName = "CreatePDFA1WithwasmModule_Temp.pdf";

      const outFileName = "CreatePDFA1WithwasmModule_A2A.pdf";

      let converter = wasmModule.PdfStandardsConverter.Create({
        filePath: inputFileName,
      });
      // Convert the input PDF file to PDFA-2A format
      converter.ToPdfA2A({ filePath: outFileName });

      // Read the saved file and convert to a Blob object
      const modifiedFileArray = wasmModule.FS.readFile(outFileName);
      const modifiedFile = new Blob([modifiedFileArray], {
        type: "application/pdf",
      });

      // Download the file
      downloadNameA2A.value = outFileName;
      downloadUrlA2A.value = URL.createObjectURL(modifiedFile);
    }
    // to A2B
    function CreatePDFA1WithwasmModule_A2B() {
      const inputFileName = "CreatePDFA1WithwasmModule_Temp.pdf";

      const outFileName = "CreatePDFA1WithwasmModule_A2B.pdf";
      let converter = wasmModule.PdfStandardsConverter.Create({
        filePath: inputFileName,
      });
      // Convert the input PDF file to PDFA-2B format
      converter.ToPdfA2B({ filePath: outFileName });

      // Read the saved file and convert to a Blob object
      const modifiedFileArray = wasmModule.FS.readFile(outFileName);
      const modifiedFile = new Blob([modifiedFileArray], {
        type: "application/pdf",
      });

      // Download the file
      downloadNameA2B.value = outFileName;
      downloadUrlA2B.value = URL.createObjectURL(modifiedFile);
    }

    // to A3A
    function CreatePDFA1WithwasmModule_A3A() {
      const inputFileName = "CreatePDFA1WithwasmModule_Temp.pdf";

      const outFileName = "CreatePDFA1WithwasmModule_A3A.pdf";

      let converter = wasmModule.PdfStandardsConverter.Create({
        filePath: inputFileName,
      });
      // Convert the input PDF file to PDFA-3A format
      converter.ToPdfA3A({ filePath: outFileName });

      // Read the saved file and convert to a Blob object
      const modifiedFileArray = wasmModule.FS.readFile(outFileName);
      const modifiedFile = new Blob([modifiedFileArray], {
        type: "application/pdf",
      });

      // Download the file
      downloadNameA3A.value = outFileName;
      downloadUrlA3A.value = URL.createObjectURL(modifiedFile);
    }

    // to A3B
    function CreatePDFA1WithwasmModule_A3B() {
      const inputFileName = "CreatePDFA1WithwasmModule_Temp.pdf";

      const outFileName = "CreatePDFA1WithwasmModule_A3B.pdf";
      let converter = wasmModule.PdfStandardsConverter.Create({
        filePath: inputFileName,
      });
      // Convert the input PDF file to PDFA-3B format
      converter.ToPdfA3B({ filePath: outFileName });

      // Read the saved file and convert to a Blob object
      const modifiedFileArray = wasmModule.FS.readFile(outFileName);
      const modifiedFile = new Blob([modifiedFileArray], {
        type: "application/pdf",
      });

      // Download the file
      downloadNameA3B.value = outFileName;
      downloadUrlA3B.value = URL.createObjectURL(modifiedFile);
    }

    // to X1A2001
    function CreatePDFA1WithwasmModule_X1A2001() {
      const inputFileName = "CreatePDFA1WithwasmModule_Temp.pdf";

      const outFileName = "CreatePDFA1WithwasmModule_X1A2001.pdf";
      let converter = wasmModule.PdfStandardsConverter.Create({
        filePath: inputFileName,
      });
      // Convert the input PDF file to PDF/X-1a:2001 format
      converter.ToPdfX1A2001({ filePath: outFileName });

      // Read the saved file and convert to a Blob object
      const modifiedFileArray = wasmModule.FS.readFile(outFileName);
      const modifiedFile = new Blob([modifiedFileArray], {
        type: "application/pdf",
      });

      // Download the file
      downloadNameX1A2001.value = outFileName;
      downloadUrlX1A2001.value = URL.createObjectURL(modifiedFile);
    }
    const startProcessing = async () => {
      wasmModule = window.wasmModule;
      if (wasmModule) {
        // Load the ARIALUNI.TTF font file into the virtual file system (VFS)
        await wasmModule.FetchFileToVFS(
          "ARIALUNI.TTF",
          "/Library/Fonts/",
          `${import.meta.env.BASE_URL}static/font/`
        );

        let inputImageName1 = "Wikipedia_Science.png";
        await wasmModule.FetchFileToVFS(
          inputImageName1,
          "",
          `${import.meta.env.BASE_URL}static/data/`
        );

        let ScienceTxt = "Summary_of_Science.txt";
        await wasmModule.FetchFileToVFS(
          ScienceTxt,
          "",
          `${import.meta.env.BASE_URL}static/data/`
        );

        CreatePDFA1WithwasmModule_A1B();
        CreatePDFA1WithwasmModule_A1A();
        CreatePDFA1WithwasmModule_A2A();
        CreatePDFA1WithwasmModule_A2B();
        CreatePDFA1WithwasmModule_A3A();
        CreatePDFA1WithwasmModule_A3B();
        CreatePDFA1WithwasmModule_X1A2001(); 
      }
    };

    return {
      startProcessing,
      downloadUrlA1B,
      downloadNameA1B,

      downloadUrlA1A,
      downloadNameA1A,

      downloadUrlA2A,
      downloadNameA2A,

      downloadUrlA2B,
      downloadNameA2B,

      downloadUrlA3A,
      downloadNameA3A,

      downloadUrlA3B,
      downloadNameA3B,

      downloadUrlX1A2001,
      downloadNameX1A2001,
    };
  },
};
</script>
