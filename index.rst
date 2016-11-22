=============================================
A Topical Collection of SQL Server TraceFlags
=============================================

Welcome to Topical Trace Flags, a collection of SQL Server trace flags organized by area of the SQL engine. The goal of this 
project is to present the full set of publicly-known documented and undocumented trace flags in a usable and intuitive way. The 
content is always a work-in-progress and your contributions are always welcome! Please review the disclaimers and conventions below 
and the navigational bar to the left to familiarize yourself with the collection's limitations and it's organization. Thanks for dropping by!

*Editor/Primary compiler*: Aaron Morelli

*License*: `Topical Trace Flags`_ is licensed under the MIT license.
  
*Short Disclaimer*: The majority of these flags are undocumented by Microsoft, and therefore unsupported. This repository has neither the 
authority nor ability to vouch for complete accuracy in the descriptions provided. The contributors to this repository assumes no responsibility 
for any negative (or positive!) consequences arising from the use or misuse of these trace flags in any production or non-production environment. 
For more information, see the full :doc:`Disclaimers <core/Disclaimers>` page. Use at your own risk.

Flags are organized into 3 main parent categories (and an "other" category), with specific pages for each subject area within a category. 
Each page contains a brief description of that subject area, followed by a table with short descriptions of the flags. This table is 
followed by more verbose descriptions for each flag, including important links. For a full description of the conventions of this repository, 
see the :doc:`Conventions <core/Conventions>` page.


.. toctree::
   :caption: How to Use
   :hidden:

   core/Disclaimers
   core/Conventions
   core/TFUsageNotes
   core/OtherRepos
   core/Contribute
   core/ChangeLog
   
.. toctree::
   :caption: Queries
   :hidden:

   cat/qry_StatsAndEst
   cat/qry_CompInfo
   cat/qry_CompBehav
   cat/qry_QueryExec
   cat/qry_PlanCache

   
.. toctree::
   :caption: Databases
   :hidden:

   cat/db_DBsAndFiles
   cat/db_LockAndWait
   cat/db_TranLogRecov
   cat/db_BakAndRes
   cat/db_CHECKCorrupt

   
.. toctree::
   :caption: Server
   :hidden:

   cat/svr_Inst
   cat/svr_SchedAndCPU
   cat/svr_MemAndBuf
   cat/svr_DiskAndNetIO
   cat/svr_Background
   cat/svr_Security
   cat/svr_Connectivity
   cat/svr_HADR
   cat/svr_SpecialFeatures
   cat/svr_ExceptDump
   cat/svr_MiscInfo
   
   
.. toctree::
   :caption: Other
   :hidden:

   cat/oth_UnableToConf
   cat/oth_OtherStuff
   cat/oth_TODO



.. Links 


.. _Topical Trace Flags : https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/LICENSE.rst
