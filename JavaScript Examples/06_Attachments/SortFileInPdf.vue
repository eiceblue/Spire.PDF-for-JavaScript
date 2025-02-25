<template>
  <span>The following example demonstrates how to sort PDF files.</span>
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
        // Load the sample file into the virtual file system (VFS)
        let inputFileName1 = "ReplaceImage.pdf";
        await wasmModule.FetchFileToVFS(inputFileName1, "", `${import.meta.env.BASE_URL}static/data/`);
        let inputFileName2 = "SampleB_2.pdf";
        await wasmModule.FetchFileToVFS(inputFileName2, "", `${import.meta.env.BASE_URL}static/data/`);
        let inputFileName3 = "SampleB_3.pdf";
        await wasmModule.FetchFileToVFS(inputFileName3, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a new PdfDocument object
        let doc = wasmModule.PdfDocument.Create();
        //Add a custom field with the name "No", the internal name "number", and the type NumberField to the document's collection
        doc.Collection.AddCustomField("No", "number", wasmModule.CustomFieldType.NumberField);

        //Add a file-related field with the name "Desc", the internal name "desc", and the type Desc to the document's collection
        doc.Collection.AddFileRelatedField("Desc", "desc", wasmModule.FileRelatedFieldType.Desc);

        //Sort the document's collection based on the fields "No" and "Desc" in ascending order
        let fieldNames = ["No", "Desc"];
        let order = [true, true];
        doc.Collection.Sort(fieldNames, order);

        //Create a PdfAttachment object
        let pdfAttachment = wasmModule.PdfAttachment.Create({ fileName: inputFileName1 });

        //Add the PdfAttachment object to the document's collection
        doc.Collection.AddAttachment(pdfAttachment);

        //Create a PdfAttachment object 
        pdfAttachment = wasmModule.PdfAttachment.Create({ fileName: inputFileName2 });

        //Add the PdfAttachment object to the document's collection
        doc.Collection.AddAttachment(pdfAttachment);

        //Create a PdfAttachment object 
        pdfAttachment = wasmModule.PdfAttachment.Create({ fileName: inputFileName3 });

        //Add the PdfAttachment object to the document's collection
        doc.Collection.AddAttachment(pdfAttachment);

        //Iterate through each PdfAttachment object in the document's associated files
        for (let i = 0; i < doc.Collection.AssociatedFiles.length;) {

          let attachment = doc.Collection.AssociatedFiles[i];
          let embeddedFileSpecification = new wasmModule.PdfEmbeddedFileSpecification(attachment.H);

          //Set the value of the "No" field in the attachment to the current counter value
          embeddedFileSpecification.SetFieldValue({ fieldName: "No", intFieldValue: i+1 });

          //Set the value of the "Desc" field in the attachment to the file name of the attachment
          embeddedFileSpecification.SetFieldValue({ fieldName: "Desc", strFieldValue: attachment.FileName });

          //Increment the counter variable
          i++;
        }

        // Define the output file name
        const outputFileName = "SortFileInPdf_out.pdf";

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
