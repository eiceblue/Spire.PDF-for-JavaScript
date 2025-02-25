<template>
  <div class="worker_page">
    <div class="tree_item">
      <SpireTree @item-id="handleChildID" />
    </div>
    <div class="file_item">
      <div class="file-reader">
        <div v-if="filePageId == '0'" class="non-centered">
          <Description />
        </div>

        <!--01_QuickGuide -->
        <div v-if="filePageId == '1-1'">
          <HelloWorld />
        </div>

         <!--02_Text -->
         <div v-if="filePageId == '2-1'">
          <AddBorderForText />
        </div>
        <div v-if="filePageId == '2-2'">
          <AddTooltipToText />
        </div>
        <div v-if="filePageId == '2-3'">
          <AddTransparentText />
        </div>
        <div v-if="filePageId == '2-4'">
          <DrawRotatedText />
        </div>
        <div v-if="filePageId == '2-5'">
          <DrawText /> 
        </div>
        <div v-if="filePageId == '2-6'">
          <DrawTextWithGradient /> 
        </div>
        <div v-if="filePageId == '2-7'">
          <ExtractHighlightedText /> 
        </div>
        <div v-if="filePageId == '2-8'">
          <ExtractTextFromParticularPage /> 
        </div>
        <div v-if="filePageId == '2-9'">
          <ExtractTextFromSpecificArea /> 
        </div>
        <div v-if="filePageId == '2-10'">
          <FindAndHighlightText /> 
        </div>
        <div v-if="filePageId == '2-11'">
          <FindTextInDefineArea /> 
        </div>
        <div v-if="filePageId == '2-12'">
          <Font /> 
        </div>
        <div v-if="filePageId == '2-13'">
          <GetDetailsOfSearchedText /> 
        </div>
        <div v-if="filePageId == '2-14'">
          <GetTextSizeBasedOnFont /> 
        </div>
        <div v-if="filePageId == '2-15'">
          <ReplaceAllSearchedText /> 
        </div>
        <div v-if="filePageId == '2-16'">
          <ReplaceFirstSearchedText /> 
        </div>
        <div v-if="filePageId == '2-17'">
          <ReplaceFont /> 
        </div>
        <div v-if="filePageId == '2-18'">
          <ReplaceTextSecond /> 
        </div>
        <div v-if="filePageId == '2-19'">
          <SearchTextAndAddHyperlink /> 
        </div>
        <div v-if="filePageId == '2-20'">
          <SearchTextAndDrawRectangle /> 
        </div>
        <div v-if="filePageId == '2-21'">
          <SearchWithRegularExpression /> 
        </div>
        <div v-if="filePageId == '2-22'">
          <SetLineBreak /> 
        </div>
        <div v-if="filePageId == '2-23'">
          <SuperScriptAndSubScript /> 
        </div>
        <div v-if="filePageId == '2-24'">
          <TextLayout /> 
        </div>
        <div v-if="filePageId == '2-25'">
          <WrapTextAroundImage /> 
        </div>

          <!--03_Images -->
        <div v-if="filePageId == '3-1'">
          <AddSVGToPDF /> 
        </div>
        <div v-if="filePageId == '3-2'">
          <ConvertAllPagesToPNG /> 
        </div>
        <div v-if="filePageId == '3-3'">
          <ConvertImageStreamToPDF /> 
        </div>
        <div v-if="filePageId == '3-4'">
          <ConvertImageToPDF /> 
        </div>
        <div v-if="filePageId == '3-5'">
          <DeleteImageByBounds /> 
        </div>
        <div v-if="filePageId == '3-6'">
          <DeleteImageFirstApproach /> 
        </div>
        <div v-if="filePageId == '3-7'">
          <DrawImage /> 
        </div>
        <div v-if="filePageId == '3-8'">
          <PageToPNG /> 
        </div>
        <div v-if="filePageId == '3-9'">
          <ReplaceImage /> 
        </div>
        <div v-if="filePageId == '3-10'">
          <ReplaceImageWithText/> 
        </div>
        <div v-if="filePageId == '3-11'">
          <SetImageSize /> 
        </div>

        <!--04_Graphs -->
        <div v-if="filePageId == '4-1'">
          <Barcode />
        </div>
        <div v-if="filePageId == '4-2'">
          <DrawContentWithSpotColor />
        </div>
        <div v-if="filePageId == '4-3'">
          <DrawDashedLine />
        </div>
        <div v-if="filePageId == '4-4'">
          <DrawFilledRectangles />
        </div>
        <div v-if="filePageId == '4-5'">
          <DrawLine />
        </div>
        <div v-if="filePageId == '4-6'">
          <DrawRectangles />
        </div>
        <div v-if="filePageId == '4-7'">
          <DrawShape />
        </div>
        <div v-if="filePageId == '4-8'">
          <DrawShapeWithGradientFill />
        </div>
        <div v-if="filePageId == '4-9'">
          <List />
        </div>
        <div v-if="filePageId == '4-10'">
          <Overlay />
        </div>
        <div v-if="filePageId == '4-11'">
          <SetRectangleTransparency />
        </div>
        <div v-if="filePageId == '4-12'">
          <SetSeparationColorSpace />
        </div>
        <div v-if="filePageId == '4-13'">
          <Transparency />
        </div>

        <!--05_Annotations -->
        <div v-if="filePageId == '5-1'">
          <AddFreeTextAnnotation />
        </div>
        <div v-if="filePageId == '5-2'">
          <Annotation />
        </div>
        <div v-if="filePageId == '5-3'">
          <CreatePdf3DAnnotation />
        </div>
        <div v-if="filePageId == '5-4'">
          <CreatePdfLineAnnotation />
        </div>
        <div v-if="filePageId == '5-5'">
          <CreatePdfLinkAnnotation />
        </div>
        <div v-if="filePageId == '5-6'">
          <CreatePdfPolygonAnnotation />
        </div>
        <div v-if="filePageId == '5-7'">
          <CreatePolylineAnnotation />
        </div>
        <div v-if="filePageId == '5-8'">
          <DeleteAllAnnotations />
        </div>
        <div v-if="filePageId == '5-9'">
          <DeleteAnnotation />
        </div>
        <div v-if="filePageId == '5-10'">
          <Extract3DViedoFile />
        </div>
        <div v-if="filePageId == '5-11'">
          <GetAllAnnotationsFromPage />
        </div>
        <div v-if="filePageId == '5-12'">
          <GetParticularAnnotationInfo />
        </div>
        <div v-if="filePageId == '5-13'">
          <InvisibleFreeTextAnnotation />
        </div>
        <div v-if="filePageId == '5-14'">
          <ModifyLineAnnotation />
        </div>
        <div v-if="filePageId == '5-15'">
          <SetFreeTextAnnotationAlignment />
        </div>
        <div v-if="filePageId == '5-16'">
          <SetFreeTextAnnotationStyle />
        </div>
        <div v-if="filePageId == '5-17'">
          <SetFreeTextAnnotationSubject />
        </div>
        <div v-if="filePageId == '5-18'">
          <TextAnnotationProperties />
        </div>
        <div v-if="filePageId == '5-19'">
          <UpdateFreeTextAnnotation />
        </div>

        <!--06_Attachments -->
        <div v-if="filePageId == '6-1'">
          <Attachment />
        </div>
        <div v-if="filePageId == '6-2'">
          <DeleteAllAttachments />
        </div>
        <div v-if="filePageId == '6-3'">
          <GetAllAttachments />
        </div>
        <div v-if="filePageId == '6-4'">
          <GetIndividualAttachment />
        </div>
        <div v-if="filePageId == '6-5'">
          <GetPdfAttachmentInfo />
        </div>
        <div v-if="filePageId == '6-6'">
          <PdfAttachmentRelationship />
        </div>
        <div v-if="filePageId == '6-7'">
          <SortFileInPdf />
        </div>

        <!--07_Bookmarks -->
        <div v-if="filePageId == '7-1'">
          <Bookmark /> 
        </div>
        <div v-if="filePageId == '7-2'">
          <DeleteAllBookmarks /> 
        </div>
        <div v-if="filePageId == '7-3'">
          <DeleteBookmark /> 
        </div>
        <div v-if="filePageId == '7-4'">
          <ExpandBookmarks /> 
        </div>
        <div v-if="filePageId == '7-5'">
          <ExpandSpecificBookmarks /> 
        </div>
        <div v-if="filePageId == '7-6'">
          <GetAllPdfBookmarks /> 
        </div>
        <div v-if="filePageId == '7-7'">
          <GetBookmarkPageNumber /> 
        </div>
        <div v-if="filePageId == '7-8'">
          <GetPdfChildBookmarks /> 
        </div>
        <div v-if="filePageId == '7-9'">
          <SetInheritZoom /> 
        </div>
        <div v-if="filePageId == '7-10'">
          <SetInheritZoomForBookmarks /> 
        </div>
        <div v-if="filePageId == '7-11'">
          <UpdateBookmark /> 
        </div>

        <!--08_Forms -->
        <div v-if="filePageId == '8-1'">
          <AddCalendarDropdown /> 
        </div>
        <div v-if="filePageId == '8-2'">
          <AddCheckBox /> 
        </div>
        <div v-if="filePageId == '8-3'">
          <AddComboBox /> 
        </div>
        <div v-if="filePageId == '8-4'">
          <AddJavaScriptAction /> 
        </div>
        <div v-if="filePageId == '8-5'">
          <AddRadioButtonCaption /> 
        </div>
        <div v-if="filePageId == '8-6'">
          <AddRadioButtonField /> 
        </div>
        <div v-if="filePageId == '8-7'">
          <AddRadioButtonFieldWithOptions /> 
        </div>
        <div v-if="filePageId == '8-8'">
          <AddTextBoxField /> 
        </div>
        <div v-if="filePageId == '8-9'">
          <AddTooltipForFormField /> 
        </div>
        <div v-if="filePageId == '8-10'">
          <AssignIconToButtonField /> 
        </div>
        <div v-if="filePageId == '8-11'">
          <AutoFontsizeForTextBoxField /> 
        </div>
        <div v-if="filePageId == '8-12'">
          <AutomaticField /> 
        </div>
        <div v-if="filePageId == '8-13'">
          <ChangeFormFieldLocation /> 
        </div>
        <div v-if="filePageId == '8-14'">
          <DetermineRequiredField /> 
        </div>
        <div v-if="filePageId == '8-15'">
          <ExtractJavaScript /> 
        </div>
        <div v-if="filePageId == '8-16'">
          <FillFormField /> 
        </div>
        <div v-if="filePageId == '8-17'">
          <FillImageInButtonField /> 
        </div>
        <div v-if="filePageId == '8-18'">
          <FlattenFormField /> 
        </div>
        <div v-if="filePageId == '8-19'">
          <GetCoordinates /> 
        </div>
        <div v-if="filePageId == '8-20'">
          <GetFieldValue /> 
        </div>
        <div v-if="filePageId == '8-21'">
          <GetJavaScriptFromCheckBoxField /> 
        </div>
        <div v-if="filePageId == '8-22'">
          <GetStyleOfRadioButton /> 
        </div>
        <div v-if="filePageId == '8-23'">
          <GetValuesFromAllFields /> 
        </div>
        <div v-if="filePageId == '8-24'">
          <ModifyFormFieldValue /> 
        </div>
        <div v-if="filePageId == '8-25'">
          <ModifyFormFieldVisibility /> 
        </div>
        <div v-if="filePageId == '8-26'">
          <RecognizeRequiredField /> 
        </div>
        <div v-if="filePageId == '8-27'">
          <RemoveFormField /> 
        </div>
        <div v-if="filePageId == '8-28'">
          <SelectRadioButtonItem /> 
        </div>
        <div v-if="filePageId == '8-29'">
          <SetExportValueForCheckbox /> 
        </div>
        <div v-if="filePageId == '8-30'">
          <SetFontForFormField /> 
        </div>

        <!--09_StampsAndWatermarks -->
        <div v-if="filePageId == '9-1'">
          <AddDateTimeStamp /> 
        </div>
        <div v-if="filePageId == '9-2'">
          <AddImageStamp /> 
        </div>
        <div v-if="filePageId == '9-3'">
          <AddTextStamp /> 
        </div>
        <div v-if="filePageId == '9-4'">
          <AddTilingBackgroundImage /> 
        </div>
        <div v-if="filePageId == '9-5'">
          <FillStrokeText /> 
        </div>
        <div v-if="filePageId == '9-6-1'">
          <AddDifferentHeaders /> 
        </div>
        <div v-if="filePageId == '9-6-2'">
          <HeaderAndFooter /> 
        </div>
        <div v-if="filePageId == '9-6-3'">
          <ImageAndPageNumber /> 
        </div>
        <div v-if="filePageId == '9-6-4'">
          <ImageAndTextUsingTemplate /> 
        </div>
        <div v-if="filePageId == '9-6-5'">
          <InlineImageAndPageNumber /> 
        </div>
        <div v-if="filePageId == '9-6-6'">
          <PageNumberInFooter /> 
        </div>
        <div v-if="filePageId == '9-7'">
          <ImageWatermarkFirst /> 
        </div>
        <div v-if="filePageId == '9-8'">
          <ImageWatermarkSecond /> 
        </div>
        <div v-if="filePageId == '9-9'">
          <SetPropertiesForStamp /> 
        </div>
        <div v-if="filePageId == '9-10'">
          <TextWaterMark /> 
        </div>

        <!--10_LinksAndActions -->
        <div v-if="filePageId == '10-1'">
          <AddPdfLaunchAction /> 
        </div>
        <div v-if="filePageId == '10-2'">
          <AddTableOfContent /> 
        </div>
        <div v-if="filePageId == '10-3'">
          <DocumentLinkAnnotation /> 
        </div>
        <div v-if="filePageId == '10-4'">
          <EmbedSoundFile /> 
        </div>
        <div v-if="filePageId == '10-5'">
          <ExtractAndUpdateLink /> 
        </div>
        <div v-if="filePageId == '10-6'">
          <FileLinkAnnotation /> 
        </div>
        <div v-if="filePageId == '10-7'">
          <GetLinkAnnotation /> 
        </div>
        <div v-if="filePageId == '10-8'">
          <GoToAction /> 
        </div>
        <div v-if="filePageId == '10-9'">
          <LaunchFileInNewWindow /> 
        </div>
        <div v-if="filePageId == '10-10'">
          <Link /> 
        </div>
        <div v-if="filePageId == '10-11'">
          <SpecifyPageToView /> 
        </div>

        <!--11_Conversion -->
        <div v-if="filePageId == '11-1'">
          <AddAttachmentsToPDFA /> 
        </div>
        <div v-if="filePageId == '11-2'">
          <ConvertToDocxSettingProperties /> 
        </div>
        <div v-if="filePageId == '11-3'">
          <ConvertToGrayPdf /> 
        </div>
        <div v-if="filePageId == '11-4'">
          <ConvertToOFD /> 
        </div>
        <div v-if="filePageId == '11-5'">
          <EncryptedPDFToPDFA /> 
        </div>
        <div v-if="filePageId == '11-6'">
          <OFDToPDF /> 
        </div>
        <div v-if="filePageId == '11-7'">
          <PDFAToPDF /> 
        </div>
        <div v-if="filePageId == '11-8'">
          <PdfToExcel /> 
        </div>
        <div v-if="filePageId == '11-9'">
          <PdfToExcelOptions /> 
        </div>
        <div v-if="filePageId == '11-10-1'">
          <EmbedImages /> 
        </div>
        <div v-if="filePageId == '11-10-2'">
          <EmbedSVG /> 
        </div>
        <div v-if="filePageId == '11-10-3'">
          <ToHTML /> 
        </div>
        <div v-if="filePageId == '11-10-4'">
          <ToHTMLFilesSplittedByPages /> 
        </div>
        <div v-if="filePageId == '11-10-5'">
          <ToHTMLStream /> 
        </div>
        <div v-if="filePageId == '11-11'">
          <RemoveHyperlinks /> 
        </div>
        <div v-if="filePageId == '11-12'">
          <SVGToPDF /> 
        </div>
        <div v-if="filePageId == '11-13'">
          <TextToPDF /> 
        </div>
        <div v-if="filePageId == '11-14'">
          <ToDoc /> 
        </div>
        <div v-if="filePageId == '11-15'">
          <ToDocx /> 
        </div>
        <div v-if="filePageId == '11-16'">
          <ToDocxNew /> 
        </div>
        <div v-if="filePageId == '11-17'">
          <ToImage /> 
        </div>
        <div v-if="filePageId == '11-18'">
          <ToLinearizedPdf /> 
        </div>
        <div v-if="filePageId == '11-19'">
          <ToPCL /> 
        </div>
        <div v-if="filePageId == '11-20'">
          <ToPDFA /> 
        </div>
        <div v-if="filePageId == '11-21'">
          <ToPdfA2B /> 
        </div>
        <div v-if="filePageId == '11-22'">
          <ToPostScript /> 
        </div>
        <div v-if="filePageId == '11-23'">
          <ToSVG /> 
        </div>
        <div v-if="filePageId == '11-24'">
          <ToTransparentBackgroundImages /> 
        </div>
        <div v-if="filePageId == '11-25'">
          <ToXLSX /> 
        </div>
        <div v-if="filePageId == '11-26'">
          <ToXPS /> 
        </div>

        <!--12_Page -->
        <div v-if="filePageId == '12-1'">
          <DeletePage />
        </div>
        <div v-if="filePageId == '12-2'">
          <Extraction />
        </div>
        <div v-if="filePageId == '12-3'">
          <GetNumberOfPages />
        </div>
        <div v-if="filePageId == '12-4'">
          <GetPageInfo />
        </div>
        <div v-if="filePageId == '12-5'">
          <GetPageLabel />
        </div>
        <div v-if="filePageId == '12-6'">
          <GetPageSize />
        </div>
        <div v-if="filePageId == '12-7'">
          <InsertEmptyPage />
        </div>
        <div v-if="filePageId == '12-8'">
          <InsertEmptyPageAtEnd />
        </div>
        <div v-if="filePageId == '12-9'">
          <PageLable />
        </div>
        <div v-if="filePageId == '12-10'">
          <PageSetup />
        </div>
        <div v-if="filePageId == '12-11'">
          <Pagination />
        </div>
        <div v-if="filePageId == '12-12'">
          <ResetPageSize />
        </div>
        <div v-if="filePageId == '12-13'">
          <RotateExistingPDF />
        </div>
        <div v-if="filePageId == '12-14'">
          <RotateNewPDF />
        </div>
        <div v-if="filePageId == '12-15'">
          <SetPageOrientation />
        </div>
        <div v-if="filePageId == '12-16'">
          <SetTabOrder />
        </div>
        <div v-if="filePageId == '12-17'">
          <SplitAPageIntoMultipage />
        </div>
        <div v-if="filePageId == '12-18'">
          <SplitFileByParticularPage />
        </div>
        <div v-if="filePageId == '12-19'">
          <ZoomToPageContents />
        </div>

        <!--13_Document -->
        <div v-if="filePageId == '13-1'">
          <AddLayers />
        </div>
        <div v-if="filePageId == '13-2'">
          <Booklet />
        </div>
        <div v-if="filePageId == '13-3'">
          <ChangePdfVersion />
        </div>
        <div v-if="filePageId == '13-4'">
          <CompressDocument />
        </div>
        <div v-if="filePageId == '13-5'">
          <CompressDocumentSecondApproach />
        </div>
        <div v-if="filePageId == '13-6'">
          <CreateMultilayerPDF />
        </div>
        <div v-if="filePageId == '13-7'">
          <CreatePDFA1WithSpirePDF />
        </div>
        <div v-if="filePageId == '13-8'">
          <CreatePDFPortfolio />
        </div>
        <div v-if="filePageId == '13-9'">
          <CreateTwoColumnPDF />
        </div>
        <div v-if="filePageId == '13-10'">
          <CustomDocumentProperties />
        </div>
        <div v-if="filePageId == '13-11'">
          <DeleteLayer />
        </div>
        <div v-if="filePageId == '13-12'">
          <GetDocumentProperties />
        </div>
        <div v-if="filePageId == '13-13'">
          <GetViewerPreference />
        </div>
        <div v-if="filePageId == '13-14'">
          <GetZoomFactor />
        </div>
        <div v-if="filePageId == '13-15'">
          <InvisibleAllPdfLayers />
        </div>
        <div v-if="filePageId == '13-16'">
          <InvisibleParticularPdfLayers />
        </div>
        <div v-if="filePageId == '13-17'">
          <IsPDFPortfolio />
        </div>
        <div v-if="filePageId == '13-18'">
          <MergeDocuments />
        </div>
        <div v-if="filePageId == '13-19'">
          <MergePdfsByFile />
        </div>
        <div v-if="filePageId == '13-20'">
          <ModifyPageMargins />
        </div>
        <div v-if="filePageId == '13-21'">
          <Properties />
        </div>
        <div v-if="filePageId == '13-22'">
          <RearrangePageOrder />
        </div>
        <div v-if="filePageId == '13-23'">
          <RemovePageMargins />
        </div>
        <div v-if="filePageId == '13-24'">
          <SetExpiryDate />
        </div>
        <div v-if="filePageId == '13-25'">
          <SetLayerPrintProperties />
        </div>
        <div v-if="filePageId == '13-26'">
          <SetMagnificationToFitHeight />
        </div>
        <div v-if="filePageId == '13-27'">
          <SetXMPMetadata />
        </div>
        <div v-if="filePageId == '13-28'">
          <SetZoomFactor />
        </div>
        <div v-if="filePageId == '13-29'">
          <SplitDocument />
        </div>
        <div v-if="filePageId == '13-30'">
          <Template />
        </div>
        <div v-if="filePageId == '13-31'">
          <Transitions />
        </div>
        <div v-if="filePageId == '13-32'">
          <ViewerPreference />
        </div>

      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import SpireTree from "@/components/SpireTree.vue";
import Description from "@/components/Description.vue";

//01_QuickGuide
import HelloWorld from "@/components/01_QuickGuide/HelloWorld.vue";

//02_Text
import AddBorderForText from "@/components/02_Text/AddBorderForText.vue";
import AddTooltipToText from "@/components/02_Text/AddTooltipToText.vue";
import AddTransparentText from "@/components/02_Text/AddTransparentText.vue";
import DrawRotatedText from "@/components/02_Text/DrawRotatedText.vue";
import DrawText from "@/components/02_Text/DrawText.vue";
import DrawTextWithGradient from "@/components/02_Text/DrawTextWithGradient.vue";
import ExtractHighlightedText from "@/components/02_Text/ExtractHighlightedText.vue";
import ExtractTextFromParticularPage from "@/components/02_Text/ExtractTextFromParticularPage.vue";
import ExtractTextFromSpecificArea from "@/components/02_Text/ExtractTextFromSpecificArea.vue";
import FindAndHighlightText from "@/components/02_Text/FindAndHighlightText.vue";
import FindTextInDefineArea from "@/components/02_Text/FindTextInDefineArea.vue";
import Font from "@/components/02_Text/Font.vue";
import GetDetailsOfSearchedText from "@/components/02_Text/GetDetailsOfSearchedText.vue";
import GetTextSizeBasedOnFont from "@/components/02_Text/GetTextSizeBasedOnFont.vue";
import ReplaceAllSearchedText from "@/components/02_Text/ReplaceAllSearchedText.vue";
import ReplaceFirstSearchedText from "@/components/02_Text/ReplaceFirstSearchedText.vue";
import ReplaceFont from "@/components/02_Text/ReplaceFont.vue";
import ReplaceTextSecond from "@/components/02_Text/ReplaceTextSecond.vue";
import SearchTextAndAddHyperlink from "@/components/02_Text/SearchTextAndAddHyperlink.vue";
import SearchTextAndDrawRectangle from "@/components/02_Text/SearchTextAndDrawRectangle.vue";
import SearchWithRegularExpression from "@/components/02_Text/SearchWithRegularExpression.vue";
import SetLineBreak from "@/components/02_Text/SetLineBreak.vue";
import SuperScriptAndSubScript from "@/components/02_Text/SuperScriptAndSubScript.vue";
import TextLayout from "@/components/02_Text/TextLayout.vue";
import WrapTextAroundImage from "@/components/02_Text/WrapTextAroundImage.vue";

//03_Images
import AddSVGToPDF from "@/components/03_Images/AddSVGToPDF.vue";
import ConvertAllPagesToPNG from "@/components/03_Images/ConvertAllPagesToPNG.vue";
import ConvertImageStreamToPDF from "@/components/03_Images/ConvertImageStreamToPDF.vue";
import ConvertImageToPDF from "@/components/03_Images/ConvertImageToPDF.vue";
import DeleteImageByBounds from "@/components/03_Images/DeleteImageByBounds.vue";
import DeleteImageFirstApproach from "@/components/03_Images/DeleteImageFirstApproach.vue";
import DrawImage from "@/components/03_Images/DrawImage.vue";
import PageToPNG from "@/components/03_Images/PageToPNG.vue";
import ReplaceImage from "@/components/03_Images/ReplaceImage.vue";
import ReplaceImageWithText from "@/components/03_Images/ReplaceImageWithText.vue";
import SetImageSize from "@/components/03_Images/SetImageSize.vue";

//04_Graphs
import Barcode from "@/components/04_Graphs/Barcode.vue";
import DrawContentWithSpotColor from "@/components/04_Graphs/DrawContentWithSpotColor.vue";
import DrawDashedLine from "@/components/04_Graphs/DrawDashedLine.vue";
import DrawFilledRectangles from "@/components/04_Graphs/DrawFilledRectangles.vue";
import DrawLine from "@/components/04_Graphs/DrawLine.vue";
import DrawRectangles from "@/components/04_Graphs/DrawRectangles.vue";
import DrawShape from "@/components/04_Graphs/DrawShape.vue";
import DrawShapeWithGradientFill from "@/components/04_Graphs/DrawShapeWithGradientFill.vue";
import List from "@/components/04_Graphs/List.vue";
import Overlay from "@/components/04_Graphs/Overlay.vue";
import SetRectangleTransparency from "@/components/04_Graphs/SetRectangleTransparency.vue";
import SetSeparationColorSpace from "@/components/04_Graphs/SetSeparationColorSpace.vue";
import Transparency from "@/components/04_Graphs/Transparency.vue";

//05_Annotations
import AddFreeTextAnnotation from "@/components/05_Annotations/AddFreeTextAnnotation.vue";
import Annotation from "@/components/05_Annotations/Annotation.vue";
import CreatePdf3DAnnotation from "@/components/05_Annotations/CreatePdf3DAnnotation.vue";
import CreatePdfLineAnnotation from "@/components/05_Annotations/CreatePdfLineAnnotation.vue";
import CreatePdfLinkAnnotation from "@/components/05_Annotations/CreatePdfLinkAnnotation.vue";
import CreatePdfPolygonAnnotation from "@/components/05_Annotations/CreatePdfPolygonAnnotation.vue";
import CreatePolylineAnnotation from "@/components/05_Annotations/CreatePolylineAnnotation.vue";
import DeleteAllAnnotations from "@/components/05_Annotations/DeleteAllAnnotations.vue";
import DeleteAnnotation from "@/components/05_Annotations/DeleteAnnotation.vue";
import Extract3DViedoFile from "@/components/05_Annotations/Extract3DViedoFile.vue";
import GetAllAnnotationsFromPage from "@/components/05_Annotations/GetAllAnnotationsFromPage.vue";
import GetParticularAnnotationInfo from "@/components/05_Annotations/GetParticularAnnotationInfo.vue";
import InvisibleFreeTextAnnotation from "@/components/05_Annotations/InvisibleFreeTextAnnotation.vue";
import ModifyLineAnnotation from "@/components/05_Annotations/ModifyLineAnnotation.vue";
import SetFreeTextAnnotationAlignment from "@/components/05_Annotations/SetFreeTextAnnotationAlignment.vue";
import SetFreeTextAnnotationStyle from "@/components/05_Annotations/SetFreeTextAnnotationStyle.vue";
import SetFreeTextAnnotationSubject from "@/components/05_Annotations/SetFreeTextAnnotationSubject.vue";
import TextAnnotationProperties from "@/components/05_Annotations/TextAnnotationProperties.vue";
import UpdateFreeTextAnnotation from "@/components/05_Annotations/UpdateFreeTextAnnotation.vue";

//06_Attachments
import Attachment from "@/components/06_Attachments/Attachment.vue";
import DeleteAllAttachments from "@/components/06_Attachments/DeleteAllAttachments.vue";
import GetAllAttachments from "@/components/06_Attachments/GetAllAttachments.vue";
import GetIndividualAttachment from "@/components/06_Attachments/GetIndividualAttachment.vue";
import GetPdfAttachmentInfo from "@/components/06_Attachments/GetPdfAttachmentInfo.vue";
import PdfAttachmentRelationship from "@/components/06_Attachments/PdfAttachmentRelationship.vue";
import SortFileInPdf from "@/components/06_Attachments/SortFileInPdf.vue";

//07_Bookmarks
import Bookmark from "@/components/07_Bookmarks/Bookmark.vue";
import DeleteAllBookmarks from "@/components/07_Bookmarks/DeleteAllBookmarks.vue";
import DeleteBookmark from "@/components/07_Bookmarks/DeleteBookmark.vue";
import ExpandBookmarks from "@/components/07_Bookmarks/ExpandBookmarks.vue";
import ExpandSpecificBookmarks from "@/components/07_Bookmarks/ExpandSpecificBookmarks.vue";
import GetAllPdfBookmarks from "@/components/07_Bookmarks/GetAllPdfBookmarks.vue";
import GetBookmarkPageNumber from "@/components/07_Bookmarks/GetBookmarkPageNumber.vue";
import GetPdfChildBookmarks from "@/components/07_Bookmarks/GetPdfChildBookmarks.vue";
import SetInheritZoom from "@/components/07_Bookmarks/SetInheritZoom.vue";
import SetInheritZoomForBookmarks from "@/components/07_Bookmarks/SetInheritZoomForBookmarks.vue";
import UpdateBookmark from "@/components/07_Bookmarks/UpdateBookmark.vue";

//08_Forms
import AddCalendarDropdown from "@/components/08_Forms/AddCalendarDropdown.vue";
import AddCheckBox from "@/components/08_Forms/AddCheckBox.vue";
import AddComboBox from "@/components/08_Forms/AddComboBox.vue";
import AddJavaScriptAction from "@/components/08_Forms/AddJavaScriptAction.vue";
import AddRadioButtonCaption from "@/components/08_Forms/AddRadioButtonCaption.vue";
import AddRadioButtonField from "@/components/08_Forms/AddRadioButtonField.vue";
import AddRadioButtonFieldWithOptions from "@/components/08_Forms/AddRadioButtonFieldWithOptions.vue";
import AddTextBoxField from "@/components/08_Forms/AddTextBoxField.vue";
import AddTooltipForFormField from "@/components/08_Forms/AddTooltipForFormField.vue";
import AssignIconToButtonField from "@/components/08_Forms/AssignIconToButtonField.vue";
import AutoFontsizeForTextBoxField from "@/components/08_Forms/AutoFontsizeForTextBoxField.vue";
import AutomaticField from "@/components/08_Forms/AutomaticField.vue";
import ChangeFormFieldLocation from "@/components/08_Forms/ChangeFormFieldLocation.vue";
import DetermineRequiredField from "@/components/08_Forms/DetermineRequiredField.vue";
import ExtractJavaScript from "@/components/08_Forms/ExtractJavaScript.vue";
import FillFormField from "@/components/08_Forms/FillFormField.vue";
import FillImageInButtonField from "@/components/08_Forms/FillImageInButtonField.vue"; 
import FlattenFormField from "@/components/08_Forms/FlattenFormField.vue";
import GetCoordinates from "@/components/08_Forms/GetCoordinates.vue";
import GetFieldValue from "@/components/08_Forms/GetFieldValue.vue";
import GetJavaScriptFromCheckBoxField from "@/components/08_Forms/GetJavaScriptFromCheckBoxField.vue";
import GetStyleOfRadioButton from "@/components/08_Forms/GetStyleOfRadioButton.vue";
import GetValuesFromAllFields from "@/components/08_Forms/GetValuesFromAllFields.vue";
import ModifyFormFieldValue from "@/components/08_Forms/ModifyFormFieldValue.vue";
import ModifyFormFieldVisibility from "@/components/08_Forms/ModifyFormFieldVisibility.vue";
import RecognizeRequiredField from "@/components/08_Forms/RecognizeRequiredField.vue";
import RemoveFormField from "@/components/08_Forms/RemoveFormField.vue";
import SelectRadioButtonItem from "@/components/08_Forms/SelectRadioButtonItem.vue";
import SetExportValueForCheckbox from "@/components/08_Forms/SetExportValueForCheckbox.vue";
import SetFontForFormField from "@/components/08_Forms/SetFontForFormField.vue";

//09_StampsAndWatermarks
import AddDateTimeStamp from "@/components/09_StampsAndWatermarks/AddDateTimeStamp.vue";
import AddImageStamp from "@/components/09_StampsAndWatermarks/AddImageStamp.vue";
import AddTextStamp from "@/components/09_StampsAndWatermarks/AddTextStamp.vue";
import AddTilingBackgroundImage from "@/components/09_StampsAndWatermarks/AddTilingBackgroundImage.vue";
import FillStrokeText from "@/components/09_StampsAndWatermarks/FillStrokeText.vue";
import ImageWatermarkFirst from "@/components/09_StampsAndWatermarks/ImageWatermarkFirst.vue";
import ImageWatermarkSecond from "@/components/09_StampsAndWatermarks/ImageWatermarkSecond.vue";
import SetPropertiesForStamp from "@/components/09_StampsAndWatermarks/SetPropertiesForStamp.vue";
import TextWaterMark from "@/components/09_StampsAndWatermarks/TextWaterMark.vue";
import AddDifferentHeaders from "@/components/09_StampsAndWatermarks/HeaderAndFooter/AddDifferentHeaders.vue";
import HeaderAndFooter from "@/components/09_StampsAndWatermarks/HeaderAndFooter/HeaderAndFooter.vue";
import ImageAndPageNumber from "@/components/09_StampsAndWatermarks/HeaderAndFooter/ImageAndPageNumber.vue";
import ImageAndTextUsingTemplate from "@/components/09_StampsAndWatermarks/HeaderAndFooter/ImageAndTextUsingTemplate.vue";
import InlineImageAndPageNumber from "@/components/09_StampsAndWatermarks/HeaderAndFooter/InlineImageAndPageNumber.vue";
import PageNumberInFooter from "@/components/09_StampsAndWatermarks/HeaderAndFooter/PageNumberInFooter.vue";

//10_LinksAndActions
import AddPdfLaunchAction from "@/components/10_LinksAndActions/AddPdfLaunchAction.vue";
import AddTableOfContent from "@/components/10_LinksAndActions/AddTableOfContent.vue";
import DocumentLinkAnnotation from "@/components/10_LinksAndActions/DocumentLinkAnnotation.vue";
import EmbedSoundFile from "@/components/10_LinksAndActions/EmbedSoundFile.vue";
import ExtractAndUpdateLink from "@/components/10_LinksAndActions/ExtractAndUpdateLink.vue";
import FileLinkAnnotation from "@/components/10_LinksAndActions/FileLinkAnnotation.vue";
import GetLinkAnnotation from "@/components/10_LinksAndActions/GetLinkAnnotation.vue";
import GoToAction from "@/components/10_LinksAndActions/GoToAction.vue";
import LaunchFileInNewWindow from "@/components/10_LinksAndActions/LaunchFileInNewWindow.vue";
import Link from "@/components/10_LinksAndActions/Link.vue";
import SpecifyPageToView from "@/components/10_LinksAndActions/SpecifyPageToView.vue";

//11_Conversion
import AddAttachmentsToPDFA from "@/components/11_Conversion/AddAttachmentsToPDFA.vue";
import ConvertToDocxSettingProperties from "@/components/11_Conversion/ConvertToDocxSettingProperties.vue";
import ConvertToGrayPdf from "@/components/11_Conversion/ConvertToGrayPdf.vue";
import ConvertToOFD from "@/components/11_Conversion/ConvertToOFD.vue";
import EncryptedPDFToPDFA from "@/components/11_Conversion/EncryptedPDFToPDFA.vue";
import OFDToPDF from "@/components/11_Conversion/OFDToPDF.vue";
import PDFAToPDF from "@/components/11_Conversion/PDFAToPDF.vue";
import PdfToExcel from "@/components/11_Conversion/PdfToExcel.vue";
import PdfToExcelOptions from "@/components/11_Conversion/PdfToExcelOptions.vue";
import RemoveHyperlinks from "@/components/11_Conversion/RemoveHyperlinks.vue";
import SVGToPDF from "@/components/11_Conversion/SVGToPDF.vue";
import TextToPDF from "@/components/11_Conversion/TextToPDF.vue";
import ToDoc from "@/components/11_Conversion/ToDoc.vue";
import ToDocx from "@/components/11_Conversion/ToDocx.vue";
import ToDocxNew from "@/components/11_Conversion/ToDocxNew.vue";
import ToImage from "@/components/11_Conversion/ToImage.vue";
import ToLinearizedPdf from "@/components/11_Conversion/ToLinearizedPdf.vue";
import ToPCL from "@/components/11_Conversion/ToPCL.vue";
import ToPDFA from "@/components/11_Conversion/ToPDFA.vue";
import ToPdfA2B from "@/components/11_Conversion/ToPdfA2B.vue";
import ToPostScript from "@/components/11_Conversion/ToPostScript.vue";
import ToSVG from "@/components/11_Conversion/ToSVG.vue";
import ToTransparentBackgroundImages from "@/components/11_Conversion/ToTransparentBackgroundImages.vue";
import ToXLSX from "@/components/11_Conversion/ToXLSX.vue";
import ToXPS from "@/components/11_Conversion/ToXPS.vue";
import EmbedImages from "@/components/11_Conversion/PDFToHTML/EmbedImages.vue";
import EmbedSVG from "@/components/11_Conversion/PDFToHTML/EmbedSVG.vue";
import ToHTML from "@/components/11_Conversion/PDFToHTML/ToHTML.vue";
import ToHTMLFilesSplittedByPages from "@/components/11_Conversion/PDFToHTML/ToHTMLFilesSplittedByPages.vue";
import ToHTMLStream from "@/components/11_Conversion/PDFToHTML/ToHTMLStream.vue";

//12_Page
import DeletePage from "@/components/12_Page/DeletePage.vue";
import Extraction from "@/components/12_Page/Extraction.vue";
import GetNumberOfPages from "@/components/12_Page/GetNumberOfPages.vue";
import GetPageInfo from "@/components/12_Page/GetPageInfo.vue";
import GetPageLabel from "@/components/12_Page/GetPageLabel.vue";
import GetPageSize from "@/components/12_Page/GetPageSize.vue";
import InsertEmptyPage from "@/components/12_Page/InsertEmptyPage.vue";
import InsertEmptyPageAtEnd from "@/components/12_Page/InsertEmptyPageAtEnd.vue";
import PageLable from "@/components/12_Page/PageLable.vue";
import PageSetup from "@/components/12_Page/PageSetup.vue";
import Pagination from "@/components/12_Page/Pagination.vue";
import ResetPageSize from "@/components/12_Page/ResetPageSize.vue";
import RotateExistingPDF from "@/components/12_Page/RotateExistingPDF.vue";
import RotateNewPDF from "@/components/12_Page/RotateNewPDF.vue";
import SetPageOrientation from "@/components/12_Page/SetPageOrientation.vue";
import SetTabOrder from "@/components/12_Page/SetTabOrder.vue";
import SplitAPageIntoMultipage from "@/components/12_Page/SplitAPageIntoMultipage.vue";
import SplitFileByParticularPage from "@/components/12_Page/SplitFileByParticularPage.vue";
import ZoomToPageContents from "@/components/12_Page/ZoomToPageContents.vue";

//13_Document
import AddLayers from "@/components/13_Document/AddLayers.vue";
import Booklet from "@/components/13_Document/Booklet.vue";
import ChangePdfVersion from "@/components/13_Document/ChangePdfVersion.vue";
import CompressDocument from "@/components/13_Document/CompressDocument.vue";
import CompressDocumentSecondApproach from "@/components/13_Document/CompressDocumentSecondApproach.vue";
import CreateMultilayerPDF from "@/components/13_Document/CreateMultilayerPDF.vue";
import CreatePDFA1WithSpirePDF from "@/components/13_Document/CreatePDFA1WithSpirePDF.vue";
import CreatePDFPortfolio from "@/components/13_Document/CreatePDFPortfolio.vue";
import CreateTwoColumnPDF from "@/components/13_Document/CreateTwoColumnPDF.vue";
import CustomDocumentProperties from "@/components/13_Document/CustomDocumentProperties.vue";
import DeleteLayer from "@/components/13_Document/DeleteLayer.vue";
import GetDocumentProperties from "@/components/13_Document/GetDocumentProperties.vue";
import GetViewerPreference from "@/components/13_Document/GetViewerPreference.vue";
import GetZoomFactor from "@/components/13_Document/GetZoomFactor.vue";
import InvisibleAllPdfLayers from "@/components/13_Document/InvisibleAllPdfLayers.vue";
import InvisibleParticularPdfLayers from "@/components/13_Document/InvisibleParticularPdfLayers.vue";
import IsPDFPortfolio from "@/components/13_Document/IsPDFPortfolio.vue";
import MergeDocuments from "@/components/13_Document/MergeDocuments.vue";
import MergePdfsByFile from "@/components/13_Document/MergePdfsByFile.vue";
import ModifyPageMargins from "@/components/13_Document/ModifyPageMargins.vue";
import Properties from "@/components/13_Document/Properties.vue";
import RearrangePageOrder from "@/components/13_Document/RearrangePageOrder.vue";
import RemovePageMargins from "@/components/13_Document/RemovePageMargins.vue";
import SetExpiryDate from "@/components/13_Document/SetExpiryDate.vue";
import SetLayerPrintProperties from "@/components/13_Document/SetLayerPrintProperties.vue";
import SetMagnificationToFitHeight from "@/components/13_Document/SetMagnificationToFitHeight.vue";
import SetXMPMetadata from "@/components/13_Document/SetXMPMetadata.vue";
import SetZoomFactor from "@/components/13_Document/SetZoomFactor.vue";
import SplitDocument from "@/components/13_Document/SplitDocument.vue";
import Template from "@/components/13_Document/Template.vue";
import Transitions from "@/components/13_Document/Transitions.vue";
import ViewerPreference from "@/components/13_Document/ViewerPreference.vue";


const filePageId = ref<string>("0");
const handleChildID = (id: any) => {
  filePageId.value = id;
};
</script>

<style lang="scss" scoped>
* {
  padding: 0%;
  margin: 0%;
}
button {
  padding: 5px 10px;
}
.worker_page {
  width: 100%;
  height: 100vh;
  display: flex;
  padding: 2rem;
  .tree_item {
    width: 20%;
    border-right: 1px solid #eeeeee;
  }
  .file_item {
    display: flex;
    flex: 1;
    justify-content: center;
    align-items: center;
    .file-reader div:first-of-type {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 20px;
      span {
        font-size: 18px;
        font-weight: 500;
      }
    }
    .non-centered {
      align-items: flex-start;
    }
  }
}

pre {
  text-align: left;
  background-color: #f8f8f8;
  padding: 10px;
  border: 1px solid #ccc;
}
</style>
