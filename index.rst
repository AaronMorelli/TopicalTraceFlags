.. TopicalTraceFlags documentation master file, created by
   sphinx-quickstart on Wed Nov 16 12:52:33 2016.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

A Topical Collection of SQL Server TraceFlags
=============================================

*Purpose*: A collection of SQL Server trace flags organized by area of the SQL Server engine.
 
*Editor/Primary compiler*: Aaron Morelli
  
*Short Disclaimer*: The majority of these flags are undocumented by Microsoft, and therefore unsupported. This repository cannot vouch for complete 
accuracy in the descriptions provided, nor has authority to do so. The contributors to this repository assumes no responsibility for any 
negative (or positive!) consequences arising from the use or misuse of these trace flags in any production or non-production environment. 
For more information, see the full Disclaimers_ page. Use at your own risk.

*How to use this repo*: Disclaimers_ | Conventions_ | `Flag Search`_ | Contribute_ | `Change Log`_ | License_

*Trace Flag Fun*: `TF Usage`_ | `Other Notable Repositories`_


.. First toc tree, How to Use

.. toctree::
   :maxdepth: 3
   
   core/HowToUse


.. Second toc tree, Categories   
   
.. toctree::
   :maxdepth: 3

   cat/Categories
   

  
  

 
.. Links
.. _Disclaimers: core/Disclaimers.html
.. _Conventions: core/Conventions.html
.. _License: LICENSE
.. _Flag Search: core/FlagSearch.html
.. _Contribute: core/Contribute.html
.. _TF Usage: core/MiscTFUsageNotes.html
.. _Other Notable Repositories: core/OtherRepos.html
.. _Change Log: core/ChangeLog.html

.. _Statistics and Estimation: cat/queries/StatsAndEst.html
.. _Compilation (Informational): cat/queries/CompilationInfo.html
.. _Compilation (Behavioral): cat/queries/CompilationBehavioral.html
.. _Plan Caching: cat/queries/PlanCaching.html
.. _Query Execution: cat/queries/QueryExec.html

.. _Databases and Files: cat/databases/DBsAndFiles.html
.. _Locking and Waiting: cat/databases/LockingAndWaiting.html
.. _Transactions, Logging, and Recovery: cat/databases/TranLoggingRecov.html
.. _Backup and Restore: cat/databases/BackupAndRestore.html
.. _CHECKDB and Corruptions: cat/databases/CHECKDBandCorruptions.html

.. _Instance and Database Startup, General Instance Behavior: cat/server/Inst.html
.. _SQL Miscellaneous Informational: cat/server/MiscInfo.html
.. _SQLOS Scheduling and CPU-related: cat/server/SchedAndCPU.html
.. _SQLOS Memory and Buffer Pool: cat/server/MemAndBuf.html
.. _Disk and Network IO: cat/server/DiskAndNetIO.html
.. _Connectivity: cat/server/Connectivity.html
.. _Background Sessions and Tasks: cat/server/Background.html
.. _High Availability/Distributed Technologies: cat/server/HADR.html
.. _Security: cat/server/Security.html
.. _Special Features: cat/server/SpecialFeatures.html
.. _Exceptions and Memory Dump Behavior: cat/server/ExceptionsAndMemDump.html

.. _Other: cat/other/Other.html
.. _Unable to confirm: cat/other/UnableToConfirm.html
.. _To-Do List: cat/other/TODO.html

   


Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

