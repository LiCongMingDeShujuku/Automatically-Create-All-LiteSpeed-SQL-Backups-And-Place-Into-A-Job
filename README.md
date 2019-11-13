![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# 自动创建所有LiteSpeed SQL备份并放入一个作业
### Automatically Create All LiteSpeed SQL Backups And Place Into A Job
****发布-日期: 2014年05月05日 (评论)*


## Contents

- [中文](#中文)
- [English](#English)
- [SQL Logic](#Logic)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
如果你使用LiteSpeed进行SQL备份，这里有一些非常棒的备份逻辑。

0，配置SQL Server以对所有备份使用“备份压缩”。
1，根据Server名称（或instance名称），数据库名称等自动在备份共享上创建文件夹结构。
2，在备份文件名中创建自动TimeStamp。
3,，将“.bkp”备份扩展名添加到文件中，以便在标准“.bak”和“.bkp”之间可以快速识别。



## English
Here’s some really great backup logic if you are using LiteSpeed for your SQL backups. This does a number of things.
0. Configures SQL Server to use Backup Compression for all backups.
1. Automatically creates the folder structure on your Backup Share based on server_name(or instance name), DatabaseName, etc. 2. Creates an automatic TimeStamp in the backup file name.
3. Adds the “.bkp” backup extension to the file to readily identify between standard ‘.bak’, and ‘.bkp’.
All you need to do is put this into an SQL Backup Job, and it automatically creates the rest of your backup folder structure under the backup share you specify. Ensure that the Agent Service Account has full rights to your backup share for this to work.


---
## Logic
```SQL

use master;
 
set quoted_identifier off
set nocount on
 
declare @sql_instance 				varchar(255)
declare @server_name 				varchar(255)
declare @full_backup 				varchar(255)
declare @log_backup 				varchar(255)
declare @diff_backup 				varchar(255)
declare @backup_time_stamp 			varchar(255)
declare @backup_location 			varchar(255) 
declare @create_folder_server_name 	varchar(max) 
declare @create_folder_backup_type 	varchar(max) 
declare @backup_database 			varchar(max)
 
set @full_backup 	= 'Full'
set @log_backup 	= 'Log'
set @diff_backup 	= 'Diff'
set @server_name 	= ( select @@server_name )
set @sql_instance 	= ( select left(@@server_name, charindex('\', @@server_name) +50) )
set @backup_time_stamp = ( select replace(replace(replace(replace(replace(convert(varchar,getdate(), 9),' ',' '),':','-'),' ',' ' ), 'pm', ' pm'), 'am', ' am') ) set @backup_location = '\\MyBackupShare\MyBackupFolder\' set @backup_database = ''
exec master..sp_configure 'show advanced options',	'1' reconfigure
exec master..sp_configure 'xp_cmdshell', 			'1' reconfigure

```

[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")

