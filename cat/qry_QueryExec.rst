===============================
Query Execution / Functionality
===============================

This category mashes up several smaller categories:

#. `Query Execution`_ --> Flags in this category affect runtime decisions made just before a query plan begins execution (or prints information about runtime behavior).
#. `Query Functionality`_ --> Flags in this category modify the functional definition of the syntax; most flags are quite old, and many cannot be confirmed.
#. `Query Functionality (Unable to Confirm)`_ --> Other public repos contain large numbers of flags that we cannot confirm. Many may be from old Sybase days, and these flags typically modify the functionality of syntax. These have been put into their own section.

The standard `Fixes and Past Relevance`_ section holds, as usual, flags that have trustworthy links but have ceased to be relevant
for modern builds (or are fix flags whose expected utility is very short). 

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
	* - ...
	  - 
	* - :ref:`QueryExec <QryExec>`
	  - 
	* - 646_
	  - Prints to errlog which ColStore segments were eliminated at runtime.
	* - 1504_
	  - Prints to console or errlog when an index rebuild increases its mem grant.
	* - 2466_
	  - Affects runtime DOP choice; reverts to older polling-based logic.
	* - 2467_
	  - Affects runtime DOP choice; attempts to fit query within least-loaded node.
	* - 2468_
	  - Affects runtime DOP choice; round-robin search for node that can handle query.
	* - 2479_
	  - Affects runtime DOP choice; restrict query to node the connection is tied to.
	* - 2486_
	  - Enables output for query_trace_column_values Extended Event.
	* - 7525_
	  - Affects when cursors are closed upon ROLLBACK
	* - 9389_
	  - Enables dynamic memory grant for batch mode operators
	* - ...
	  - 
	* - :ref:`QueryFunc <QryFunc>`
	  - 
	* - 3640_
	  - Like SET NOCOUNT ON, eliminates sending DONE_IN_PROC messages to client.
	* - ...
	  - 
	* - :ref:`QueryFunc ? <QryFuncOld>`
	  - 
	* - 237_
	  - ?? Use correlated sub-queries in Non-ANSI backward compatibility. ??
	* - 242_
	  - ?? Provides backward compat for correlated sub-queries w/non-ANSI results. ??
	* - 243_
	  - ?? Provides backward compat for nullability behavior. ??
	* - 244_
	  - ?? Disables checking for allowed interim constraint violations. ??
	* - 506_
	  - ?? Enforces SQL-92 standards re: null values for parm/var comparisons ??
	* - 8783_
	  - ?? Allows INSERT/UPDATE/DELETE statements to honor SET ROWCOUNT ON ??
	* - 8816_
	  - ?? Logs every two-digit year conversion to a four-digit year. ??
	* - ...
	  - 
	* - :ref:`Fix/PastRel <FixPastRel4>`
	  - 
	* - 107_
	  - Enabled the insertion of '0 into columns of type float, decimal, numeric, or real.
	* - 262_
	  - Enables a SQL 7 fix re: strings w/trailing spaces being truncated.
	* - 6530_
	  - Enables a fix for poor performance when building an index on a spatial data type.
	 
.. This comment line is as long as we would ever want the short desc to be in the table above.

|

.. _QryExec: 
	 
Query Execution
---------------

.. _646:

646
	Prints (to SQL error log) which segments were eliminated at runtime by columnstore segment elimination. Requires 3605. 
	
	Technet_1_ | Niko_1_ | Niko_2_ | JSack_1_ 

.. _1504:

1504
	Prints to the console (w/3604) or the error log (w/3605; required for parallel index builds) when an index DDL command 
	requires more memory to be granted to continue sorting rows in memory.
	
	PWhite_1_

.. _2466:

2466
	When the optimizer is choosing the runtime DOP for a parallel plan, this directs it to use logic found in "older versions" 
	(the post doesn’t say which versions) to determine which NUMA node to place the parallel plan on. This older logic relies on 
	a polling mechanism (roughly every 1 second), and can result in race conditions where 2 parallel plans end up on the same node. 
	The newer logic "significantly reduces" the likelihood of this happening. 
	
	CSS_1_ 
	
.. _2467:

2467
	"If target MAXDOP target is less than a single node can provide and if trace flag 2467 is enabled, attempt to locate 
	the least loaded node."
	
	CSS_1_ | CSS_2_

.. _2468:
	
2468
	"Find the next node that can service the DOP request. Unlike full mode, the global, resource manager keeps track of the 
	last node used. Starting from the last position, and moving to the next node, SQL Server checks for query placement 
	opportunities. If a node can’t support the request SQL Server continues advancing nodes and searching."
	
	CSS_2_ 

.. _2479:

2479
	When SQL Server is determining the runtime DOP for a parallel plan, this flag directs it to limit the NUMA Node 
	placement for the query to the node that the connection is associated with.
	
	CSS_1_ | CSS_2_ 

.. _2486:
	
2486
	In SQL 2016 (CTP 3.0 at least), enables output for the "query_trace_column_values" Extended Event, allowing the 
	value of output columns from individual plan iterators to be traced.
	
	Dima_1_




.. _7525:
	
7525
	Affects when cursors are closed upon ROLLBACK. This flag reverts to SQL 7.0 RTM behavior. Unsure of whether this is still 
	applicable to modern versions.
	
	199294_ 

.. _9389:

9389 ``Doc2014`` 
	`BOL 2014`_: "Enables dynamic memory grant for batch mode operators...If the dynamic memory grant trace flag is enabled,
	a batch mode operator may ask for additional memory and avoid spilling to tempdb if additional memory is available."


|

.. _QryFunc: 
	 
Query Functionality
-------------------

.. _3640:

3640
	Eliminates sending DONE_IN_PROC messages to client for each statement in stored procedure. This is similar to the 
	session setting of SET NOCOUNT ON. (The flag gives the ability to control at a wider level without changing code).
	
	Selvar_

	

|


.. _QryFuncOld: 
	 
Query Functionality (Unable to Confirm)
---------------------------------------
Initial attempts to find online documentation that is reasonably trustworthy has not been successful. 

.. _237:

237
	Tells SQL Server to use correlated sub-queries in Non-ANSI standard backward compatibility mode	
	
.. _242:
	
242
	Provides backward compatibility for correlated subqueries where non-ANSI-standard results are desired.	
	
.. _243:
	
243
	Provides backward compatibility for nullability behavior. When set, SQL Server has the same nullability violation behavior as that of a ver 4.2:
	
		a)	Processing of the entire batch is terminated if the nullability error (inserting NULL into a NOT NULL field) can be detected at compile time. 
		b)	Processing of offending row is skipped, but the command continues if the nullability violation is detected at run time. 
		
	Behavior of SQL Server is now more consistent because nullability checks are made at run time and a nullability violation results in the command terminating 
	and the batch or transaction process continuing.	
	
.. _244: 

244
	Disables checking for allowed interim constraint violations. By default, SQL Server checks for and allows interim constraint violations. 
	An interim constraint violation is caused by a change that removes the violation such that the constraint is met, all within a single statement and transaction. 
	SQL Server checks for interim constraint violations for self-referencing DELETE statements, INSERT, and multi-row UPDATE statements. This checking requires more 
	work tables. With this trace flag you can disallow interim constraint violations, thus requiring fewer work tables.	

.. _506:

506	
	Enforces SQL-92 standards regarding null values for comparisons between variables and parameters. Any comparison of variables and parameters that contain a 
	NULL always results in a NULL.
	
.. _8783:

8783
	Allows DELETE, INSERT, and UPDATE statements to honor the SET ROWCOUNT ON setting when enabled.	
	
.. _8816:

8816
	Logs every two-digit year conversion to a four-digit year.	


|

.. _FixPastRel4:

Fixes and Past Relevance
------------------------
These flags either are old and irrelevant for modern builds, appear only in CTPs, or enable a fix 
in a CU but are baselined in a later service pack or release.

.. _107: 

107
	Enabled the insertion of '0 into columns of type float, decimal, numeric, or real. 
	
	160732_

.. _262: 

262
	Enables a fix in SQL 7.0 to correct a problem with strings w/trailing spaces being truncated even when ANSI PADDING was on. 
	
	891116_ 

	
.. _6530:

6530
	Enables a fix for poor performance when building an index on a spatial data type.
	
	2896720_



.. Official Links 

.. _BOL 2014: https://technet.microsoft.com/en-us/library/ms188396.aspx

.. _160732: https://support.microsoft.com/en-us/kb/160732

.. _199294: https://support.microsoft.com/en-us/kb/199294/en-us

.. _891116: https://support.microsoft.com/en-us/kb/891116

.. _2896720: http://support.microsoft.com/kb/2896720/en-us


.. MSFT Blog links

.. _CSS_1: http://blogs.msdn.com/b/psssql/archive/2013/09/27/how-it-works-maximizing-max-degree-of-parallelism-maxdop.aspx

.. _CSS_2: https://blogs.msdn.microsoft.com/psssql/2016/03/04/sql-server-parallel-query-placement-decision-logic/

.. _Selvar: http://blogs.msdn.com/b/selvar/archive/2010/07/14/delete-operation-editing-a-data-source-from-a-reporting-service-2005-report-manager-fails-internalcatalogexception-and-throwing-watson-dump.aspx

.. _Technet_1: http://social.technet.microsoft.com/wiki/contents/articles/5611.verifying-columnstore-segment-elimination.aspx

.. _JSack_1: http://www.sqlskills.com/blogs/joe/exploring-columnstore-index-metadata-segment-distribution-and-elimination-behaviors/


.. Non-MSFT bloggers

.. _Dima_1: http://www.queryprocessor.com/query-trace-column-values/

.. _Niko_1: http://www.nikoport.com/2014/07/24/clustered-columnstore-indexes-part-35-trace-flags-query-optimiser-rules/ 

.. _Niko_2: http://www.nikoport.com/2015/05/24/azure-columnstore-part-3-modern-segment-elimination-and-set-statistics-io/ 

.. _PWhite_1: http://sqlperformance.com/2015/04/sql-plan/internals-of-the-seven-sql-server-sorts-part-1
	


.. Connect links


.. Forums 



.. Other Links 

