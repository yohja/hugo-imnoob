---
title: Automating Power BI Report and Data Source Retrieval with PowerShell
date: 2023-07-04T05:02:46.335Z
draft: false
categories: Power BI
author: YohJawa
comments: true
share: true
---
#### Introduction

* Briefly introduce the purpose of the blog post.
* Highlight the importance of automating tasks for Power BI administrators.
* Mention that PowerShell can be a powerful tool for managing Power BI Report Servers.

#### Prerequisites

* Outline the prerequisites for running the PowerShell script:

  * PowerShell installed on the machine.
  * Appropriate permissions to access the Power BI Report Server.
  * Knowledge of the Power BI Report Server URL.
  * [R﻿eportingServicesTools](https://github.com/microsoft/ReportingServicesTools) PowerShell module installed on the machine

#### Connecting to the Power BI Report Server

* Defining parameters needed for further execution of this script

```powershell
$rsUri = "http://localhost/reports"
$filter = "text-to-filter-from-report-name"
$rsFolder = "/folder-contain-pbi-reports"

#Importing ReportingServicesTools module
Import-Module ReportingServicesTools
```

#### Retrieving Power BI Reports

* Retrieve all Power BI Reports from report server URL and report folder defined in the previous step

  ```powershell
  $rptLists = Get-RsRestFolderContent -ReportPortalUri $rsUri -RsFolder = $rsFolder -Recurse | Where-Object {$_.Type -wq "PowerBIReport"}
  ```



#### Extracting Data Sources for Each Report

* Loop through each Power BI Reports and extracting their data sources.

  ```powershell
  $rptDs = ForEach ($rpt in $rptLists) {
    Get-RsRestItemRestItemDataSource -ReportPortalUri $rsUri -RsItem $rpt.Path
  }

  ```

  c﻿

#### Outputting the Results

* Export the result to CSV files

  ```
  #select specific field to export
  $rptDs | Select Name, Path, DataModelDataSource | Where {$_.DataModelDataSource -ne Null}
  $rptDs | Export-Csv $csvOutput -NoTypeInformation
  ```

#### Conclusion

* Summarize the key points covered in the blog post.
* Encourage readers to use and customize the provided script based on their specific needs.
* Mention the importance of automation in Power BI administration.

#### Additional Tips and Resources

* Provide additional tips or considerations for enhancing the script.
* Include links to official Power BI documentation and relevant resources.

By following this outline, you can create a comprehensive blog post that guides readers through the process of automating the retrieval of Power BI reports and their data sources from a Power BI Report Server using PowerShell.