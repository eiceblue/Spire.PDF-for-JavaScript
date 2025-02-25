# Spire.PDF-for-JavaScript

A robust PDF library empowering developers to create, edit, convert, and process PDF documents seamlessly in JavaScript

[![Foo](https://i.imgur.com/6JRTxql.png)](https://www.e-iceblue.com/Introduce/pdf-for-javascript.html)

[Product Page](https://www.e-iceblue.com/Introduce/pdf-for-javascript.html) | Documentation | Examples | [Forum](https://www.e-iceblue.com/forum/spire-pdf-f7.html) | [Temporary License](https://www.e-iceblue.com/TemLicense.html) | [Customized Demo](https://www.e-iceblue.com/Misc/customized-demo.html)

[Spire.PDF for JavaScript](https://www.e-iceblue.com/Introduce/pdf-for-javascript.html) is a leading PDF library designed for developers to effortlessly create, read, edit, and convert PDF documents. With this powerful JavaScript PDF component, you can build PDFs from scratch or seamlessly process existing PDF files, making it an essential tool for JavaScript applications.

This versatile library offers an extensive range of functionalities, including setting security options, extracting text and images, merging and splitting PDFs, drawing text, images, shapes, and barcodes, creating and filling form fields, managing bookmarks, inserting watermarks (text or image), adding annotations, working with layers, and compressing PDF documents. Its comprehensive features enable developers to handle various PDF processing tasks efficiently.

Additionally, Spire.PDF for JavaScript supports advanced presentation capabilities. Developers can create, edit, and customize PowerPoint presentations directly in JavaScript. This includes handling text, images, shapes, tables, animations, audio, and video on slides. The library also enables exporting slides to formats such as PNG, JPG, TIFF, EMF, SVG, PDF, XPS, and HTML, making it ideal for creating dynamic and interactive presentations.

The library excels in format conversion, allowing seamless conversion of PDFs to Word (Doc/Docx), PDF to Excel, PDF to PowerPoint, PDF to images, PDF to HTML, PDF to Markdown, PDF to XPS, PDF to SVG, PDF to TIFF, PDF to PCL, PDF to PostScript, and PDF to PDF/A. Additionally, it supports high-quality conversions from text, images, TIFF, XPS, SVG, and HTML into PDF format. This makes Spire.PDF for JavaScript a one-stop solution for developers working with document management and format conversions.

### Independent Spire.PDF for JavaScript - No Adobe Acrobat Needed
Spire.PDF for JavaScript is a fully independent JavaScript PDF library, requiring no reliance on Adobe Acrobat or any other third-party software or libraries. It operates seamlessly on its own, simplifying your development process.

### Powerful Features to Simplify PDF Manipulation
Spire.PDF for JavaScript streamlines PDF handling with robust capabilities. It enables you to create and manipulate text, images, tables, barcodes, and shapes, extract content effortlessly, and manage form fields with ease. Additionally, the library supports adding watermarks, bookmarks, hyperlinks, annotations, attachments, and stamping PDFs with text or images.

### Rich Document Settings Features
Using Spire.PDF for JavaScript, you can efficiently handle document details, such as defining properties and customizing viewer settings like zoom levels. Additionally, the library enables page-related operations, including counting pages, altering page dimensions, retrieving size information, and adjusting orientation or rotation.

### High-Quality PDF File Conversion
Spire.PDF for JavaScript provides an efficient solution for converting PDFs into multiple formats, such as Word (DOC/DOCX), Excel, PowerPoint, HTML, Images (TIFF, JPEG, PNG), XPS, SVG, PostScript, and PDF/A. It also supports seamless, high-quality conversions from HTML, Images, Text, XPS, and SVG back to PDF.

### Multiple Security Features for PDFs
Spire.PDF for JavaScript empowers developers to manage digital signatures effortlessly, including adding, removing, verifying, and extracting them from PDF files. Additionally, it supports encrypting and decrypting PDFs, configuring security permissions, and detecting modifications in signed documents.

### Easy Integration
Spire.PDF for JavaScript can be easily integrated into JavaScript applications.
Developers can easily integrate Spire.PDF for JavaScript into their JavaScript applications.

## Vue Examples

### Delete All Bookmarks from PDF Document in JavaScript
```JavaScript
<template>
  <span>The following example demonstrates how to delete all bookmarks from PDF document.</span>
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
        let inputFileName = "Template_Pdf_1.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a new PdfDocument object
        let doc = wasmModule.PdfDocument.Create();

        // Load an existing PDF document from a file.
        doc.LoadFromFile({ fileName: inputFileName });

        //Get all bookmarks
        let bookmarks = doc.Bookmarks;

        //Remove
        bookmarks.Clear();

        // Define the output file name
        const outputFileName = "DeleteAllBookmarks_out.pdf";

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
```

### Sort PDF Files in JavaScript
```JavaScript
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
```

[Product Page](https://www.e-iceblue.com/Introduce/pdf-for-javascript.html) | Documentation | Examples | [Forum]((https://www.e-iceblue.com/forum/spire-pdf-f7.html) | [Temporary License](https://www.e-iceblue.com/TemLicense.html) | [Customized Demo](https://www.e-iceblue.com/Misc/customized-demo.html)
