<template>
  <span>The following example demonstrates how to add tooltip to text in PDF document. </span>
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

        // Define the output file name
        const outputFileName = "AddTooltipToText_out.pdf";

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
