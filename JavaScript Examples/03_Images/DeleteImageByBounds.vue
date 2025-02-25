<template>
  <span>The following example shows how to delete the images by their bounds.</span>
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

        let inputFileName  = "DeleteImageByBounds.pdf";
        await wasmModule.FetchFileToVFS(inputFileName , '', `${import.meta.env.BASE_URL}static/data/`);

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

        // Define the output file name
        const outputFileName = "DeleteImageByBounds_out.pdf";

        // Save the document to the specified path
        pdf.SaveToFile({fileName: outputFileName});
        pdf.Close();

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
