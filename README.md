![MIKES DATA WORK GIT REPO](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_01.png "Mikes Data Work")        

# Configure SSIS for multiple SQL Instances
**Post Date: September 14, 2016**        



## Contents    
- [About Process](##About-Process)  
- [ini File](#ini-file)  
- [Author](#Author)  
- [License](#License)       

## About-Process

<p>In this example we have a generic database server with SQL Server 2014. There are 6 instances.</p>

DEFAULT
SQLSHARE01
SQLSHARE02
SQLSHARE03
SQLSHARE04
SQLSHARE05

Now; according to SQL 2014; you are still unable to install SSIS on a Multi-Instance SQL Database Server. Ok this is all well and good, but most of the time you'll have least some databases across the instances which requires the SSIS Service to handle it's ETL automations.

Ok; so what do you do in this case?
It's no problem to have a single SSIS install which drives the SSIS processes across all other instances, but how do you manage the SSIS packages in each MSDB?
The classic answer to this ( which is still current today as it was in former SQL versions ) is to edit the config file for SSIS. In my case it's located here:
D:Program FilesMicrosoft SQL Server120DTSBinn

![Check SSIS Config File]( https://mikesdatawork.files.wordpress.com/2016/09/image001.png "Check SSIS Config File")
 
The actual file name is called this: MsDtsSrvr.ini You'll notice it's basically an XML file with extension .ini
In any event; here's what you'll need to edit:



## ini-File
```SQL
<?xml version="1.0" encoding="utf-8"?>
<DtsServiceConfiguration xmlns:xsd="<a href="http://www.w3.org/2001/XMLSchema"">http://www.w3.org/2001/XMLSchema"</a> xmlns:xsi="<a href="http://www.w3.org/2001/XMLSchema-instance">">http://www.w3.org/2001/XMLSchema-instance"></a>
<StopExecutingPackagesOnShutdown>true</StopExecutingPackagesOnShutdown>
<TopLevelFolders>
<Folder xsi:type="SqlServerFolder">
<Name>MSDB</Name>
<ServerName>.</ServerName>
</Folder>
<Folder xsi:type="FileSystemFolder">
<Name>File System</Name>
<StorePath>..Packages</StorePath>
</Folder>
</TopLevelFolders>
</DtsServiceConfiguration>
```
So what do you need to edit exactly? Just take the following lines… Copy & Paste, and edit them for each instance you want to manage.


## ini-File
```SQL
<Folder xsi:type="SqlServerFolder">
 
<Name>MSDB MyInstanceName</Name>
<ServerName>. MyInstanceName</ServerName>
 
</Folder>
```
As you can see I just used MSDB SQLSHARE01, …02, …03 etc. This perfectly denotes where to find the MSDB database packages in the other instances.
Then simply paste it back into the original script like the following: ( I gave additional edits to represent every instance in my server ).

![Copy Edit SSIS Instances]( https://mikesdatawork.files.wordpress.com/2016/09/image002.png "Copy Edit SSIS Instances")
 
Once you do this; you'll be able to manage each package set from each database instance. This is still classified as a work around by the way. It's not the end-all-be-all solution until Microsoft makes each instance capable have carrying it's own SSIS Service independent of the other.
You'll see the folders for each MSDB under the b-tree folder structure under the SSIS portion of Management Studio.
Anyway; hope this is helpful. 


[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

[![Gist](https://img.shields.io/badge/Gist-MikesDataWork-<COLOR>.svg)](https://gist.github.com/mikesdatawork)
[![Twitter](https://img.shields.io/badge/Twitter-MikesDataWork-<COLOR>.svg)](https://twitter.com/mikesdatawork)
[![Wordpress](https://img.shields.io/badge/Wordpress-MikesDataWork-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)
     
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Mikes Data Work](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_02.png "Mikes Data Work")

