---
title: Word Device Information Settings
page_title: Word Device Information Settings 
description: Word Device Information Settings
slug: telerikreporting/using-reports-in-applications/export-and-configure/configure-the-export-formats/word-device-information-settings
tags: word,device,information,settings
published: True
position: 12
previous_url: /device-information-settings-word
---
<style>
table th:first-of-type {
    width: 15%;
}
table th:nth-of-type(2) {
    width: 10%;
}
table th:nth-of-type(3) {
    width: 75%;
}
</style>

# Word Device Information Settings

The following table lists the device information settings for rendering in Word (DOCX) format.

## Word Device Information Settings

|__Name__|__Type__|__Description__|
| ------ | ------ | ------ |
|StartPage|Integer|The first page of the report to render. A value of __0__ indicates that all pages are rendered.|
|EndPage|Integer|The last page of the report to render.|
|UseMetafile|Boolean|A flag specifying whether to render Graph, Map and Barcode items as [Metafile (EMF)](http://msdn.microsoft.com/en-us/library/windows/desktop/ms536391(v=vs.85).aspx) or [Bitmap](http://msdn.microsoft.com/en-us/library/windows/desktop/ms536393(v=vs.85).aspx) images. The default value is __true__.|

For an example of how to set up the settings for a rendering extension, see [extensions Element]({%slug telerikreporting/using-reports-in-applications/export-and-configure/configure-the-report-engine/extensions-element%}). 

## See Also

* [Device Information Settings]({%slug telerikreporting/using-reports-in-applications/export-and-configure/configure-the-export-formats/overview%})

* [Export Formats]({%slug telerikreporting/using-reports-in-applications/export-and-configure/export-formats%})
