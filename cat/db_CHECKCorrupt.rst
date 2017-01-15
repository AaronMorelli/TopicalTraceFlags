======================
CHECKDB and Corruption
======================

Flags in this category fall into the below groupings:

#. `CHECKDB`_ --> Flags that modify CHECKDB behavior or return additional info.
#. `Engine Detection`_ --> Flags that enable additional checking for corruption by core engine activities.
#. `Fixes and Past Relevance`_ --> Fix flags and flags which are no longer relevant.



See also: 

TODO: Strongly consider combining this and the BackupRestore section somehow

TODO: Reconsider where 2509 and 2514 go. (Have to do with file/page internals, but activated by DBCC CHECKDB)


|

Short Descriptions
------------------

.. This comment line is as long as we would ever want the short desc to be in the table below.

.. list-table::
	:widths: 10 60
	:header-rows: 1

	* - Flag
	  - Short Description
	* - See Also
	  - 
	* - :doc:`205 <qry_CompInfo>`
	  - Reports to error log when recompile occurs due to auto-update of stats.
	* - ...
	  - 
	* - :ref:`CHECKDB <CHECKDB>`
	  - 
	* - 2508_
	  - (speculative) Disables parallel non-clustered idx checks in CHECKTABLE.
	* - 2514_
	  - Causes DBCC CHECKTABLE to return total count of ghost records.
	* - \* 2528_
	  - Disables parallel checking of objects by DBCC CHECK* commands.
	* - 2529_
	  - Appears to print memory usage by DBCC CHECKDB.
	* - 2549_
	  - Causes CHECKDB to assume each file is on a unique LUN.
	* - 2558_
	  - Disables integration between CHECKDB and Watson.
	* - \* 2562_
	  - Runs all DB objects in a single CHECKDB batch.
	* - 2566_
	  - Disables DATA_PURITY checks in CHECKDB.
	* - ...
	  - 
	* - :ref:`Engine <EngDetect>`
	  - 
	* - 806_
	  - Enables "DBCC-style" auditing on each page read.
	* - 815_
	  - Enables latch enforcement to help catch memory scribblers.
	* - 818_
	  - Adds auditing for disk write IOs.
	* - 813_
	  - Protects unchanged buffer pool changes from memory corruptions.
	* - ...
	  - 
	* - :ref:`Fix/PastRel <FixPastRel8>`
	  - 
	* - 2509_
	  - Possibly the SQL 2000 equivalent to TF 2514 above.


	 
.. This comment line is as long as we would ever want the short desc to be in the table above.

|

.. _CHECKDB:
	 
CheckDB
-------

.. _2508:

2508
	TODO: experiment to see if this has any kernel of truth, may just be a typo of 2528:  
	Social.Technet: "Disables parallel non-clustered index checking for DBCC CHECKTABLE"
	
	`Social.Technet`_

	
.. _2514:

2514 (Info)
	Used with DBCC CHECKTABLE to see the total count of ghost records in a table.
	
	Argenis_1_ 


.. _2528:

2528 ``Doc2005`
	`BOL 2014`_: "Disables parallel checking of objects by DBCC CHECKDB, DBCC CHECKFILEGROUP, and 
	DBCC CHECKTABLE. By default, the degree of parallelism is automatically determined by the query 
	processor. The maximum degree of parallelism is configured just like that of parallel queries. 
	For more information, see Configure the max degree of parallelism Server Configuration Option.

	Parallel DBCC should typically be left enabled. For DBCC CHECKDB, the query processor 
	reevaluates and automatically adjusts parallelism with each table or batch of tables checked. 
	Sometimes, checking may start when the server is almost idle. An administrator who knows that 
	the load will increase before checking is complete may want to manually decrease or disable 
	parallelism.

	Disabling parallel checking of DBCC can cause DBCC to take much longer to complete and if 
	DBCC is run with the TABLOCK feature enabled and parallelism set off, tables may be locked 
	for longer periods of time."
	
	PRand_2_ 
	

.. _2529:

2529 (Info?) 
	Full functionality unknown, but appears to print memory usage for CHECKDB (memory that 
	appears to be allocated via an “IMemObj” interface) at the very beginning and very end 
	of CHECKDB output.
	
	`SQLService.se`_ 


.. _2549:

2549 ``Doc2014``
	`BOL 2014`_: "Runs the DBCC CHECKDB command assuming each database file is on a unique disk 
	drive. DBCC CHECKDB command builds an internal list of pages to read per unique disk drive 
	across all database files. This logic determines unique disk drives based on the drive letter 
	of the physical file name of each file.

	Note: Do not use this trace flag unless you know that each file is based on a unique 
	physical disk.

	Note: Although this trace flag improve the performance of the DBCC CHECKDB commands which 
	target usage of the PHYSICAL_ONLY option, some users may not see any improvement in 
	performance. While this trace flag improves disk I/O resources usage, the underlying 
	performance of disk resources may limit the overall performance of the DBCC CHECKDB command."
	
	2634571_ | BobWard_1_ | CSS_2_ | Bertrand_1_ | PRand_3_ | Connect_1_ (problems w/SQL 2014)

	
.. _2558:

2558
	Disables integration between CHECKDB and Watson.
	
	CSS_3_

	
.. _2562:

2562 ``Doc2014``
	`BOL 2014`_: "Runs the DBCC CHECKDB command in a single "batch" regardless of the number of 
	indexes in the database. By default, the DBCC CHECKDB command tries to minimize tempdb 
	resources by limiting the number of indexes or "facts" that it generates by using a "batches" 
	concept. This trace flag forces all processing into one batch.

	One effect of using this trace flag is that the space requirements for tempdb may increase. 
	Tempdb may grow to as much as 5% or more of the user database that is being processed by the 
	DBCC CHECKDB command.

	Note: Although this trace flag improve the performance of the DBCC CHECKDB commands which 
	target usage of the PHYSICAL_ONLY option, some users may not see any improvement in 
	performance. While this trace flag improves disk I/O resources usage, the underlying 
	performance of disk resources may limit the overall performance of the DBCC CHECKDB command."
	
	2634571_ | BobWard_1 | CSS_2_ | Bertrand_1_ | PRand_3_ | PRand_4_
	

.. _2566:

2566
	Disables DATA_PURITY checks (in CHECKDB) and was released as a workaround for several problems in x64 
	instances (as described in the KB articles).
	
	945770_ | 2888996 | Bertrand_1_ 
	

	
|

.. _EngDetect: 
	 
Engine Detection
----------------

.. _806: 

806
	PRand: "Causes 'DBCC-style' page auditing to be performed whenever a database page is read 
	into the buffer pool. This is useful to catch cases where pages are being corrupted in memory 
	and then written out to disk with a new page checksum. When they're read back in the checksum 
	will look correct, but the page is corrupt (because of the previous memory corruption). This 
	page auditing goes someway to catching this - especially on non-Enterprise Edition systems that 
	don't have the 'checksum sniffer'." [Paul notes there will be a CPU hit if you turn this on].
	
	841776_ | `IOBasics Chapter 2`_ | PRand_1_
	
	
.. _815:

815
	IO Basics: "To help detect unwanted changes to in-memory SQL Server data pages, latch 
	enforcement is enhanced with the –T815 trace flag. When a page is latched for modification, 
	the VirtualProtect on the page is set to PAGE_READWRITE. At all other times the protection 
	is PAGE_READONLY. This can help catch actions such as memory overwrites (scribblers). Starting 
	with...build 8.00.0922, you can dynamically turn on or turn off trace flag -T815 by using the 
	DBCC TRACEON...TRACEOFF. Important Latch enforcement is only valid for non-AWE (Address 
	Windowing Extensions) environments."
	
	IOBasics_ | CSS_1_ 

	
.. _818:

818
	IO Basics: "...tracks the last 2,048 page write operations. During a successful write I/O 
	completion (proper page ID, bytes transferred successfully, and the proper OS error codes), 
	the DBID, Page ID, and LSN are recorded in a ring buffer. If a failure occurs, error 823 is 
	raised. When an 823 or 605 error is detected, SQL Server looks in the ring buffer for the 
	LSN value that was on the page during the last write. If not correct, extra information is 
	added to the SQL Server error log. The information indicates the type of error along with both 
	the expected and the retrieved LSN."
	
	826433_ | 828339_ | IOBasics_ 


.. _831:

831
	Randal-SQL-SDB407: "Protect unchanged pages in the buffer pool to catch memory corruptions."

	`Randal-SQL-SDB407`_



	
|

.. _FixPastRel8:

Fixes and Past Relevance
------------------------
These flags either are old and irrelevant for modern builds, appear only in CTPs, or enable a 
fix in a CU but are baselined in a later service pack or release.

.. _2509:

2509
	The only reference I can find is found in Ken Henderson’s Guru’s Guide to T-SQL, on page 
	503 (found via books.google.com): "Used in conjunction with DBCC CHECKTABLE to see the 
	total count of ghost records in a table." Maybe the SQL 2000 corollary to 2514 in 
	modern builds?

	



.. Official Links 

.. _BOL 2008: https://technet.microsoft.com/en-us/library/ms188396(v=sql.100).aspx

.. _BOL 2014: https://technet.microsoft.com/en-us/library/ms188396.aspx

.. _BOL 2016: https://technet.microsoft.com/en-us/library/ms188396.aspx

.. _ALTER DATABASE file and filegroup options: https://msdn.microsoft.com/en-us/library/bb522469.aspx

.. _ALTER DATABASE SET Options: https://msdn.microsoft.com/en-us/library/bb522682.aspx

.. _IOBasics: http://technet.microsoft.com/en-us/library/cc966500.aspx

.. _IOBasics Chapter 2: http://technet.microsoft.com/en-us/library/cc917726.aspx

.. _826433: http://support.microsoft.com/kb/826433

.. _828339: http://support.microsoft.com/kb/828339/en-us

.. _841776: http://support.microsoft.com/kb/841776/en-us

.. _945770: http://support.microsoft.com/kb/945770

.. _2634571: http://support.microsoft.com/kb/2634571/en-us

.. _2888996: http://support.microsoft.com/kb/2888996/en-us


.. MSFT Blog links

.. _Bertrand_1: http://www.sqlperformance.com/2012/11/io-subsystem/minimize-impact-of-checkdb

.. _BobWard_1: https://blogs.msdn.microsoft.com/bobsql/2016/07/12/dbcc-trace-flags-2562-and-2549/

.. _CSS_1: http://blogs.msdn.com/b/psssql/archive/2012/11/12/how-can-reference-counting-be-a-leading-memory-scribbler-cause.aspx

.. _CSS_2: http://blogs.msdn.com/b/psssql/archive/2012/02/23/a-faster-checkdb-part-ii.aspx

.. _CSS_3: http://blogs.msdn.com/b/psssql/archive/2009/11/17/how-it-works-controlling-sql-server-memory-dumps.aspx


.. Non-MSFT bloggers

.. _Argenis_1: http://sqlblog.com/blogs/argenis_fernandez/archive/2012/05/29/ghost-records-backups-and-database-compression-with-a-pinch-of-security-considerations.aspx

.. _PRand_1: http://www.sqlskills.com/blogs/paul/how-to-tell-if-the-io-subsystem-is-causing-corruptions/

.. _PRand_2: http://www.sqlskills.com/blogs/paul/checkdb-from-every-angle-how-long-will-checkdb-take-to-run/

.. _PRand_3: http://www.sqlskills.com/blogs/paul/dbcc-checkdb-scalability-and-performance-benchmarking-on-ssds/

.. _PRand_4: http://www.sqlskills.com/blogs/paul/how-does-dbcc-checkdb-with-estimateonly-work/


.. Connect links

.. _Connect_1: https://connect.microsoft.com/SQLServer/feedback/details/982532/sql-server-2014-rtm-ignores-trace-flag-2549


.. Forums 


.. Other Links 

.. _Randal-SQL-SDB407: http://www.scribd.com/doc/109431789/Randal-SQL-SDB407-Undocumented

.. _Social.Technet: http://social.technet.microsoft.com/wiki/contents/articles/13105.trace-flags-in-sql-server.aspx

.. _SQLService.se: http://sqlservice.se/sv/start/blogg/updated-microsoft-sql-server-trace-flag-list.aspx
