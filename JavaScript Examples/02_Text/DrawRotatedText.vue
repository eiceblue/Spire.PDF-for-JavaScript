<template>
  <span>The sample demonstrates how to draw rotated text.</span>
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

        //Draw text in rotated Canvas  s, font, brush, point
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

        // Define the output file name
        const outputFileName = "DrawRotatedText_out.pdf";

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
