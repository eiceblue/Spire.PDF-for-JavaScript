<template>
  <div class="app-description">
    <h1>Welcome to the Vue Demo</h1>
    <p>
      We are excited to introduce our Vue application, showcasing the powerful capabilities of Spire.PDF for JavaScript in handling PDF files. 
      This project seamlessly integrates the functionalities of Spire.PDF for JavaScript through modern web application frameworks. 
    </p>
    <p class="title">Getting Start</p>
    <ol>
      <li>Copy the two files (<span class="getstart">Spire.Pdf.Base.js & Spire.Pdf.Base.wasm</span>) from <span class="getstart">lib</span> directory to the <span class="getstart">public</span> directory in Vue project.</li>
      <li>Expand Spire.PDF from the left menu, select a detailed feature and click the "Start" button to execute.</li>
    </ol>
    <p class="title">Product Introduction</p>
    <p>
      Spire.PDF for JavaScript is a powerful PDF development library that allows developers to easily create, read, edit, and convert PDFs within any JavaScript environment. 
      With Spire.PDF for JavaScript, developers can efficiently handle PDF programming tasks—whether creating PDF files from scratch, processing existing PDF documents, or performing file conversions.
    </p>
    <p class="title">Technical Architecture</p>
    <p>
      Built using WebAssembly technology, Spire.PDF for JavaScript is fully compatible with popular frameworks such as Vue, React, Angular, and plain JavaScript, 
      enabling developers to easily create and distribute their JavaScript applications on the web.
    </p>
    <p class="title">File Support</p>
    <p>
      This versatile JavaScript PDF API operates independently. It eliminates the need for Adobe Acrobat or any other third-party software or libraries to be installed on your system. 
      Spire.PDF for JavaScript offers a simple solution for converting PDFs to various formats, including Word (DOC/DOCX), Excel, PowerPoint, HTML, Images (TIFF, JPEG, PNG), XPS, SVG, PostScript, PDF/A, etc. Additionally, it supports high-quality conversion from HTML, Images, Text, XPS, and SVG to PDF.
    </p>
    <p class="title">Contact Us</p>
    <ul>
      <li>For any technical questions regarding the product, feel free to contact us: <a href="mailto:support@e-iceblue.com">support@e-iceblue.com</a>.</li>
      <li>For any questions about purchasing the product, please reach out to: <a href="mailto:sales@e-iceblue.com">sales@e-iceblue.com</a>.</li>
      <li>If you can't find the example you need, contact us for a free custom demo: <a href="mailto:support@e-iceblue.com">support@e-iceblue.com</a>.</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'AppDescription',
};
</script>

<style scoped>
.app-description {
  padding: 20px;
  font-family: Arial, sans-serif;
  text-align: left;
}
.title {
  text-align: left;
  font-weight: bold;
  color: #2980B9;
  font-size: 17px;
}

h1 {
  color: #1F618D;
}

p {
  line-height: 1.6;
}
.getstart
{
  color: #E67E22 ;
}
ul {
  list-style-type: none;
  padding: 0;
}
ol {
  list-style-type: style type num;;
  padding: 0;
}
b
{
  font-weight: bold
};
ul li{
  margin: 10px 0;
}
</style>