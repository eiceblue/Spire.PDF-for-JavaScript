<template>
  <span>The following example demonstrates how to extract 3D Viedo file from Pdf3DAnnotation.</span>
  <el-button @click="startProcessing">Start</el-button>
  <a v-if="downloadUrl" :href="downloadUrl" :download="downloadName">
    Click here to download the generated file
  </a>
</template>

<script>
import { ref } from "vue";
import JSZip from "jszip";

export default {
  setup() {
    const downloadUrl = ref(null);
    const downloadName = ref("");

    const startProcessing = async () => {
      wasmModule = window.wasmModule;
      if (wasmModule) {
        // Load the sample file into the virtual file system (VFS)
        let inputFileName = "3D.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        // Load the PDF fle
        doc.LoadFromFile({ fileName: inputFileName });

        // Get the first page
        let firstPage = doc.Pages.get_Item(0);

        // Get the annotation collection of the first page
        let annot = firstPage.Annotations;

        // Define an int variable
        let count = 0;

        let outputDirectoryName = "outputFiles/";
        FS.mkdirTree(outputDirectoryName);

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

        const zip = new JSZip();
        let items = await FS.readdir(outputDirectoryName);
        items = items.filter((item) => item !== "." && item !== "..");
        for (const item of items) {
          const itemPath = `${outputDirectoryName}/${item}`;
          const fileData = await FS.readFile(itemPath);
          zip.file(item, fileData);
        }

        const zipBlob = await zip.generateAsync({ type: "blob" });
        const zipDownloadUrl = URL.createObjectURL(zipBlob);
        const zipDownloadName = `outputFiles.zip`;
        downloadName.value = zipDownloadName;
        downloadUrl.value = zipDownloadUrl;
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
