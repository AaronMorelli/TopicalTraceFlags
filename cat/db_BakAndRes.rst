==================
Backup and Restore
==================

Flags in this category fall into the below groupings:

#. `Informational`_ --> Flags that return info about the BACKUP or RESTORE process.
#. `Functionality Toggles`_ --> Flags that modify how BACKUP or RESTORE behavior.
#. `Fixes and Past Relevance`_ --> Fix flags and flags which are no longer relevant.


See also: 

TODO: 3028 may need to be moved to a fix flag section, ditto for 3057, 3111, 3117 
TODO: 3607 should at least be referenced in the TranLog Recovery section, and in the CheckDB Corruption section.
TODO: 9958 should at least be referenced in the TranLog Recovery section
TODO: does 3222 belong here, or as a See Also anywhere else?

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
	* - :ref:`Info <InfoBakRes>`
	  - 
	* - 3001_
	  - Suppresses logging to msdb.dbo.backuphistory
	* - 3004_
	  - Prints info (e.g. file initialization) re: BACKUP/RESTORE operations
	* - 3014_
	  - Prints info about config options used during BACKUP.
	* - 3210_
	  - Prints info about Async Disk Pool thread collision and wait times.
	* - 3212_
	  - Prints "backup stats" to the SQL log.
	* - 3213_
	  - Prints info about BACKUP param values used, e.g. Max Transfer Size
	* - 3216_
	  - Prints info about RESTORE internals.
	* - \* 3226_
	  - Suppresses BACKUP messages to the error log.
	* - 3400_
	  - Appears to print out various info about RESTORE process.
	* - ...
	  - 
	* - :ref:`Func <FuncBakRes>`
	  - 
	* - 3034_
	  - Overrides server default and forces backup compression
	* - 3035_
	  - Overrides server default and avoids backup compression
	* - 3039_
	  - Allows VDI backups to be affected by the server default compression setting.
	* - 3042_
	  - Causes BACKUP to allocate size of .bak file as needed instead of up-front.
	* - 3205_
	  - Disables hardware compression for tape drivers during BACKUP or RESTORE.
	* - 3222_
	  - Disables read-ahead used during recovery operation during roll-forward.
	* - 3422_
	  - Enables auditing of t-log records during rollback or log recovery.
	* - 3607_
	  - Lets SQL Server open the master database w/out running recovery on it.
	* - 3608_
	  - SQL Server only starts up master, not any other DB.
	* - 3609_
	  - Skips creation of tempdb at startup.
	* - 3660_
	  - Forces database recovery at SQL Server startup even for auto-close DBs.
	* - ...
	  - 
	* - :ref:`Fix/PastRel <FixPastRel7>`
	  - 
	* - 3023_
	  - Enables CHECKSUM option as default for all BACKUPs.
	* - 3028_
	  - Enables a hotfix for problems backing up to tape.
	* - 3031_
	  - Turns NO_LOG and TRUNCATE_ONLY into checkpoints.
	* - 3057_
	  - Allows log backups w/sector size 512 to be restore on sector size 4096.
	* - 3101_
	  - Causes RESTORE to bypass a CDC upgrade operation.
	* - 3111_
	  - Enables a fix for backing up very large T-log files.
	* - 3117_
	  - Enables a fix when restoring from a file/FG-based snapshot backup.
	* - 3231_
	  - Turns NO_LOG/TRUNCATE_ONLY into no-ops/log clears based on recov mode.
	* - 9109_
	  - Enables a workaround for restoring a DB w/query notifs and NEW_BROKER.
	* - 9958_
	  - Enables a fix for a bug restoring a t-log with hekaton records.


	 
.. This comment line is as long as we would ever want the short desc to be in the table above.

|

.. _InfoBakRes:
	 
Informational
-------------

.. _3001: 

3001 (Info)
	Erland: "suppresses logging to msdb.backuphistory...undocumented with all that means. On the 
	other hand, I got [it] from a Microsoft engineer who said it was OK to share them."
	
	Erland_1_ 

.. _3004:

3004 (Info)
	Prints trace info to the SQL error log about RESTORE (and BACKUP?) operations and can be 
	used to view file initialization; use with 3605 to direct to error log
	
	CSS_1_ | SQLPFE_1_ | SQLMunkee_ 

.. _3014:
	
3014 (Info)
	Prints info to the error log about config option values chosen during the BACKUP command. 
	In the CSS blog post, used with 3213 (not sure how they are different; more testing is needed). 
	
	CSS_2_ 


.. _3210: 

3210 (Info)
	Bob Ward PASS 2014 IO talk: prints information about "collisions and wait times" that occur between 
	the various "Asynchronous Disk Pool" threads during BACKUP (what about RESTORE?) operations.
	
	<links needed>

	
.. _3212:

3212 (Info)
	Prints "Backup stats" to the SQL log.
	
	Nacho_1_ 

.. _3213:
	
3213 (Info)
	Prints info about BACKUP parameter values used, especially regarding Buffer size/number and 
	Max Transfer size.
	
	CSS_2_ | CSS_3_

	
.. _3216:

3216 (Info)
	Prints info about RESTORE internals. Only seems to print to the error log (TF 3605 is required). 
	Not able to find an official link.
	
	JamesSQL_ 


.. _3226:
	
3226 ``Doc2008`` (Info)
	`BOL 2014`_: "By default, every successful backup operation adds an entry in the SQL Server error 
	log and in the system event log. If you create very frequent log backups, these success messages 
	accumulate quickly, resulting in huge error logs in which finding other messages is problematic.
	
	With this trace flag, you can suppress these log entries. This is useful if you are running 
	frequent log backups and if none of your scripts depend on those entries."

	StorEng_1_ | PRand_1_


.. _3400:
	
3400 (Info)
	Connect: appears (based on context) to print information re: the RESTORE process. 
	BWard PASS 2014 IO talk: explained as printing out "checkpoint pages/sec" (to the Error Log, 
	presumably)
	
	Connect_1_


|

.. _FuncBakRes: 
	 
Functionality Toggles
---------------------

.. _3034:
	
3034
	(is this just for VDI?) overrides the server default, and thus always forces backup compression 
	unless the backup command had the no_compression clause explicitly present.
	
	Nacho_2_ 


.. _3035: 

3035
	(is this just for VDI?) overrides the server default to always avoid compression, unless the 
	backup command explicitly uses the compression clause. If both 3034 and 3035 are enabled, 3035 
	takes precedence.
	
	Nacho_2_


.. _3039:
	
3039
	As long as the SQL edition supports backup compression, this will allow VDI backups to be affected 
	by the default compression setting just as non-VDI BACKUP commands are affected.
	
	Nacho_2_


.. _3042:	
	
3042 ``Doc2012``
	`BOL 2014`_: "Bypasses the default backup compression pre-allocation algorithm to allow the backup 
	file to grow only as needed to reach its final size. This trace flag is useful if you need to save 
	on space by allocating only the actual size required for the compressed backup. Using this trace 
	flag might cause a slight performance penalty (a possible increase in the duration of the backup 
	operation)."
	
	The KB article discusses the algorithm used to estimate space when the TF is NOT on.
	
	2001026_ | CSS_4_ 


.. _3205:

3205 ``Doc2005``
	`BOL 2014`_: "By default, if a tape drive supports hardware compression, either the DUMP or BACKUP 
	statement uses it. With this trace flag, you can disable hardware compression for tape drivers. This 
	is useful when you want to exchange tapes with other sites or tape drives that do not support compression."
	
	
.. _3222:	

3222
	Disables the read ahead that is used by the recovery operation during roll forward operations.
	
	268081_

	
.. _3422:
	
3422
	PRand: "Cause auditing of transaction log records as they're read (during transaction rollback 
	or log recovery). This is useful because there is no equivalent to page checksums for transaction 
	log records and so no way to detect whether log records are being corrupted." 
	[There is a CPU hit for turning this on].

	`IO Basics, chapter 2`_ | PRand_2_


.. _3607:
	
3607
	Khen2005, page 80: lets SQL open master w/out running recovery on it. Other sources say that 
	SQL doesn’t try to "start up" master. The differences in wording may not be important.
	
	Nacho_1_ 

	
.. _3608:
	
3608 ``Doc2008``
	`BOL 2014`_: "Prevents SQL Server from automatically starting and recovering any database 
	except the master database. If activities that require tempdb are initiated, then model is 
	recovered and tempdb is created. User databases will be started and recovered when accessed. 
	Some features, such as snapshot isolation and read committed snapshot, might not work."
	
	Nacho_1_ | Nacho_3_ 
	

.. _3609: 

3609
	Skips the creation of the tempdb database at startup. Use this trace flag if the device 
	or devices on which tempdb resides are problematic or problems exist in the model database.
	
	PRand_3_ | Nacho_1_ 


.. _3660:
	
3660
	W/o this flag, for DBs that have Auto_Close=true and for DBs on Express Edition, DB recovery 
	is normally deferred until first user access when SQL starts up. This TF forces DB recovery to 
	always run (well, only for DBs that actually need recovery done) at SQL Server startup.
	
	Nacho_4_ 
	


|

.. _FixPastRel7:

Fixes and Past Relevance
------------------------
These flags either are old and irrelevant for modern builds, appear only in CTPs, or enable a 
fix in a CU but are baselined in a later service pack or release.

.. _3023:

3023 ``Doc2014``
	`BOL 2014`_: "Enables CHECKSUM option as default for BACKUP command. Note: Beginning with SQL 
	Server 2014 this behavior is controlled by setting the backup checksum default configuration option."
	
	2656988_


.. _3028:
	
3028
	Enables a hotfix for a problem encountered when backing up to tape with specific backup options.
	
	940379_

	
.. _3031:

3031
	Will turn the NO_LOG and TRUNCATE_ONLY options into checkpoints in all recovery modes (applicable 
	to SQL 2005). 
	
	PRand_4_


.. _3057:

3057
	Enables a hotfix change that allows a log backup that was taken on a t-log file hosted on a drive 
	with "Bytes per physical sector"=512 to be restored onto a log file/drive that has "Bytes per physical 
	sector"=4096.
	
	2987585_


.. _3101:
	
3101
	Causes the RESTORE process to bypass a CDC upgrade operation that can cause slow RESTORE operations 
	under certain conditions.
	
	2567366_

	
.. _3111:

3111
	"FIX: Backup or Restore Using Large Transaction Logs May Return Error 3241" 
	Causes LogMgr::ValidateBackedupBlock to be skipped during backup and restore operations, 
	allowing backups of very large T-logs to succeed. 
	
	297104_

	
.. _3117:
	
3117
	Enables a fix in 2005 when restoring from a file- or filegroup-based snapshot backup. 
	3117 causes the restore process to use an approach found in SQL 2000 rather than 2005's logic. 
	
	915385_


.. _3231:

3231
	SQL 2000/2005 - Will turn the NO_LOG and TRUNCATE_ONLY options into no-ops in FULL/BULK_LOGGED 
	recovery mode, and will clear the log in SIMPLE recovery mode.
	
	PRand_4_ | KTripp_1_ 


.. _9109:

9109
	Used to workaround a problem with query notifications and restoring a DB with the NEW_BROKER 
	option enabled.
	
	2483090_


.. _9958:
	
9958
	Enables a fix that allows the restore of a Hekaton transaction log backup when a certain 
	bug is hit.
	
	3171002_




.. Official Links 

.. _BOL 2008: https://technet.microsoft.com/en-us/library/ms188396(v=sql.100).aspx

.. _BOL 2014: https://technet.microsoft.com/en-us/library/ms188396.aspx

.. _BOL 2016: https://technet.microsoft.com/en-us/library/ms188396.aspx

.. _ALTER DATABASE file and filegroup options: https://msdn.microsoft.com/en-us/library/bb522469.aspx

.. _ALTER DATABASE SET Options: https://msdn.microsoft.com/en-us/library/bb522682.aspx

.. _IO Basics, chapter 2: https://technet.microsoft.com/en-us/library/cc917726.aspx

.. _268081: http://support.microsoft.com/kb/268081/en-us

.. _297104: http://support.microsoft.com/kb/297104/en-us

.. _915385: https://support.microsoft.com/en-us/kb/915385

.. _940379: http://support.microsoft.com/kb/940379/en-us

.. _2001026: http://support.microsoft.com/kb/2001026

.. _2483090: http://support.microsoft.com/kb/2483090/en-us

.. _2567366: http://support.microsoft.com/kb/2567366/en-us

.. _2656988: http://support.microsoft.com/kb/2656988

.. _2987585: http://support.microsoft.com/kb/2987585/en-us

.. _3171002: https://support.microsoft.com/en-us/kb/3171002




.. MSFT Blog links

.. _CSS_1_: http://blogs.msdn.com/b/psssql/archive/2008/01/23/how-it-works-what-is-restore-backup-doing.aspx

.. _CSS_2: http://blogs.msdn.com/b/psssql/archive/2008/02/06/how-it-works-how-does-sql-server-backup-and-restore-select-transfer-sizes.aspx

.. _CSS_3: http://blogs.msdn.com/b/psssql/archive/2008/01/28/how-it-works-sql-server-backup-buffer-exchange-a-vdi-focus.aspx

.. _CSS_4: http://blogs.msdn.com/b/psssql/archive/2011/08/11/how-compressed-is-your-backup.aspx?

.. _Nacho_1: http://blogs.msdn.com/b/ialonso/archive/2012/10/24/why-does-restoring-a-database-needs-tempdb.aspx

.. _Nacho_2: http://blogs.msdn.com/b/ialonso/archive/2012/02/24/vdi-backups-and-backup-compression-default.aspx

.. _Nacho_3: http://blogs.msdn.com/b/ialonso/archive/2012/05/04/the-seven-reasons-why-auto-update-stats-event-will-not-trigger-despite-how-many-modifications-affect-any-of-the-tables-involved-in-a-compiled-plan.aspx

.. _Nacho_4: http://blogs.msdn.com/b/ialonso/archive/2012/10/09/how-much-is-crash-recovery-parallelized-in-which-order-are-databases-recovered.aspx

.. _SQLPFE_1: http://blogs.msdn.com/b/sql_pfe_blog/archive/2009/12/23/how-and-why-to-enable-instant-file-initialization.aspx

.. _StorEng_1: http://blogs.msdn.com/b/sqlserverstorageengine/archive/2007/10/30/when-is-too-much-success-a-bad-thing.aspx



.. Non-MSFT bloggers

.. _JamesSQL: http://jamessql.blogspot.com/2013/07/trace-flag-for-backup-and-restore.html

.. _KTripp_1: http://www.sqlskills.com/blogs/kimberly/understanding-backups-and-log-related-trace-flags-in-sql-server-20002005-and-2008/

.. _SQLMunkee: http://www.sqlmunkee.com/2014/04/sql-trace-flag-3004-and-backup-database.html?m=1

.. _PRand_1: http://www.sqlskills.com/blogs/paul/fed-up-with-backup-success-messages-bloating-your-error-logs/

.. _PRand_2: http://www.sqlskills.com/blogs/paul/how-to-tell-if-the-io-subsystem-is-causing-corruptions/

.. _PRand_3: http://www.sqlskills.com/blogs/paul/search-engine-qa-18-whats-the-current-uptime-of-sql-server/

.. _PRand_4: http://www.sqlskills.com/blogs/paul/backup-log-with-no_log-use-abuse-and-undocumented-trace-flags-to-stop-it/



.. Connect links

.. _Connect_1: http://connect.microsoft.com/SQLServer/feedback/details/392158/recovery-portion-of-sql-2008-restore-takes-much-longer-than-normal-when-restoring-from-sql-2005-backup


.. Forums 

.. _Erland_1: http://bytes.com/topic/sql-server/answers/162385-how-do-i-prevent-sql-2000-posting-message-event-viewer-application-log


.. Other Links 
