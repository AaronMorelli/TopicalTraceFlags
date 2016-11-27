# A Topical Collection of SQL Server TraceFlags

(Note: This repo is built into HTML pages hosted on ReadTheDocs, and is intended to be viewed 
[here](http://topicaltraceflags.readthedocs.io/en/latest/).)

Welcome to Topical Trace Flags, a collection of SQL Server trace flags organized by area of the SQL engine. The goal of this project 
is to present the full set of publicly-known documented and undocumented trace flags in a usable and intuitive way. The content is 
always a work-in-progress and your contributions are always welcome! Please review the disclaimers, conventions, and table of contents 
below to familiarize yourself with the collection's limitations and it's organization. Thanks for dropping by!

*Editor/Primary compiler*: Aaron Morelli

*License*: Topical Trace Flags is licensed under the [MIT license](LICENSE.rst).
  
*Short Disclaimer*: The majority of these flags are undocumented by Microsoft, and therefore unsupported. This repository has neither the 
authority nor ability to vouch for complete accuracy in the descriptions provided. The contributors to this repository assumes no responsibility 
for any negative (or positive!) consequences arising from the use or misuse of these trace flags in any production or non-production environment. 
For more information, see the full [Disclaimers](Disclaimers.rst) page. 
Use at your own risk.

Flags are organized into 3 main parent categories (and an "other" category), with specific pages for each subject area within a category. 
Each page contains a brief description of that subject area, followed by a table with short descriptions of the flags. This table is 
followed by more verbose descriptions for each flag, including important links. For a full description of the conventions of this repository, 
see the [Conventions](Conventions.rst) page.

## Table of Contents

_How to Use_

 - [Disclaimers](core/Disclaimers.rst)
 - [Conventions](core/Conventions.rst)
 - [Using Trace Flags](core/TFUsageNotes.rst)
 - [Other Important Repositories](core/OtherRepos.rst)
 - [How to Contribute](core/Contribute.rst)
 - [Change Log](core/ChangeLog.rst)
   
_Queries_
 - [Statistics and Estimation](cat/qry_StatsAndEst.rst)
 - [Compilation Informational](cat/qry_CompInfo.rst)
 - [Compilation Behavioral](cat/qry_CompBehav.rst)
 - [Query Execution](cat/qry_QueryExec.rst)
 - [Plan Caching](cat/qry_PlanCache.rst)
   
_Databases_

 - [Databases and Files](cat/db_DBsAndFiles.rst)
 - [Locking and Waiting](cat/db_LockAndWait.rst)
 - [Transactions, Logging, and Recovery](cat/db_TranLogRecov.rst)
 - [Backup And Restore](cat/db_BakAndRes.rst)
 - [CHECKDB And Corruption](cat/db_CHECKCorrupt.rst)
   
_Server_

 - [General Instance-level](cat/svr_Inst.rst)
 - [SQLOS Scheduling and CPU](cat/svr_SchedAndCPU.rst)
 - [SQLOS Memory and Buffer Pool](cat/svr_MemAndBuf.rst)
 - [Disk and Network IO](cat/svr_DiskAndNetIO.rst)
 - [Background Tasks](cat/svr_Background.rst)
 - [Security](cat/svr_Security.rst)
 - [Connectivity](cat/svr_Connectivity.rst)
 - [HADR / Distributed Technologies](vsvr_HADR.rst)
 - [Special Features](cat/svr_SpecialFeatures.rst)
 - [Exceptions and Memory Dumping](cat/svr_ExceptDump.rst)
 - [Miscellaneous Info](cat/svr_MiscInfo.rst)
   
_Other_

 - [Unable To Confirm](cat/oth_UnableToConf.rst)
 - [Other Stuff](cat/oth_OtherStuff.rst)
 - [To Do](cat/oth_TODO.rst)


