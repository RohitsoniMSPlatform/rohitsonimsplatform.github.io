Recently we had some cases where AppFabric caching service was crashing with error "An entry with the same key already exists."
Issue: In the AppFabric event logs (Applications and Services Logs > Microsoft > Windows > Application Server-System Services), we'll see below error logged.
AppFabric Caching service crashed with exception {System.ArgumentException: An entry with the same key already exists.
at System.ThrowHelper.ThrowArgumentException(ExceptionResource resource)
at System.Collections.Generic.TreeSet`1.AddIfNotPresent(T item)
at System.Collections.Generic.SortedDictionary`2.Add(TKey key, TValue value)
at Microsoft.Fabric.Data.PartitionTable.UpdateEntry(LookupTableEntry newEntry)
at Microsoft.Fabric.Data.PM.PMPartitionTable..ctor(PartitionManager pm, IList`1 partitions, Int64 savedVersion, Object lockObject)
at Microsoft.Fabric.Data.PM.PartitionCache..ctor(PartitionManager pm, IPartitionManagerStore pmStore, LoadTable loadTable, IList`1 partitions, Int64 savedLookupVersion)
at Microsoft.Fabric.Data.PM.PartitionManager.ProcessLoadPM(Object state)
at Microsoft.Fabric.Common.IOCompletionPortWorkQueue.Invoke(WaitCallback callback, Object state)
at Microsoft.Fabric.Common.IOCompletionPortWorkQueue.WorkerThreadStart()
at System.Threading.ExecutionContext.RunInternal(ExecutionContext executionContext, ContextCallback callback, Object state, Boolean preserveSyncCtx)
at System.Threading.ExecutionContext.Run(ExecutionContext executionContext, ContextCallback callback, Object state, Boolean preserveSyncCtx)
at System.Threading.ExecutionContext.Run(ExecutionContext executionContext, ContextCallback callback, Object state)
at System.Threading.ThreadHelper.ThreadStart()}. Check debug log for more information
 
Reason: This is happening at PartitionTable.UpdateEntry, which indicates a problem with the cache partitions.
 
Solution:
We would need to wipe the partitions out and rebuild them.
-- Exported and Imported cache cluster config…
1. Open PowerShell as Administrator.
2. Use-CacheCluster.
3. Export-CacheClusterConfig c:\output.txt
4. Open output.txt with Notepad.
5. Find and replace <dataCache size=”Medium”> to <dataCache size=”Small”>.
6. Find and replace <caches partitionCount=”256″> to <caches partitionCount=”32″>.
7. Save and close output.txt.
8. Go back to PowerShell as Administrator.
9. Run: Stop-CacheCluster to ensure that AppFabric Distributed Cache Service is not running.
10. Run: Import-CacheClusterConfig c:\output.txt.
11. Run: Start-CacheCluster and ensure that all cache hosts are started.
12. Wait a few minutes to allow the cache hosts to start completely.
 
Select one of the following three options to optimize performance based on cluster size. This setting is available only if the New Cluster control is selected.
	• Small [1-5 computers]
	• Medium [6-15 computers]
	• Large [> 15 computers]
Reference link: https://msdn.microsoft.com/en-us/library/ff637694%28v=azure.10%29.aspx?f=255&MSPPError=-2147217396
