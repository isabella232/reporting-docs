---
title: Error registering the viewer with the service. Access to the path X is denied.
description: After deploying there is an error Access to the path 'C:\Windows\TEMP\<NAME>\<VERSION>\LCT\value.dat' is denied.
type: troubleshooting
page_title: Error registering the viewer with the service. Access to the path X is denied.
slug: error-registering-the-viewer-with-the-service
position: 
tags: 
ticketid: 1165109
res_type: kb
---

## Environment
<table>
	<tr>
		<td>Product</td>
		<td>Progress® Telerik® Reporting </td>
	</tr>
	<tr>
		<td>Viewers</td>
		<td>All</td>
	</tr>
</table>


## Error Message
```
Error registering the viewer with the service. 
An error has occurred. 
Access to the path 'C:\Windows\TEMP\\**NAME**\\**VERSION**\LCT\value.dat' is denied.
```

Where **NAME** is the name of the application and **VERSION** is the currently installed on the machine version of the product

## Cause\Possible Cause(s)
The error message "Access to the path X is denied" indicates that the [Telerik Reporting REST service]({% slug telerikreporting/using-reports-in-applications/host-the-report-engine-remotely/telerik-reporting-rest-services/overview %}) cannot access the configured file storage. By default, user temp folder will be used as a storage which is *'C:\Windows\TEMP'* in this case. 

## Solution
Test changing the default folder using the [second overload of the FileStorage](https://docs.telerik.com/reporting/api/Telerik.Reporting.Cache.File.FileStorage.html#collapsible-Telerik_Reporting_Cache_File_FileStorage__ctor_System_String_) and passing the custom folder location. The changes need to be made in ReportsController.cs file where the settings of the Reporting REST service are defined.

Example of ReporsController which connects the REST Service and HTML5 Report Viewer:
```CSharp
  public class ReportsController : ReportsControllerBase
    {
        static ReportServiceConfiguration configurationInstance;

        static ReportsController()
        {
            var appPath = HttpContext.Current.Server.MapPath("~/");
            var reportsPath = Path.Combine(appPath, "Reports");
			
            var resolver = new ReportFileResolver(reportsPath)
                .AddFallbackResolver(new ReportTypeResolver());

	    //Setup the ReportServiceConfiguration
            configurationInstance = new ReportServiceConfiguration
            {
                HostAppId = "Html5App",
                Storage = new FileStorage("C:\MyFolder"),
                ReportResolver = resolver,
            };
        }

        public ReportsController()
        {
            this.ReportServiceConfiguration = configurationInstance;
        }
    }
```

Make sure that the IIS process has read/write access for the folder used by the storage. To give access to the folder go to Properties - Sharing|Security options or change the used application pool's Identity through IIS Manager.

In case of different instances of the application running, set a unique **HostAppId** for each instance of the Reporting REST Service. Another approach is using MsSqlStorage or Redis, which are suitable for multiple instance environment. The settings must be added in the ReportsController.cs.
The appropriate cache storage options should be considered depending on the hosting environment. Please check the available options in [REST Service Storage](../telerik-reporting-rest-service-storage).
