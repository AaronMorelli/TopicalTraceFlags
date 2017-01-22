===================
Locking and Waiting
===================

Flags in this category fall into the below groupings:

#. `Informational`_ --> Flags that return info about locks and deadlocks.
#. `Functionality Toggles`_ --> Flags that modify lock escalation or other locking behaviors.
#. `Fixes and Past Relevance`_ --> Fix flags and flags which are no longer relevant.


See also: 

1229 prob needs a See Also in the scheduler section.

Is 8755 already referenced in the query optimization section? 

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
	* - :ref:`Info <InfoLockWait>`
	  - 
	* - 611_
	  - Records lock escalations in the SQL error log.
	* - 1200_
	  - Prints detailed lock info for every lock request/release.
	* - \* 1204_
	  - Prints detailed info upon every deadlock occurrence.
	* - 1205_
	  - Prints info to SQL error log every time a deadlock search begins.
	* - 1206_
	  - Complements 1204 by printing other locks held by deadlock parties.
	* - 1208_
	  - Prints host name/program name [by clients in a deadlock?]
	* - 1222_
	  - Similar to 1204, but with more info and in an XML format.
	* - 8001_
	  - Adds more wait types to sys.dm_os_wait_stats
	* - 8050_
	  - Removes "optional" wait types from sys.dm_os_wait_stats.
	* - ...
	  - 
	* - :ref:`Func <FuncLockWait>`
	  - 
	* - \* 1211_
	  - Disables lock escalations due to memory pressure.
	* - \* 1224_
	  - Disables lock escalations based on number of locks.
	* - 1229_
	  - Disables lock partitioning among schedulers.
	* - 8755_
	  - Disables any locking hints in query text.
	* - ...
	  - 
	* - :ref:`Fix/PastRel <FixPastRel8>`
	  - 
	* - 1216_
	  - Complicated; see below
	* - 1236_
	  - Reduces LOCK_HASH spinlock contention on older builds
	* - 2456_
	  - Relieves RESOURCE_SEMAPHORE_MUTEX contention in SQL 2005.
	 
.. This comment line is as long as we would ever want the short desc to be in the table above.

|

.. _InfoLockWait:
	 
Informational
-------------

.. _611:

611 (Info)
	Records each lock escalation, in the form: "Escalated locks - Reason: LOCK_THRESHOLD, Mode: S, 
	Granularity: TABLE, Table: 222623836, HoBt: 150:256, HoBt Lock Count: 6248, Escalated Lock 
	Count: 6249, Line Number: 1, Start Offset: 0, SQL Statement: select count(*) from dbo.BigTable"
	
	Aaron: Confirmed still in SQL 2014. 
	
	SSWUG_1_


.. _1200:	
	
1200
	Prints detailed lock info as every request for a lock is made (the process ID and type of 
	lock requested).
	
	169960_ | StorEng_1_ (comments)
	

.. _1204:	
	
1204 ``Doc2005`` (Info)
	`BOL 2014`_: "Returns the resources and types of locks participating in a deadlock and 
	also the current command affected.". 
	
	937950 notes a specific type of error where 1204 will not help with deadlocks.
	
	832524_ | 937950_ 
	

.. _1205:
	
1205 (Info)
	Prints information to the SQL error log every time a deadlock search starts, 
	whether or not a deadlock is found.
	
	832524_

	
.. _1206:

1206 (Info)
	Used to complement flag 1204 by displaying other locks held by deadlock parties.
	
	169960_
	

.. _1208:
	
1208 (Info)
	KB: "Prints the host name and program name supplied by the client. This can help 
	identify a client involved in a deadlock, assuming the client specifies a unique 
	value for each connection." 

	169960_


.. _1222:

1222 ``Doc2005`` (Info) 
	`BOL 2014`_: "Returns the resources and types of locks that are participating in a 
	deadlock and also the current command affected, in an XML format that does not comply 
	with any XSD schema."
	
	
.. _8001:

8001 (Info)
	Khen2005, p2: "For SQL Server 2005, the SQL Server product team opted not to include 
	[in sys.dm_os_wait_stats] some wait types that fall under one of the following three 
	categories:
		-	Wait types that are never used in SQL Server 2005; note that some wait types not excluded are also never used.
		-	Wait types that can occur only at times when they do not affect user activity, such as during initial server startup and shutdown, and are not visible to users.
		-	Wait types that are innocuous but have caused concern among users because of their high occurrence or duration

	The complete list of wait types is available by enabling trace flag 8001. The only effect 
	of this trace flag is to force sys.dm_os_wait_stats to display all wait types."


.. _8050:

8050 (Info)
	Causes "optional" wait types (see the CSS article) to be excluded when querying sys.dm_os_wait_stats.
	
	CSS_2_


|

.. _FuncLockWait: 
	 
Functionality Toggles
---------------------

.. _1211:
	
1211 ``Doc2005``
	`BOL 2014`_: "Disables lock escalation based on memory pressure, or based on number of 
	locks. The SQL Server Database Engine will not escalate row or page locks to table locks.

	Using this trace flag can generate excessive numbers of locks. This can slow the 
	performance of the Database Engine, or cause 1204 errors (unable to allocate lock 
	resource) because of insufficient memory.

	If both trace flag 1211 and 1224 are set, 1211 takes precedence over 1224. However, 
	because trace flag 1211 prevents escalation in every case, even under memory pressure, 
	we recommend that you use 1224. This helps avoid "out-of-locks" errors when many locks 
	are being used."
	
	PRand_1_


.. _1224:

1224 ``Doc2005``
	`BOL 2014`_: "Disables lock escalation based on the number of locks. However, memory 
	pressure can still activate lock escalation. The Database Engine escalates row or page 
	locks to table (or partition) locks if the amount of memory used by lock objects exceeds 
	one of the following conditions:
		- Forty percent of the memory that is used by Database Engine. This is applicable only when the locks parameter of sp_configure is set to 0.
		- Forty percent of the lock memory that is configured by using the locks parameter of sp_configure.
		
	If both trace flag 1211 and 1224 are set, 1211 takes precedence over 1224. However, 
	because trace flag 1211 prevents escalation in every case, even under memory pressure, 
	we recommend that you use 1224. This helps avoid "out-of-locks" errors when many locks 
	are being used.

	Note: Lock escalation to the table- or HoBT-level granularity can also be controlled by 
	using the LOCK_ESCALATION option of the ALTER TABLE statement."

	PRand_1_


.. _1229:

1229
	Disables lock partitioning (among schedulers).
	
	CSS_1_ 


.. _8755:

8755
	Disables any locking hints and permits the optimizer/query execution engine to select 
	the appropriate lock behavior.
	
	SQLMag_1_




|

.. _FixPastRel8:

Fixes and Past Relevance
------------------------
These flags either are old and irrelevant for modern builds, appear only in CTPs, or enable a 
fix in a CU but are baselined in a later service pack or release.

.. _1216:

1216 (Info?)
	319892: mentions 1216 in passing as a flag in SQL 2000 that is associated with 
	deadlock output. The mention occurs only to distance TF1216 on SQL 2000 from TF 1216 
	on SQL 7.0 and TF 1261 on SQL 2000, which both have a different purpose than 1216 on 
	SQL 2000. Prob needs to be tested on modern versions.

	319892_
	

.. _1236:

1236
	(On by default in SQL 2014 SP1+ and SQL 2012 SP3) Partitions the DATABASE lock type to 
	help reduce contention on internal locking structures symptomized by LOCK_HASH spinlock 
	contention.
	
	2926217_

	
.. _2456:

2456
	Relieves RESOURCE_SEMAPHORE_MUTEX contention, which may be primarily due to a bug in 
	SQL 2005.

	956375_


	


.. Official Links 

.. _BOL 2008: https://technet.microsoft.com/en-us/library/ms188396(v=sql.100).aspx

.. _BOL 2014: https://technet.microsoft.com/en-us/library/ms188396.aspx

.. _BOL 2016: https://technet.microsoft.com/en-us/library/ms188396.aspx

.. _169960: http://support.microsoft.com/kb/169960/en-us

.. _2926217: http://support.microsoft.com/kb/2926217

.. _319892: http://support.microsoft.com/kb/319892

.. _832524: http://support.microsoft.com/kb/832524

.. _956375: http://support.microsoft.com/kb/956375/en-us

.. _937950: http://support.microsoft.com/kb/937950/en-us


.. MSFT Blog links

.. _CSS_1: http://blogs.msdn.com/b/psssql/archive/2012/08/31/strange-sch-s-sch-m-deadlock-on-machines-with-16-or-more-schedulers.aspx

.. _CSS_2: http://blogs.msdn.com/b/psssql/archive/2009/11/03/the-sql-server-wait-type-repository.aspx

.. _StorEng_1: http://blogs.msdn.com/b/sqlserverstorageengine/archive/2008/03/30/sql-server-table-variable-vs-local-temporary-table.aspx



.. Non-MSFT bloggers

.. _PRand_1: http://www.sqlskills.com/blogs/paul/a-sql-server-dba-myth-a-day-2330-lock-escalation/


.. Connect links


.. Forums 


.. Other Links 

.. _SSWUG_1: http://www.sswug.org/articles/viewarticle.aspx?id=undocumented_sql_server_2005_trace_flags-27595

.. _SQLMag_1: http://sqlmag.com/sql-server/investigating-trace-flags
