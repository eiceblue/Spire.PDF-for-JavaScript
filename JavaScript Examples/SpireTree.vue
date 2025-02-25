<template>
  <div>
    <el-tree
      style="max-width: 600px"
      :data="data"
      :props="defaultProps"
      accordion
      @node-click="handleNodeClick"
    />
  </div>
</template>
<script setup lang="ts">
interface Tree {
  label: string;
  children?: Tree[];
  $treeNodeId?: string | number;
  [key: string]: any;
}
const emits = defineEmits(["item-id"]);
const handleNodeClick = (data: Tree) => {
  if ((data as any).id !== undefined) {
    emits("item-id", (data as any).id);
  }
};
const data: Tree[] = [
  {
    label: "Spire.PDF",
    children: [
      // 01_QuickGuide
      {
        label: "01_QuickGuide",
        children: [
          {
            label: "HelloWorld",
            id: "1-1",
          },
        ],
      },
       // 02_Text
       {
        label: "02_Text",
        children: [
          {
            label: "AddBorderForText",
            id: "2-1",
          },
          {
            label: "AddTooltipToText",
            id: "2-2",
          },
          {
            label: "AddTransparentText",
            id: "2-3",
          },
          {
            label: "DrawRotatedText",
            id: "2-4",
          },
          {
            label: "DrawText",
            id: "2-5",
          },
          {
            label: "DrawTextWithGradient",
            id: "2-6",
          },
          {
            label: "ExtractHighlightedText",
            id: "2-7",
          },
          {
            label: "ExtractTextFromParticularPage",
            id: "2-8",
          },
          {
            label: "ExtractTextFromSpecificArea",
            id: "2-9",
          },
          {
            label: "FindAndHighlightText",
            id: "2-10",
          },
          {
            label: "FindTextInDefineArea",
            id: "2-11",
          },
          {
            label: "Font",
            id: "2-12",
          },
          {
            label: "GetDetailsOfSearchedText",
            id: "2-13",
          },
          {
            label: "GetTextSizeBasedOnFont",
            id: "2-14",
          },
          {
            label: "ReplaceAllSearchedText",
            id: "2-15",
          },
          {
            label: "ReplaceFirstSearchedText",
            id: "2-16",
          },
          {
            label: "ReplaceFont",
            id: "2-17",
          },
          {
            label: "ReplaceTextSecond",
            id: "2-18",
          },
          {
            label: "SearchTextAndAddHyperlink",
            id: "2-19",
          },
          {
            label: "SearchTextAndDrawRectangle",
            id: "2-20",
          },
          {
            label: "SearchWithRegularExpression",
            id: "2-21",
          },
          {
            label: "SetLineBreak",
            id: "2-22",
          },
          {
            label: "SuperScriptAndSubScript",
            id: "2-23",
          },
          {
            label: "TextLayout",
            id: "2-24",
          },
          {
            label: "WrapTextAroundImage",
            id: "2-25",
          },
        ],
      },
    // 03_Images
    {
        label: "03_Images",
        children: [
          {
            label: "AddSVGToPDF",
            id: "3-1",
          },
          {
            label: "ConvertAllPagesToPNG",
            id: "3-2",
          },
          {
            label: "ConvertImageStreamToPDF",
            id: "3-3",
          },
          {
            label: "ConvertImageToPDF",
            id: "3-4",
          },
          {
            label: "DeleteImageByBounds",
            id: "3-5",
          },
          {
            label: "DeleteImageFirstApproach",
            id: "3-6",
          },
          {
            label: "DrawImage",
            id: "3-7",
          },
          {
            label: "PageToPNG",
            id: "3-8",
          },
          {
            label: "ReplaceImage",
            id: "3-9",
          },
          {
            label: "ReplaceImageWithText",
            id: "3-10",
          },
          {
            label: "SetImageSize",
            id: "3-11",
          },
        ],
      },

      // 04_Graphs
    {
        label: "04_Graphs",
        children: [
          {
            label: "Barcode",
            id: "4-1",
          },
          {
            label: "DrawContentWithSpotColor",
            id: "4-2",
          },
          {
            label: "DrawDashedLine",
            id: "4-3",
          },
          {
            label: "DrawFilledRectangles",
            id: "4-4",
          },
          {
            label: "DrawLine",
            id: "4-5",
          },
          {
            label: "DrawRectangles",
            id: "4-6",
          },
          {
            label: "DrawShape",
            id: "4-7",
          },
          {
            label: "DrawShapeWithGradientFill",
            id: "4-8",
          },
          {
            label: "List",
            id: "4-9",
          },
          {
            label: "Overlay",
            id: "4-10",
          },
          {
            label: "SetRectangleTransparency",
            id: "4-11",
          },
          {
            label: "SetSeparationColorSpace",
            id: "4-12",
          },
          {
            label: "Transparency",
            id: "4-13",
          },
        ],
      },
      // 05_Annotations
    {
        label: "05_Annotations",
        children: [
          {
            label: "AddFreeTextAnnotation",
            id: "5-1",
          },
          {
            label: "Annotation",
            id: "5-2",
          },
          {
            label: "CreatePdf3DAnnotation",
            id: "5-3",
          },
          {
            label: "CreatePdfLineAnnotation",
            id: "5-4",
          },
          {
            label: "CreatePdfLinkAnnotation",
            id: "5-5",
          },
          {
            label: "CreatePdfPolygonAnnotation",
            id: "5-6",
          },
          {
            label: "CreatePolylineAnnotation",
            id: "5-7",
          },
          {
            label: "DeleteAllAnnotations",
            id: "5-8",
          },
          {
            label: "DeleteAnnotation",
            id: "5-9",
          },
          {
            label: "Extract3DViedoFile",
            id: "5-10",
          },
          {
            label: "GetAllAnnotationsFromPage",
            id: "5-11",
          },
          {
            label: "GetParticularAnnotationInfo",
            id: "5-12",
          },
          {
            label: "InvisibleFreeTextAnnotation",
            id: "5-13",
          },
          {
            label: "ModifyLineAnnotation",
            id: "5-14",
          },
          {
            label: "SetFreeTextAnnotationAlignment",
            id: "5-15",
          },
          {
            label: "SetFreeTextAnnotationStyle",
            id: "5-16",
          },
          {
            label: "SetFreeTextAnnotationSubject",
            id: "5-17",
          },
          {
            label: "TextAnnotationProperties",
            id: "5-18",
          },
          {
            label: "UpdateFreeTextAnnotation",
            id: "5-19",
          },
        ],
      },
      // 06_Attachments
     {
        label: "06_Attachments",
        children: [
          {
            label: "Attachment",
            id: "6-1",
          },
          {
            label: "DeleteAllAttachments",
            id: "6-2",
          },
          {
            label: "GetAllAttachments",
            id: "6-3",
          },
          {
            label: "GetIndividualAttachment",
            id: "6-4",
          },
          {
            label: "GetPdfAttachmentInfo",
            id: "6-5",
          },
          {
            label: "PdfAttachmentRelationship",
            id: "6-6",
          },
          {
            label: "SortFileInPdf",
            id: "6-7",
          },
        ],
      },
      // 07_Bookmarks
     {
        label: "07_Bookmarks",
        children: [
          {
            label: "Bookmark",
            id: "7-1",
          },
          {
            label: "DeleteAllBookmarks",
            id: "7-2",
          },
          {
            label: "DeleteBookmark",
            id: "7-3",
          },
          {
            label: "ExpandBookmarks",
            id: "7-4",
          },
          {
            label: "ExpandSpecificBookmarks",
            id: "7-5",
          },
          {
            label: "GetAllPdfBookmarks",
            id: "7-6",
          },
          {
            label: "GetBookmarkPageNumber",
            id: "7-7",
          },
          {
            label: "GetPdfChildBookmarks",
            id: "7-8",
          },
          {
            label: "SetInheritZoom",
            id: "7-9",
          },
          {
            label: "SetInheritZoomForBookmarks",
            id: "7-10",
          },
          {
            label: "UpdateBookmark",
            id: "7-11",
          },
        ],
      },
      // 08_Forms
    {
        label: "08_Forms",
        children: [
          {
            label: "AddCalendarDropdown",
            id: "8-1",
          },
          {
            label: "AddCheckBox",
            id: "8-2",
          },
          {
            label: "AddComboBox",
            id: "8-3",
          },
          {
            label: "AddJavaScriptAction",
            id: "8-4",
          },
          {
            label: "AddRadioButtonCaption",
            id: "8-5",
          },
          {
            label: "AddRadioButtonField",
            id: "8-6",
          },
          {
            label: "AddRadioButtonFieldWithOptions",
            id: "8-7",
          },
          {
            label: "AddTextBoxField",
            id: "8-8",
          },
          {
            label: "AddTooltipForFormField",
            id: "8-9",
          },
          {
            label: "AssignIconToButtonField",
            id: "8-10",
          },
          {
            label: "AutoFontsizeForTextBoxField",
            id: "8-11",
          },
          {
            label: "AutomaticField",
            id: "8-12",
          },
          {
            label: "ChangeFormFieldLocation",
            id: "8-13",
          },
          {
            label: "DetermineRequiredField",
            id: "8-14",
          },
          {
            label: "ExtractJavaScript",
            id: "8-15",
          },
          {
            label: "FillFormField",
            id: "8-16",
          },
          {
            label: "FillImageInButtonField",
            id: "8-17",
          },
          {
            label: "FlattenFormField",
            id: "8-18",
          },
          {
            label: "GetCoordinates",
            id: "8-19",
          },
          {
            label: "GetFieldValue",
            id: "8-20",
          },
          {
            label: "GetJavaScriptFromCheckBoxField",
            id: "8-21",
          },
          {
            label: "GetStyleOfRadioButton",
            id: "8-22",
          },
          {
            label: "GetValuesFromAllFields",
            id: "8-23",
          },
          {
            label: "ModifyFormFieldValue",
            id: "8-24",
          },
          {
            label: "ModifyFormFieldVisibility",
            id: "8-25",
          },
          {
            label: "RecognizeRequiredField",
            id: "8-26",
          },
          {
            label: "RemoveFormField",
            id: "8-27",
          },
          {
            label: "SelectRadioButtonItem",
            id: "8-28",
          },
          {
            label: "SetExportValueForCheckbox",
            id: "8-29",
          },
          {
            label: "SetFontForFormField",
            id: "8-30",
          },
        ],
      },
      // 09_StampsAndWatermarks
    {
        label: "09_StampsAndWatermarks",
        children: [
          {
            label: "AddDateTimeStamp",
            id: "9-1",
          },
          {
            label: "AddImageStamp",
            id: "9-2",
          },
          {
            label: "AddTextStamp",
            id: "9-3",
          },
          {
            label: "AddTilingBackgroundImage",
            id: "9-4",
          },
          {
            label: "FillStrokeText",
            id: "9-5",
          },
          {
            label: "HeaderAndFooter",
            children: [
              {
                label: "AddDifferentHeaders",
                id: "9-6-1",
              },
              {
                label: "HeaderAndFooter",
                id: "9-6-2",
              },
              {
                label: "ImageAndPageNumber",
                id: "9-6-3",
              },
              {
                label: "ImageAndTextUsingTemplate",
                id: "9-6-4",
              },
              {
                label: "InlineImageAndPageNumber",
                id: "9-6-5",
              },
              {
                label: "PageNumberInFooter",
                id: "9-6-6",
              },
            ]
          },
          {
            label: "ImageWatermarkFirst",
            id: "9-7",
          },
          {
            label: "ImageWatermarkSecond",
            id: "9-8",
          },
          {
            label: "SetPropertiesForStamp",
            id: "9-9",
          },
          {
            label: "TextWaterMark",
            id: "9-10",
          },
        ],
      },
      // 10_LinksAndActions
      {
        label: "10_LinksAndActions",
        children: [
          {
            label: "AddPdfLaunchAction",
            id: "10-1",
          },
          {
            label: "AddTableOfContent",
            id: "10-2",
          },
          {
            label: "DocumentLinkAnnotation",
            id: "10-3",
          },
          {
            label: "EmbedSoundFile",
            id: "10-4",
          },
          {
            label: "ExtractAndUpdateLink",
            id: "10-5",
          },
          {
            label: "FileLinkAnnotation",
            id: "10-6",
          },
          {
            label: "GetLinkAnnotation",
            id: "10-7",
          },
          {
            label: "GoToAction",
            id: "10-8",
          },
          {
            label: "LaunchFileInNewWindow",
            id: "10-9",
          },
          {
            label: "Link",
            id: "10-10",
          },
          {
            label: "SpecifyPageToView",
            id: "10-11",
          },
        ],
      },
      // 11_Conversion
    {
        label: "11_Conversion",
        children: [
          {
            label: "AddAttachmentsToPDFA",
            id: "11-1",
          },
          {
            label: "ConvertToDocxSettingProperties",
            id: "11-2",
          },
          {
            label: "ConvertToGrayPdf",
            id: "11-3",
          },
          {
            label: "ConvertToOFD",
            id: "11-4",
          },
          {
            label: "EncryptedPDFToPDFA",
            id: "11-5",
          },
          {
            label: "OFDToPDF",
            id: "11-6",
          },
          {
            label: "PDFAToPDF",
            id: "11-7",
          },
          {
            label: "PdfToExcel",
            id: "11-8",
          },
          {
            label: "PdfToExcelOptions",
            id: "11-9",
          },
          {
            label: "PDFToHTML",
            children: [
              {
                label: "EmbedImages",
                id: "11-10-1",
              },
              {
                label: "EmbedSVG",
                id: "11-10-2"
              },
              {
                label: "ToHTML",
                id: "11-10-3"
              },
              {
                label: "ToHTMLFilesSplittedByPages",
                id: "11-10-4",
              },
              {
                label: "ToHTMLStream",
                id: "11-10-5"
              },
            ]
          },
          {
            label: "RemoveHyperlinks",
            id: "11-11",
          },
          {
            label: "SVGToPDF",
            id: "11-12",
          },
          {
            label: "TextToPDF",
            id: "11-13",
          },
          {
            label: "ToDoc",
            id: "11-14",
          },
          {
            label: "ToDocx",
            id: "11-15",
          },
          {
            label: "ToDocxNew",
            id: "11-16",
          },
          {
            label: "ToImage",
            id: "11-17",
          },
          {
            label: "ToLinearizedPdf",
            id: "11-18",
          },
          {
            label: "ToPCL",
            id: "11-19",
          },
          {
            label: "ToPDFA",
            id: "11-20",
          },
          {
            label: "ToPdfA2B",
            id: "11-21",
          },
          {
            label: "ToPostScript",
            id: "11-22",
          },
          {
            label: "ToSVG",
            id: "11-23",
          },
          {
            label: "ToTransparentBackgroundImages",
            id: "11-24",
          },
          {
            label: "ToXLSX",
            id: "11-25",
          },
          {
            label: "ToXPS",
            id: "11-26",
          },
        ],
      },
      // 12_Page
      {
        label: "12_Page",
        children: [
          {
            label: "DeletePage",
            id: "12-1",
          },
          {
            label: "Extraction",
            id: "12-2",
          },
          {
            label: "GetNumberOfPages",
            id: "12-3",
          },
          {
            label: "GetPageInfo",
            id: "12-4",
          },
          {
            label: "GetPageLabel",
            id: "12-5",
          },
          {
            label: "GetPageSize",
            id: "12-6",
          },
          {
            label: "InsertEmptyPage",
            id: "12-7",
          },
          {
            label: "InsertEmptyPageAtEnd",
            id: "12-8",
          },
          {
            label: "PageLable",
            id: "12-9",
          },
          {
            label: "PageSetup",
            id: "12-10",
          },
          {
            label: "Pagination",
            id: "12-11",
          },
          {
            label: "ResetPageSize",
            id: "12-12",
          },
          {
            label: "RotateExistingPDF",
            id: "12-13",
          },
          {
            label: "RotateNewPDF",
            id: "12-14",
          },
          {
            label: "SetPageOrientation",
            id: "12-15",
          },
          {
            label: "SetTabOrder",
            id: "12-16",
          },
          {
            label: "SplitAPageIntoMultipage",
            id: "12-17",
          },
          {
            label: "SplitFileByParticularPage",
            id: "12-18",
          },
          {
            label: "ZoomToPageContents",
            id: "12-19",
          },
        ],
      },
      // 13_Document
      {
        label: "13_Document",
        children: [
          {
            label: "AddLayers",
            id: "13-1",
          },
          {
            label: "Booklet",
            id: "13-2",
          },
          {
            label: "ChangePdfVersion",
            id: "13-3",
          },
          {
            label: "CompressDocument",
            id: "13-4",
          },
          {
            label: "CompressDocumentSecondApproach",
            id: "13-5",
          },
          {
            label: "CreateMultilayerPDF",
            id: "13-6",
          },
          {
            label: "CreatePDFA1WithSpirePDF",
            id: "13-7",
          },
          {
            label: "CreatePDFPortfolio",
            id: "13-8",
          },
          {
            label: "CreateTwoColumnPDF",
            id: "13-9",
          },
          {
            label: "CustomDocumentProperties",
            id: "13-10",
          },
          {
            label: "DeleteLayer",
            id: "13-11",
          },
          {
            label: "GetDocumentProperties",
            id: "13-12",
          },
          {
            label: "GetViewerPreference",
            id: "13-13",
          },
          {
            label: "GetZoomFactor",
            id: "13-14",
          },
          {
            label: "InvisibleAllPdfLayers",
            id: "13-15",
          },
          {
            label: "InvisibleParticularPdfLayers",
            id: "13-16",
          },
          {
            label: "IsPDFPortfolio",
            id: "13-17",
          },
          {
            label: "MergeDocuments",
            id: "13-18",
          },
          {
            label: "MergePdfsByFile",
            id: "13-19",
          },
          {
            label: "ModifyPageMargins",
            id: "13-20",
          },
          {
            label: "Properties",
            id: "13-21",
          },
          {
            label: "RearrangePageOrder",
            id: "13-22",
          },
          {
            label: "RemovePageMargins",
            id: "13-23",
          },
          {
            label: "SetExpiryDate",
            id: "13-24",
          },
          {
            label: "SetLayerPrintProperties",
            id: "13-25",
          },
          {
            label: "SetMagnificationToFitHeight",
            id: "13-26",
          },
          {
            label: "SetXMPMetadata",
            id: "13-27",
          },
          {
            label: "SetZoomFactor",
            id: "13-28",
          },
          {
            label: "SplitDocument",
            id: "13-29",
          },
          {
            label: "Template",
            id: "13-30",
          },
          {
            label: "Transition",
            id: "13-31",
          },
          {
            label: "ViewerPreference",
            id: "13-32",
          },
        ],
      },
    ],
  },
];
const defaultProps = {
  children: "children",
  label: "label",
};
</script>