---
title: How to Add Telerik Reporting REST ServiceStack to Web Application
page_title: How to Add Telerik Reporting REST ServiceStack to Web Application 
description: How to Add Telerik Reporting REST ServiceStack to Web Application
slug: telerikreporting/using-reports-in-applications/host-the-report-engine-remotely/telerik-reporting-rest-services/servicestack-implementation/how-to-add-telerik-reporting-rest-servicestack-to-web-application
tags: how,to,add,telerik,reporting,rest,servicestack,to,web,application
published: True
position: 2
previous_url: /telerik-reporting-rest-servicestack-hosting-iis
---

# How to Add Telerik Reporting REST ServiceStack to Web Application

This article describes the steps required to host the __Telerik Reporting ServiceStack REST Service__ implementation on top of the classic __ASP.NET__ hosting infrastructure supported by the __IIS__ (Internet Information Services) server. 

> Telerik Reporting ServiceStack assembly requires __V3__ of the ServiceStack framework. 

## How to host the ServiceStack implementation of Telerik Reporting REST service in IIS:

1. Create a new __ASP.NET Empty Web Application__. 

1. Install the [ServiceStack 3.9.70.0](https://www.nuget.org/packages/ServiceStack/3.9.70) NuGet package. 

1. Add references to the following Telerik Reporting assemblies (required): 

	+ Telerik.Reporting.dll

	+ Telerik.Reporting.Services.ServiceStack.dll

1. Add references to the following Telerik Reporting assemblies (optional): 

	+ Telerik.Reporting.Cache.Database.dll - only if [DatabaseStorage](/reporting/api/Telerik.Reporting.Cache.Database.DatabaseStorage) caching mechanism is intended. For more details check [Reporting REST Service Storage]({%slug telerikreporting/using-reports-in-applications/host-the-report-engine-remotely/telerik-reporting-rest-services/rest-service-storage/overview%}). The assembly has dependencies on Telerik Data Access which can be checked in the version corresponding [Upgrade article]({%slug telerikreporting/upgrade/overview%}); 

	+ Telerik.Reporting.OpenXmlRendering.dll - depends on [Third-Party Dependencies]({%slug telerikreporting/using-reports-in-applications/third-party-dependencies%}). Required if you need to export in OpenXML formats (DOCX, PPTX, XLSX); 

	+ Telerik.Reporting.XpsRendering.dll - required if you need to export in XPS format; 

	+ Telerik.Reporting.Adomd.dll - required if you use [CubeDataSource]({%slug telerikreporting/designing-reports/connecting-to-data/data-source-components/cubedatasource-component/overview%}) components in reports. The assembly has dependencies on *Microsoft.AnalysisServices.AdomdClient.dll* v.10.0.0.0 or [above with proper binding redirects]({%slug telerikreporting/designing-reports/connecting-to-data/data-source-components/cubedatasource-component/configuring-your-project-for-using-microsoft-analysis-services%}); 

1. Create a new class which derives from [ReportsHostBase](/reporting/api/Telerik.Reporting.Services.ServiceStack.ReportsHostBase). It could be called *ReportsHost* for example: 

	* Set the [ReportServiceConfiguration](/reporting/api/Telerik.Reporting.Services.ServiceStack.ReportsHostBase#Telerik_Reporting_Services_ServiceStack_ReportsHostBase_ReportServiceConfiguration) property. The __ReportSourceResolver__ and __Storage__ configuration settings are required. See the [IReportServiceConfiguration](/reporting/api/Telerik.Reporting.Services.IReportServiceConfiguration) interface for more details. 

		Here is a sample implementation with the setup:             

		{{source=CodeSnippets\MvcCS\ServiceStack\ReportsHost.cs region=ReportsHost_Implementation}}
		{{source=CodeSnippets\MvcVB\ServiceStack\ReportsHost.vb region=ReportsHost_Implementation}}


		The provided sample implementation will resolve .TRDP|.TRDX report definitions from the *Reports* subfolder of the hosting ASP.NET application root. Another option is to reference a reports library and provide report [type assembly qualified name](http://msdn.microsoft.com/en-us/library/system.type.assemblyqualifiedname.aspx) from the service clients. 

		>warning Do not forget to add all necessary (i.e., referred from the report definitions) connection strings to the application configuration file. 

		>tip The above implementation uses the [FileStorage](/reporting/api/Telerik.Reporting.Cache.File.FileStorage) method in order to create a cache object instance. All Visual Studio item templates for adding the Reporting REST service use the default __FileStorage__ constructor. The second overload of the FileStorage constructor allows you to specify a folder, and it is recommended for usage in production environment. 

	* Alternatively, you may __configure the Telerik Reporting REST service from the application configuration file__. 

		If you prefer this approach, set the value of the [ReportServiceConfiguration](/reporting/api/Telerik.Reporting.Services.ServiceStack.ReportsHostBase#Telerik_Reporting_Services_ServiceStack_ReportsHostBase_ReportServiceConfiguration) property to an instance of the [ConfigSectionReportServiceConfiguration](/reporting/api/Telerik.Reporting.Services.ConfigSectionReportServiceConfiguration) class. 

		{{source=CodeSnippets\MvcCS\ServiceStack\ReportsHostConfigSection.cs region=ReportsHostConfigSectionImplementation}}
		{{source=CodeSnippets\MvcVB\ServiceStack\ReportsHostConfigSection.vb region=ReportsHostConfigSectionImplementation}}


		Then add the __restReportService__ configuration element containing the service settings to the [Telerik Reporting Configuration Section]({%slug telerikreporting/using-reports-in-applications/export-and-configure/configure-the-report-engine/overview%}). 

		{{source=CodeSnippets\MvcCS\ReportServiceConfigurationSnippets\ConfigSectionConfiguration.xml}}


		For more information see [restReportService Element]({%slug telerikreporting/using-reports-in-applications/export-and-configure/configure-the-report-engine/restreportservice-element%}). 

1. Add a new or use the existing __Global Application Class__ (global.asax) to create and initialize the ServiceStack reports service in the __Application_Start__ method: 

	{{source=CodeSnippets\MvcCS\ServiceStack\Application.cs region=ServiceStack_Application_Start}}
	{{source=CodeSnippets\MvcVB\ServiceStack\Application.vb region=ServiceStack_Application_Start}}


1. Update the configuration file (*web.config*) to include the following *location* element: 

	````XML
<configuration>
	 <location path="api">
		<system.web>
		 <compilation debug="true" targetFramework="4.0" />
		 <httpHandlers>
		   <add path="*" type="ServiceStack.WebHost.Endpoints.ServiceStackHttpHandlerFactory, ServiceStack" verb="*"/>
		 </httpHandlers>
		</system.web>
		<system.webServer>
		 <modules runAllManagedModulesForAllRequests="true"/>
		 <validation validateIntegratedModeConfiguration="false"/>
		 <handlers>
		   <add path="*" name="ServiceStack.Factory" type="ServiceStack.WebHost.Endpoints.ServiceStackHttpHandlerFactory, ServiceStack" verb="*" preCondition="integratedMode" resourceType="Unspecified" allowPathInfo="true" />
		 </handlers>
		</system.webServer>
	 </location>
	</configuration>
````


1. To verify whether the service works correctly you can make a sample request for the available document formats using the following url: 

	`http://localhost: [portnumber]/api/reports/formats`

	If the request is successful you should receive the document formats encoded in JSON. For more information see: [Get Available Document Formats]({%slug telerikreporting/using-reports-in-applications/host-the-report-engine-remotely/telerik-reporting-rest-services/rest-api-reference/general-api/get-available-document-formats%}). 

## See Also
 
* [UriReportSourceResolver](/reporting/api/Telerik.Reporting.Services.UriReportSourceResolver)  

* [TypeReportSourceResolver](/reporting/api/Telerik.Reporting.Services.TypeReportSourceResolver) 

* [HTML5 Report Viewer]({%slug telerikreporting/using-reports-in-applications/display-reports-in-applications/web-application/html5-report-viewer/overview%})

* [Overview]({%slug telerikreporting/using-reports-in-applications/host-the-report-engine-remotely/telerik-reporting-rest-services/rest-service-report-source-resolver/overview%})

* [Manual Setup]({%slug telerikreporting/using-reports-in-applications/display-reports-in-applications/web-application/html5-report-viewer/manual-setup%})
