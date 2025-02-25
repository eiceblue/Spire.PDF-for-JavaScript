<template>
  <span>The sample demonstrates how to add Table Of Content in PDF document</span>
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

        // Load the input file into the virtual file system (VFS)
        const inputFileName = "AddTableOfContent.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a new PDF document object
        let doc = wasmModule.PdfDocument.Create();

        // Load an existing PDF document from VFS
        doc.LoadFromFile({ fileName: inputFileName });

        // Get the total number of pages in the document.
        let pageCount = doc.Pages.Count;

        // Insert a blank page at the beginning of the PDF document.
        let tocPage = doc.Pages.Insert({ index: 0 });

        // Set the title for the table of contents.
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

        // Draw the table of contents entries.
        let trueTypeFont2 = wasmModule.PdfTrueTypeFont.Create({
            fontFile: "ARIALUNI.TTF",
            size: 14,
            style: spirepdf.PdfFontStyle.Regular
        });
        let titlesFont = trueTypeFont2;
        var titles = new Array();
        for (let i = 0; i < pageCount; i++) {
          titles.push("This is page" + (i + 1));
        }

        let y = titleFont.MeasureString({ text: title }).Height + 10;
        let x = 0;

        for (let i = 1; i <= pageCount; i++) {
          let text = titles[i - 1];
          let titleSize = titlesFont.MeasureString({ text: text });

          // Get the page that the table of contents entry will navigate to.
          let navigatedPage = doc.Pages.get_Item(i);

          let pageNumText = (i + 1).toString();
          let pageNumTextSize = titlesFont.MeasureString({ text: pageNumText });

          // Draw the entry text.
          tocPage.Canvas.DrawString({ s: text, font: titlesFont, brush: spirepdf.PdfBrushes.get_CadetBlue(), x: 0, y: y });

          let dotLocation = titleSize.Width + 2 + x;
          let pageNumlocation = tocPage.Canvas.ClientSize.Width - pageNumTextSize.Width;

          // Draw dots between the entry text and page number.
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

          // Draw the page number.
          tocPage.Canvas.DrawString({
            s: pageNumText,
            font: titlesFont,
            brush: spirepdf.PdfBrushes.get_CadetBlue(),
            x: pageNumlocation,
            y: y
          });

          // Add the table of contents action.
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





        // Define the output file name
        const outputFileName = "AddTableOfContent.pdf";

        // Save the document to the specified path
        doc.SaveToFile({ fileName: outputFileName });
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
