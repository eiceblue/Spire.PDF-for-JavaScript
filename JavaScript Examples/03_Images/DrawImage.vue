<template>
  <span>The following example demonstrates how to draw image in PDF document.</span>
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
        // Load an image from file
        const inputFileName = "ChartImage.png";
        await wasmModule.FetchFileToVFS(inputFileName , '', `${import.meta.env.BASE_URL}static/data/`);

        // Create a pdf instance
        let doc = wasmModule.PdfDocument.Create();

        //Create one page
        let page = doc.Pages.Add();

        TransformText(page);
        DrawImageInPage(page,inputFileName);
        TransformImage(page,inputFileName);

        // Define the output file name
        const outputFileName = "DrawImage_out.pdf";

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
    };

    return {
      startProcessing,
      downloadName,
      downloadUrl,
    };
  },
};
</script>
