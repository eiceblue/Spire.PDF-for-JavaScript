<template>
  <span>The following example demonstrates how to draw text in a PDF document.</span>
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

        // Create a pdf instance
        let doc = wasmModule.PdfDocument.Create();

         // Create one page
        let page = doc.Pages.Add();
        
        //Draw the text - brush
        DrawTextInPage(page);

        // Draw the text - alignment
        AlignText(page);

        //Draw the text - align in rectangle
        AlignTextInRectangle(page);

        // Draw the text - transform
        TransformText(page);
        RotateText(page);

        // Define the output file name
        const outputFileName = "DrawText_out.pdf";

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
function AlignText(page) {
    let font = wasmModule.PdfFont.Create({fontFamily:wasmModule.PdfFontFamily.Helvetica,size:20});
    let brush = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_Blue()});

    //Draws left-aligned text
    let leftAlignment = wasmModule.PdfStringFormat.Create({alignment:wasmModule.PdfTextAlignment.Left, lineAlignment:wasmModule.PdfVerticalAlignment.Middle});
    page.Canvas.DrawString({s:"Left!", font:font, brush:brush, x:0, y:20, format:leftAlignment});
    page.Canvas.DrawString({s:"Left!", font:font, brush:brush, x:0, y:50, format:leftAlignment});

    //Draws right-aligned text
    let rightAlignment = wasmModule.PdfStringFormat.Create({alignment:wasmModule.PdfTextAlignment.Right, lineAlignment:wasmModule.PdfVerticalAlignment.Middle});
    page.Canvas.DrawString({s:"Right!", font:font, brush:brush,x: page.Canvas.ClientSize.Width, y:20, format:rightAlignment});
    page.Canvas.DrawString({s:"Right!", font:font, brush:brush,x: page.Canvas.ClientSize.Width, y:50, format:rightAlignment});

    //Draws center-aligned text
    let centerAlignment = wasmModule.PdfStringFormat.Create({alignment:wasmModule.PdfTextAlignment.Center, lineAlignment:wasmModule.PdfVerticalAlignment.Middle});
    page.Canvas.DrawString({s:"Go! Turn Around! Go! Go! Go!", font:font, brush:brush, x:page.Canvas.ClientSize.Width / 2,y: 40, format:centerAlignment});
}

function AlignTextInRectangle(page) {
    //Create the font to use and set the font style
    let font = wasmModule.PdfFont.Create({fontFamily:wasmModule.PdfFontFamily.Helvetica,size:10});
    let brush = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_Blue()});


    let rctg1 = new wasmModule.RectangleF.Create({x:0,y: 70,width: page.Canvas.ClientSize.Width / 2, height:100});
    let rctg2 = new wasmModule.RectangleF.Create({x:page.Canvas.ClientSize.Width / 2,y: 70, width:page.Canvas.ClientSize.Width / 2,height: 100});

    //Draw rectangle and specifies its fill color and position brush, rectangle
    let brush1 = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_LightBlue()});
    let brush2 = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_LightSkyBlue()});
    page.Canvas.DrawRectangle({brush:brush1, rectangle:rctg1});
    page.Canvas.DrawRectangle({brush:brush2, rectangle:rctg2});

    //Draws left-aligned text s, font, brush, layoutRectangle, format
    let leftAlignment = new wasmModule.PdfStringFormat.Create({alignment:wasmModule.PdfTextAlignment.Left, lineAlignment:wasmModule.PdfVerticalAlignment.Top});
    page.Canvas.DrawString({s:"Left! Left!", font:font, brush:brush,layoutRectangle: rctg1, format:leftAlignment});
    page.Canvas.DrawString({s:"Left! Left!", font:font, brush:brush,layoutRectangle: rctg2, format:leftAlignment});

    //Draws right-aligned text
    let rightAlignment = new wasmModule.PdfStringFormat.Create({alignment:wasmModule.PdfTextAlignment.Right, lineAlignment:wasmModule.PdfVerticalAlignment.Middle});
    page.Canvas.DrawString({s:"Right! Right!", font:font, brush:brush,layoutRectangle: rctg1, format:rightAlignment});
    page.Canvas.DrawString({s:"Right! Right!", font:font, brush:brush,layoutRectangle: rctg2, format:rightAlignment});

    //Draws center-aligned text
    let centerAlignment = new wasmModule.PdfStringFormat.Create({alignment:wasmModule.PdfTextAlignment.Center, lineAlignment:wasmModule.PdfVerticalAlignment.Bottom});
    page.Canvas.DrawString({s:"Go! Turn Around! Go! Go! Go!", font:font, brush:brush,layoutRectangle: rctg1, format:centerAlignment});
    page.Canvas.DrawString({s:"Go! Turn Around! Go! Go! Go!", font:font, brush:brush,layoutRectangle: rctg2, format:centerAlignment});
}

function RotateText(page) {
    //Save graphics state
    let state = page.Canvas.Save();

    //Draw the text - transform
    let font = wasmModule.PdfFont.Create({fontFamily:wasmModule.PdfFontFamily.Helvetica,size:10});
    let brush = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_Blue()});

    let centerAlignment = new wasmModule.PdfStringFormat.Create({alignment:wasmModule.PdfTextAlignment.Left, lineAlignment:wasmModule.PdfVerticalAlignment.Middle});
    let x = page.Canvas.ClientSize.Width / 2;
    let y = 380;

    page.Canvas.TranslateTransform(x, y);
    for (let i = 0; i < 12; i++)
    {
        //Rotate Canvas
        page.Canvas.RotateTransform({angle:30});
        //Draw text
        page.Canvas.DrawString({s:"Go! Turn Around! Go! Go! Go!", font:font, brush:brush, x:20,y: 0, format:centerAlignment});
    }

    //Restore graphics
    page.Canvas.Restore({state});
}

function TransformText(page) {
    //Save graphics state
    let state = page.Canvas.Save();

    //Draw the text - transform
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

    //Restore graphics
    page.Canvas.Restore({state:state});
}



function DrawTextInPage(page) {
    //Save graphics state
    let state = page.Canvas.Save();
    //Draw text - brush
    let text = "Go! Turn Around! Go! Go! Go!";
    let pen = wasmModule.PdfPens.get_DeepSkyBlue();
    let brush = wasmModule.PdfSolidBrush.Create({color: wasmModule.Color.get_White()});

    let format = wasmModule.PdfStringFormat.Create();//fontFamily, size, style
    let font = wasmModule.PdfFont.Create({fontFamily: wasmModule.PdfFontFamily.Helvetica, size: 18, style: wasmModule.PdfFontStyle.Italic});

    let size = font.MeasureString({text: text, format: format});

    let rctg = wasmModule.RectangleF.Create({x: page.Canvas.ClientSize.Width / 2 + 10, y: 180, width: size.Width / 3 * 2, height: size.Height * 2});

    page.Canvas.DrawString({s: text, font: font, pen: pen, brush: brush, layoutRectangle: rctg, format: format});
    //Restore graphics
    page.Canvas.Restore({state:state});
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
