===================================
Transactions, Logging, and Recovery
===================================


See also: 9592 and 9567 High Availability/Distributed Technologies https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/HADRDist.md


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
	* - :ref:`Flags <TranLogFlags>`
	  - 
	* - \* 610_
	  - Enables minimal logging under certain scenarios.
	* - 2537_
	  - Enables fn_dblog() to read the inactive portion of the t-log.
	* - 3412_
	  - Reports when each tran is rolled forward or back.
	* - 8599_
	  - Allows the use of save points within distributed trans.
	* - 9038_
	  - Disables the multi-threaded log writer in SQL 2016.
	* - 9099_
	  - Forces single-threaded ALTER TABLE.
	* - ...
	  - 
	* - :ref:`Fix/PastRel <FixPastRel10>`
	  - 
	* - 3924_
	  - Enables a fix for "XA" trans w/in JDBC connections.
	* - 9024_
	  - Partitions a log pool memory object by memory node.

	 
.. This comment line is as long as we would ever want the short desc to be in the table above.

|

.. _TranLogFlags:
	 
Flags
-------------

.. _610:

610
	Enables minimal-logging under certain scenarios. Connect item indicate TF 610 
	soon may be a query hint.
	
	`Data Loading Perf Guide`_ | StorEng_1_ | Connect_1_


.. _2537:

2537
	Enables fn_dblog() to read all of the t-log, not just the active portion of the log. 
	Argenis Tweet: fn_dblog + 2537 is a way to check your t-log file for corruption.
	
	PRand_1_ | PRand_2_  


.. _3412:
	
3412
	Reports when each transaction is rolled forward or back, except for large transactions.
	(Needs verification for whether still current)
	
	170115_
	
	
.. _8599:

8599
	Allows the use of a save-point within a distributed transaction.
	
	295027_ | 903742_


.. _9038:

9038
	CAdkin Tweet: "If you are using low-latency storage it appears that you can also 
	cause high spins on LOGFLUSHQ in SQL 2016 with the in-memory engine and full durability. 
	The multithreaded log writer is the cause of this and can be turned off via TF 9038."


.. _9909:
	
9909
	Turns off a logging optimization when an ALTER TABLE operation on a memory-optimized 
	table would normally be done in parallel. This avoids the possibility of data loss 
	b/c of a bug in parallel ALTER TABLE.
	
	3174963 https://support.microsoft.com/en-us/kb/3174963

|

.. _FixPastRel10:

Fixes and Past Relevance
------------------------
These flags either are old and irrelevant for modern builds, appear only in CTPs, or enable a 
fix in a CU but are baselined in a later service pack or release.

.. _3924:

3924
	Enables a fix where "XA" transactions started within a JDBC-connected Java app are 
	not closed properly and stay open indefinitely.
	
	3145492_


.. _9024:

9024
	(On by default as of SQL 2014 SP1+) Enables a fix in SQL 2012/2014 that partitions 
	(by memory node) a "pointer to a memory object" (PMO) to the log pool. The KB 
	references this flag specifically in the context of Availability Groups, though 
	the flag appears to deal with the design of the "logpool". Note that 8048 is also 
	potentially applicable here, as it will further partition the PMO to a by-CPU 
	partitioning scheme. (See the KB).
	
	2809338_

	


.. Official Links 

.. _BOL 2008: https://technet.microsoft.com/en-us/library/ms188396(v=sql.100).aspx

.. _BOL 2014: https://technet.microsoft.com/en-us/library/ms188396.aspx

.. _BOL 2016: https://technet.microsoft.com/en-us/library/ms188396.aspx

.. _Data Loading Perf Guide: http://technet.microsoft.com/en-us/library/dd425070(v=sql.100).aspx

.. _170115: http://support.microsoft.com/kb/170115/en-us

.. _295027: http://support.microsoft.com/kb/295027

.. _903742: http://support.microsoft.com/kb/903742/en-us

.. _2809338: http://support.microsoft.com/kb/2809338/en-us

.. _3145492: https://support.microsoft.com/en-us/kb/3145492

.. _3174963: https://support.microsoft.com/en-us/kb/3174963


.. MSFT Blog links

.. _StorEng_1: http://blogs.msdn.com/b/sqlserverstorageengine/archive/2008/03/23/minimal-logging-changes-in-sql-server-2008-part-2.aspx


.. Non-MSFT bloggers

.. _PRand_1: http://www.sqlskills.com/blogs/paul/finding-out-who-dropped-a-table-using-the-transaction-log/

.. _PRand_2: https://www.sqlskills.com/blogs/paul/using-fn_dblog-fn_dump_dblog-and-restoring-with-stopbeforemark-to-an-lsn/


.. Connect links

.. _Connect_1: http://connect.microsoft.com/SQLServer/feedback/details/557515/trace-flag-610-functionality-should-be-implemented-as-a-hint

.. Forums 


.. Other Links 

