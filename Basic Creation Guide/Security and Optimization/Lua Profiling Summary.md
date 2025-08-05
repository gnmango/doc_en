# Course Introduction
Let's take a look at what information is available in the Lua Profiling Summary Report.

##### Reference Guide
[Optimize Using Lua Profiler]

# Lua Profiling Summary Report
Clicking **File - Export Report** in the Lua Profiler lets you export the Lua Profiling Summary Report to XML. 
The following is a list of the information available in the Summary Report:

| Item | Description |
| --- | --- |
|	Name	|	Context name	|
|	FrameCount	|	Number of frames measured	|
|	StartTime	|	Profiling Start Tick 	|
|	EndTime	|	Profiling Stop Tick <br><ul> <li>Tick read in 10 ns increments</li> <li>Profiling time can be calculated as (EndTime - StartTime) / 10,000,000 seconds</li></ul> |
|	AvgElapsedTime	|	Average time per frame (in ms)	|
|	PeakElapsedTime	|	Max. time per frame (in ms)	|
|	AvgNetworkSend	|	Average network Tx traffic per frame (in bytes)	|
|	PeakNetworkSend	|	Max. network Tx traffic per frame (in bytes)	|
|	AvgNetworkRecv	|	Average network Rx traffic per frame (in bytes)	|
|	PeakNetworkRecv	|	Max. network Rx traffic per frame (in bytes)	|
|	AvgManagedObjectCount	|	Average number of objects managed by Lua	|
|	PeakManagedObjectCount	|	Max. number of objects managed by Lua	|
|	AvgTotalLuaMemory	|	Average amount of memory used by Lua (in bytes)	|
|	PeakTotalLuaMemory	|	 Max. amount of memory used by Lua (in bytes)	|
|	SampleSummary	|	Profiling summary call tree for each sample	|

# Sample Profiling Summary
The following is a list of the information available in SampleSummary:

| Item | Description |
| --- | --- |
|	Name	|	Name of sample	|
|	Category	|	Type of sample	|
|	CallCount	|	Number of calls	|
|	AvgElapsedTime	|	Average execution time for samples (in ms)	|
|	PeakElapsedTime	|	Max. execution time for samples (in ms)	|
|	AvgSelfElapsedTime	|	Average execution time for samples (in ms), excluding the execution time of child samples	|
|	PeakSelfElapsedTime	|	Max. execution time for samples (in ms), excluding the execution time of child samples	|
|	AvgLuaMemory	|	Average volume of Lua memory assigned (in bytes)	|
|	PeakLuaMemory	|	Max. volume of Lua memory allocated (in bytes)	|
|	AvgSelfLuaMemory	|	Average volume of Lua memory allocated, excluding that of Lua memory allocated by child samples (in bytes)	|
|	PeakSelfLuaMemory	|	Max. volume of Lua memory allocated, excluding that of Lua memory allocated by child samples (in bytes)	|
|	AvgNetworkUsage	|	Average network Tx traffic (in bytes)	|
|	PeakNetworkUsage	|	Max. network Tx traffic (in bytes)	|