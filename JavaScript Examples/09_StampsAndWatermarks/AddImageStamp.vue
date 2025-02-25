<template>
  <span>The following example demonstrates how to add image stamp in PDF document</span>
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
        const inputFileName = "AddImageStamp.pdf";
        // Load the input file into the virtual file system (VFS)
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);
        const inputImageName1 = "image stamp.jpg";
        await wasmModule.FetchFileToVFS(inputImageName1, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a new PDF document object
        let doc = wasmModule.PdfDocument.Create();
        // Load an existing PDF document from VFS
        doc.LoadFromFile({ fileName: inputFileName });

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

        // Define the output file name
        const outputFileName = "AddImageStamp.pdf";

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
