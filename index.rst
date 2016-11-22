.. TopicalTraceFlags documentation master file, created by
   sphinx-quickstart on Wed Nov 16 12:52:33 2016.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

A Topical Collection of SQL Server TraceFlags
=============================================

Welcome to a topically-organized collection of SQL Server trace flags. This documentation aims to be the most usable, intuitive, 
and closest-to-comprehensive collection of trace flags anywhere on the internet. The content is always a work-in-progress
and your contributions are always welcome! Please review the content below and the navigational bar to the left to familiarize
yourself with the collection's conventions, limitations, and organization. Thanks for dropping by!

*Editor/Primary compiler*: Aaron Morelli

*License*: MIT License
  
*Short Disclaimer*: The majority of these flags are undocumented by Microsoft, and therefore unsupported. This repository cannot vouch for complete 
accuracy in the descriptions provided, nor has authority to do so. The contributors to this repository assumes no responsibility for any 
negative (or positive!) consequences arising from the use or misuse of these trace flags in any production or non-production environment. 
For more information, see the full Disclaimers_ page. Use at your own risk.


.. toctree::
   :caption: How to Use

   core/Disclaimers
   core/Conventions
   core/TFUsageNotes
   core/OtherRepos
   core/FlagSearch
   core/Contribute
   core/ChangeLog
   core/rest_basics
   core/rest_advanced
   core/rest_sphinx

   
.. toctree::
   :caption: Queries

   cat/queries__StatsAndEst
   cat/queries__CompilationInfo
   cat/queries__CompilationBehavioral
   cat/queries__QueryExec
   cat/queries__PlanCaching

   
.. toctree::
   :caption: Databases

   cat/dbs__DBsAndFiles
   cat/dbs__LockingAndWaiting
   cat/dbs__TranLoggingRecov
   cat/dbs__BackupAndRestore
   cat/dbs__CHECKDBandCorruptions

   
.. toctree::
   :caption: Server

   cat/server__Inst
   cat/server__SchedAndCPU
   cat/server__MemAndBuf
   cat/server__DiskAndNetIO
   cat/server__Background
   cat/server__Security
   cat/server__Connectivity
   cat/server__HADR
   cat/server__SpecialFeatures
   cat/server__ExceptionsAndMemDump
   cat/server__MiscInfo
   
   
.. toctree::
   :caption: Other

   cat/other__UnableToConfirm
   cat/other__OtherStuff
   cat/other__TODO

   


Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

