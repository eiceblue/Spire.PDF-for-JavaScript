<template>
  <span>The following example demonstrates how to add an attachment to a PDF document.</span>
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

        // Load the sample file into the virtual file system (VFS)
        let inputFileName = "Attachment.pdf";
        await wasmModule.FetchFileToVFS(inputFileName, "", `${import.meta.env.BASE_URL}static/data/`);

        // Create a PDF document
        let doc = wasmModule.PdfDocument.Create();

        // Load the PDF fle
        doc.LoadFromFile({ fileName: inputFileName });

        // Get the first page of the loaded document.
        let page = doc.Pages.get_Item(0);

        // Set the initial y-coordinate for positioning elements on the page
        let y = 320;

        // Add a title to the page
        let brush1 = wasmModule.PdfBrushes.get_CornflowerBlue();
        let font1 = wasmModule.PdfTrueTypeFont.Create({
            fontFile: "/Library/Fonts/ARIALUNI.TTF",
            size: 18,
            style: wasmModule.PdfFontStyle.Bold
        });
        let format1 = wasmModule.PdfStringFormat.Create({ alignment: wasmModule.PdfTextAlignment.Center });
        page.Canvas.DrawString({
          s: "Attachment",
          font: font1,
          brush: brush1,
          x: page.Canvas.ClientSize.Width / 2,
          y: y,
          format: format1
        });

        // Update the y-coordinate
        y = y + font1.MeasureString({ text: "Attachment", format: format1 }).Height;
        y = y + 10;

        // Add an attachment to the PDF document
        let attachment = wasmModule.PdfAttachment.Create({ fileName: "Header.png" });
        let inputFilePath = "Header.png";
        await wasmModule.FetchFileToVFS(inputFilePath, "", `${import.meta.env.BASE_URL}static/data/`);
        attachment.Data = wasmModule.FS.readFile(inputFilePath);
        attachment.Description = "Page header picture of demo.";
        attachment.MimeType = "image/png";
        doc.Attachments.Add({ attachment: attachment });

        // Add another attachment to the PDF document
        attachment = wasmModule.PdfAttachment.Create({ fileName: "Footer.png" });
        inputFilePath = "Footer.png";
        await wasmModule.FetchFileToVFS(inputFilePath, "", `${import.meta.env.BASE_URL}static/data/`);
        attachment.Data = wasmModule.FS.readFile(inputFilePath);
        attachment.Description = "Page footer picture of demo.";
        attachment.MimeType = "image/png";
        doc.Attachments.Add({ attachment: attachment });

        // Set the initial x-coordinate and font for the labels
        let x = 50;
        let font2 = wasmModule.PdfTrueTypeFont.Create({
            fontFile: "/Library/Fonts/ARIALUNI.TTF",
            size: 14,
            style: wasmModule.PdfFontStyle.Bold
        });

        // Add a label and annotation for the Sales Report Chart attachment
        let location = wasmModule.PointF.Create({ x: x, y: y });
        let label = "Sales Report Chart";
        inputFilePath = "SalesReportChart.png";
        await wasmModule.FetchFileToVFS(inputFilePath, "", `${import.meta.env.BASE_URL}static/data/`);
        let data = wasmModule.FS.readFile(inputFilePath);
        let size = font2.MeasureString({ text: label });
        let bounds = wasmModule.RectangleF.Create({ location: location, size: size });
        page.Canvas.DrawString({ s: label, font: font2, brush: wasmModule.PdfBrushes.get_DarkOrange(), layoutRectangle: bounds });
        bounds = wasmModule.RectangleF.Create({
          x: bounds.Right + 3,
          y: bounds.Top,
          width: font2.Height / 2,
          height: font2.Height
        });

        // Create a PdfAttachmentAnnotation for the Sales Report Chart attachment
        let annotation1 = wasmModule.PdfAttachmentAnnotation.Create({
          rectangle: bounds,
          fileName: "SalesReportChart.png",
          data: data
        });
        annotation1.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Teal() });
        annotation1.Flags = wasmModule.PdfAnnotationFlags.ReadOnly;
        annotation1.Icon = wasmModule.PdfAttachmentIcon.Graph;
        annotation1.Text = "Sales Report Chart";

        // Add the annotation to the page
        page.Annotations.Add(annotation1);

        // Update the y-coordinate
        y = y + size.Height + 3;

        // Repeat the above steps for the remaining attachments and annotations
        location = wasmModule.PointF.Create({ x: x, y: y });
        label = "Science Personification Boston";
        inputFilePath = "SciencePersonificationBoston.jpg";
        await wasmModule.FetchFileToVFS(inputFilePath, "", `${import.meta.env.BASE_URL}static/data/`);
        data = wasmModule.FS.readFile(inputFilePath);
        size = font2.MeasureString({ text: label });
        bounds = wasmModule.RectangleF.Create({ location: location, size: size });
        page.Canvas.DrawString({
          s: label,
          font: font2,
          brush: wasmModule.PdfBrushes.get_DarkOrange(),
          layoutRectangle: bounds
        });

        bounds = wasmModule.RectangleF.Create({
          x: bounds.Right + 3,
          y: bounds.Top,
          width: font2.Height / 2,
          height: font2.Height
        });

        let annotation2 = wasmModule.PdfAttachmentAnnotation.Create({
          rectangle: bounds,
          fileName: "SciencePersonificationBoston.jpg",
          data: data
        });
        annotation2.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_Orange() });
        annotation2.Flags = wasmModule.PdfAnnotationFlags.NoZoom;
        annotation2.Icon = wasmModule.PdfAttachmentIcon.PushPin;
        annotation2.Text = "SciencePersonificationBoston.jpg, from Wikipedia, the free encyclopedia";
        page.Annotations.Add(annotation2);
        y = y + size.Height + 2;

        location = wasmModule.PointF.Create({ x: x, y: y });
        label = "Picture of Science";
        inputFilePath =  "Wikipedia_Science.png";
        await wasmModule.FetchFileToVFS(inputFilePath, "", `${import.meta.env.BASE_URL}static/data/`);
        data = wasmModule.FS.readFile(inputFilePath);
        size = font2.MeasureString({ text: label });
        bounds = wasmModule.RectangleF.Create({ location: location, size: size });
        page.Canvas.DrawString({ s: label, font: font2, brush: wasmModule.PdfBrushes.get_DarkOrange(), layoutRectangle: bounds });
        bounds = wasmModule.RectangleF.Create({
          x: bounds.Right + 3,
          y: bounds.Top,
          width: font2.Height / 2,
          height: font2.Height
        });

        let annotation3 = wasmModule.PdfAttachmentAnnotation.Create({
          rectangle: bounds,
          fileName: "Wikipedia_Science.png",
          data: data
        });
        annotation3.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_SaddleBrown() });
        annotation3.Flags = wasmModule.PdfAnnotationFlags.Locked;
        annotation3.Icon = wasmModule.PdfAttachmentIcon.Tag;
        annotation3.Text = "Wikipedia_Science.png, from Wikipedia, the free encyclopedia";
        page.Annotations.Add(annotation3);
        y = y + size.Height + 2;

        location = wasmModule.PointF.Create({ x: x, y: y });
        label = "PT_Serif-Caption-Web-Regular Font";
        inputFilePath = "PT_Serif-Caption-Web-Regular.ttf";
        await wasmModule.FetchFileToVFS(inputFilePath, "", `${import.meta.env.BASE_URL}static/data/`);
        data = wasmModule.FS.readFile(inputFilePath);
        size = font2.MeasureString({ text: label });
        bounds = wasmModule.RectangleF.Create({ location: location, size: size });
        page.Canvas.DrawString({ s: label, font: font2, brush: wasmModule.PdfBrushes.get_DarkOrange(), layoutRectangle: bounds });
        bounds = wasmModule.RectangleF.Create({
          x: bounds.Right + 3,
          y: bounds.Top,
          width: font2.Height / 2,
          height: font2.Height
        });

        let annotation4 = wasmModule.PdfAttachmentAnnotation.Create({
          rectangle: bounds,
          fileName: "PT_Serif-Caption-Web-Regular.ttf",
          data: data
        });

        annotation4.Color = wasmModule.PdfRGBColor.Create({ color: wasmModule.Color.get_CadetBlue() });
        annotation4.Flags = wasmModule.PdfAnnotationFlags.NoRotate;
        annotation4.Icon = wasmModule.PdfAttachmentIcon.Paperclip;
        annotation4.Text = "PT_Serif-Caption-Web-Regular, from http://www.1001freefonts.com";
        page.Annotations.Add(annotation4);

        // Define the output file name
        const outputFileName = "Attachment_out.pdf";

        // Save the document to the specified path
        doc.SaveToFile({ fileName: outputFileName });

        // Clean up resources
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
