---
title: Why the request to "/api/reports/clients/{clientId}/instances/{instanceId}/documents/{documentId} is not authorized"
description: Vulnerability tests result in "Sensitive Information Disclosure via API Response", "Internal Path Disclosure" or similar on "/api/reports/clients/{clientId}/instances/{instanceId}/documents/{documentId}"
type: troubleshooting
page_title: Unauthorized request "/api/reports/clients/{clientId}/instances/{instanceId}/documents/{documentId}"
slug: why-get-document-request-is-unauthorized
position: 
tags: 
ticketid: 1414290
res_type: kb
---

## Environment
<table>
    <tbody>
	    <tr>
	    	<td>Product</td>
	    	<td>Progress® Telerik® Reporting</td>
	    </tr>
    </tbody>
</table>


## Description
When performing security tests with automated tools, there may be indications of vulnerability. For example, if an authorized user discloses the link of reporting API to an unauthorized user, e.g "/api/reports/clients/031206-6569/instances/031209-532c/documents/031210-54b5031210-5509/", the unauthorized user will be able to view the report.


## Reasoning
The [Get Document]({% slug telerikreporting/using-reports-in-applications/host-the-report-engine-remotely/telerik-reporting-rest-services/rest-api-reference/documents-api/get-document %}) request (e.g. "/api/reports/clients/031206-6569/instances/031209-532c/documents/031210-54b5031210-5509") is performed to download the generated report document as an attachment (e.g. PDF file). Importantly, the __JavaScript doesn't have access to the client machine file system__, so it is necessary to rely on the browser to actually download the file, e.g. to open the File Save dialog. Therefore, the Html5 Report Viewer downloads the rendered reports through the [window.open() method](https://developer.mozilla.org/en-US/docs/Web/API/Window/open). However, there are no options to add headers as this is not an AJAX request, hence if the endpoint was secured, it was not going to be possible to open/download the generated report through the viewer.

## Approaches
1. You may authorize the corresponding method in our API (check the [`GetDocument` virtual method](https://docs.telerik.com/reporting/api/Telerik.Reporting.Services.WebApi.ReportsControllerBase.html#Telerik_Reporting_Services_WebApi_ReportsControllerBase_GetDocument_System_String_System_String_System_String_)) by overriding it. In this case, you will need to take care of the authorization headers that should be passed from the client when requesting the prepared document. This will also disable the export usage for all report viewer controls that rely on the [Telerik Reporting REST Service]({% slug telerikreporting/using-reports-in-applications/host-the-report-engine-remotely/telerik-reporting-rest-services/overview %}). 

2. A workaround may be found in the [How to download a file from an authenticated Web API endpoint](https://royaljay.com/development/how-to-download-files-from-authenticated-web-api-endpoints/) post. Note that this (and probably any other solution with the current technologies) will deteriorate the user experience, and therefore, we have not adopted it.

3. Instead of using the above approaches, we have decided to leave the request unauthorized and to rely on the fact that the URL for the request has three random auto-generated GUIDs that are practically impossible to guess. The person that is authorized to open reports should be responsible for not sharing the links to sensitive documents with unauthorized users. Note that even if the _Get Document_ request was secured, once the document was downloaded it could be distributed by the one who downloaded it, hence it is all in the hands of the authorized user.
