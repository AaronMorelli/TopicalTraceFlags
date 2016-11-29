============
Plan Caching
============

Most of the flags affect the size of the plan cache, or how certain types of plans (e.g. zero-cost, ad-hoc) are cached.
This is a very small category and may be combined with another at a later time.

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
	  - These flags significantly affect the retention of cached plans.
	* - :doc:`205 <qry_CompInfo>`
	  - Reports to error log when recompile occurs due to auto-update of stats.
	* - :doc:`2389 <qry_StatsAndEst>`
	  - Enables optimizer tracking of ascending keys and subsequent recompilation behavior.
	* - :doc:`2390 <qry_StatsAndEst>`
	  - Similar to 2390 and the ascending key problem.
	* - :doc:`2453 <qry_StatsAndEst>`
	  - Applies recompile thresholds for temp tables to table variables
	* - :doc:`4139 <qry_StatsAndEst>`
	  - Enables "quick stats" histogram amendments even if key col isn't branded ascending.
	* - \* :doc:`2371 <qry_StatsAndEst>`
	  - Lowers relative threshold for stats-based recompilation as table gets larger
	* - ...
	  - 
	* - :ref:`Func Toggles <FuncToggle>`
	  - 
	* - 174_
	  - Increases plan cache bucket count from 40k to 160k
	* - 253_
	  - (May) prevent ad-hoc query plans from being cached.
	* - 2861_
	  - Instructs SQL Server to keep zero cost plans in cache
	* - 8032_
	  - Reverts the cache limit parameters to the SQL Server 2005 RTM settings.
	* - ...
	  - 
	* - :ref:`Fix/PastRel <FixPastRel5>`
	  - 
	* - 2880_
	  - Enables an upper bound for the plan cache in SQL 2000 32-bit.
	* - 2881_
	  - Turns off an upper bound on the plan cache in SQL 2000 64-bit
	 
.. This comment line is as long as we would ever want the short desc to be in the table above.

|

.. _FuncToggle: 
	 
Functionality Toggles
---------------------

.. _174: 

174 ``Doc2014``
	`BOL 2014`_: "Increases the SQL Server Database Engine plan cache bucket count from 40,009 to 
	160,001 on 64-bit systems." Can relieve SOS_CACHESTORE spinlock contention though at the cost of 
	increasing the amount of memory allowed for the plan cache.
	
	3026083_ 
	
.. _253:

253
	SSC repository: "Prevents ad-hoc query plans to stay in cache." Even though many "three-digit" query plan numbers 
	don't seem to be relevant any more, the first SO hit indicates that this flag still changes functionality in the manner 
	described by the SSC definition. As a side note, Martin Smith's comment in the second Stack Overflow hit about DAC queries 
	never being cached is interesting.
	
	StackOverflow_1_ | DBAse_1_

.. _2861:

2861
	KB: "Instructs SQL Server to keep zero cost plans in cache, which SQL Server would typically not cache (such as simple 
	ad-hoc queries, set statements, commit transaction and others)." 
	
	325607_ | SolarWindsDPA_ | SQLMag_1_ | DBAse_2_
	
.. _8032:

8032 ``Doc2008R2``
	`BOL 2014`_: "Reverts the cache limit parameters to the SQL Server 2005 RTM setting which in general allows caches to be larger. 
	Use this setting when frequently reused cache entries do not fit into the cache and when the optimize for ad hoc workloads 
	Server Configuration Option has failed to resolve the problem with plan cache. WARNING: Trace flag 8032 can cause poor 
	performance if large caches make less memory available for other memory consumers, such as the buffer pool." 
	
	`Banerjee`_: Can't be used as a startup flag; @sql_handle tweet indicates it can ONLY be used as a startup flag in SQL 2014 
	(and I would guess in 2012 as well, since that's when the SQLOS mem rearchitecture occurred). 
	Another @sql_handle tweet indicates it also increases size of log pool cache.


|

.. _FixPastRel5:

Fixes and Past Relevance
------------------------
These flags either are old and irrelevant for modern builds, appear only in CTPs, or enable a fix 
in a CU but are baselined in a later service pack or release.

.. _2880: 

2880
	Enables an upper bound for the plan cache in SQL 2000 32-bit. (The upper bound is by default disabled in 32-bit 
	SQL 2000 and enabled in 64-bit SQL 2000).
	
	891707_


.. _2881:

2881
	Turns off an an upper bound on how large the plan cache can get. (Upper bound introduced in 64-bit SQL 2000 to 
	solve a prob with the plan cache and adhoc sql). Does the flag still do anything?
	
	891707_
	

	
	






.. Official Links 

.. _BOL 2014: https://technet.microsoft.com/en-us/library/ms188396.aspx

.. _195565: http://support.microsoft.com/kb/195565/en-us

.. _325607: http://support.microsoft.com/kb/325607

.. _891707: https://support.microsoft.com/en-us/kb/891707

.. _3026083: http://support.microsoft.com/kb/3026083

 



.. MSFT Blog links


.. Non-MSFT bloggers


.. Connect links


.. Forums 

.. _StackOverflow_1: http://stackoverflow.com/questions/2596587/what-does-sql-server-trace-flag-253-do 

.. _DBAse_1: http://dba.stackexchange.com/questions/11693/is-there-an-equivalent-of-optionrecompile-or-with-recompile-for-an-entire

.. _DBAse_2: http://dba.stackexchange.com/questions/139659/trace-flag-2861-and-what-a-zero-cost-plan-actually-means


.. Other Links 

.. _Banerjee: http://troubleshootingsql.com/2014/01/20/sql-server-2012-trace-flags/

.. _Randal-SQL-SDB407: http://www.scribd.com/doc/109431789/Randal-SQL-SDB407-Undocumented

.. _SolarWindsDPA: http://logicalread.solarwinds.com/why-dpa-uses-sql-server-trace-flag-2861-and-zero-cost-plans-tl01/

.. _SQLMag_1: http://sqlmag.com/database-performance-tuning/avoid-using-trace-flag-2861-cache-zero-cost-query-plans