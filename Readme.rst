===================
Topical Trace Flags
===================

*Purpose*: A collection of SQL Server trace flags organized by area of the SQL Server engine.
 
*Editor/Primary compiler*: Aaron Morelli
  
*Short Disclaimer*: The majority of these flags are undocumented by Microsoft, and therefore unsupported. This repository cannot vouch for complete 
accuracy in the descriptions provided, nor has authority to do so. The contributors to this repository assumes no responsibility for any 
negative (or positive!) consequences arising from the use or misuse of these trace flags in any production or non-production environment. 
For more information, see the full Disclaimers_ page. Use at your own risk.


*How to use this repo*: Disclaimers_ | Conventions_ | License_ | Search_ | Contribute_ | `Change Log`_

*Trace Flag Fun*: `TF Usage`_ | `Other Notable Repositories`_

Categories
----------

**Queries**
  #. `Statistics and Estimation`_
  #. `Compilation (Informational)`_
  #. `Compilation (Behavioral)`_
  #. `Plan Caching`_
  #. `Query Execution`_

**Databases**
  #. `Databases and Files`_
  #. `Locking and Waiting`_
  #. `Transactions, Logging, and Recovery`_
  #. `Backup and Restore`_
  #. `CHECKDB and Corruptions`_

**Server**
  #. `Instance and Database Startup, General Instance Behavior`_
  #. `SQL Miscellaneous Informational`_
  #. `SQLOS Scheduling and CPU-related`_
  #. `SQLOS Memory and Buffer Pool`_
  #. `Disk and Network IO`_
  #. `Connectivity`_
  #. `Background Sessions and Tasks`_
  #. `High Availability/Distributed Technologies`_
  #. `Security`_
  #. `Special Features`_
  #. `Exceptions and Memory Dump Behavior`_
  
**Other**
  #. `Other`_
  #. `Unable to confirm`_
  #. `To-Do List`_
  
  

 
.. Links
.. _Disclaimers: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/core/Disclaimers.rst
.. _Conventions: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/core/Conventions.rst
.. _License: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/LICENSE
.. _Search: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/core/Search.rst
.. _Contribute: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/core/Contribute.rst
.. _TF Usage: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/core/MiscTFUsageNotes.rst
.. _Other Notable Repositories: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/core/OtherRepos.rst
.. _Change Log: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/core/ChangeLog.rst

.. _Statistics and Estimation: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/cat/queries/StatsAndEst.rst
.. _Compilation (Informational): https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/cat/queries/CompilationInfo.rst
.. _Compilation (Behavioral): https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/cat/queries/CompilationBehavioral.rst
.. _Plan Caching: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/cat/queries/PlanCaching.rst
.. _Query Execution: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/cat/queries/QueryExec.rst

.. _Databases and Files: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/cat/databases/DBsAndFiles.rst
.. _Locking and Waiting: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/cat/databases/LockingAndWaiting.rst
.. _Transactions, Logging, and Recovery: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/cat/databases/TranLoggingRecov.rst
.. _Backup and Restore: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/cat/databases/BackupAndRestore.rst
.. _CHECKDB and Corruptions: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/cat/databases/CHECKDBandCorruptions.rst

.. _Instance and Database Startup, General Instance Behavior: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/cat/server/Inst.rst
.. _SQL Miscellaneous Informational: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/cat/server/MiscInfo.rst
.. _SQLOS Scheduling and CPU-related: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/cat/server/SchedAndCPU.rst
.. _SQLOS Memory and Buffer Pool: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/cat/server/MemAndBuf.rst
.. _Disk and Network IO: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/cat/server/DiskAndNetIO.rst
.. _Connectivity: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/cat/server/Connectivity.rst
.. _Background Sessions and Tasks: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/cat/server/Background.rst
.. _High Availability/Distributed Technologies: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/cat/server/HADR.rst
.. _Security: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/cat/server/Security.rst
.. _Special Features: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/cat/server/SpecialFeatures.rst
.. _Exceptions and Memory Dump Behavior: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/cat/server/ExceptionsAndMemDump.rst

.. _Other: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/cat/other/Other.rst
.. _Unable to confirm: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/cat/other/UnableToConfirm.rst
.. _To-Do List: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/cat/other/TODO.rst
