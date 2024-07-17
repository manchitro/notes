For meeting ID = 9811
To get current caching status:
```
SELECT *
FROM [Proxy].[ufnGetSettingStore]('meeting_tabulation_cache_running', '0', 9811, '0', 0)
WHERE [CampaignID] <> 0
```

if no rows exists, cache not found, need to cache
else if  `[Data] = 'TRUE'`, cache is in progress
else if `[Data] = 'FALSE'`, cache exisis (this is the current state which was last modified on 11th July)
- - -
To get last tabulation cache information:
```
SELECT TOP (1000) *
  FROM [ProxyVoting].[Proxy].[tblTabulationCache]
  where CampaignID = 9811
  order by TabulationDate desc
```
Each row contains cached tabulation calculation for each fund
Detailed [[Results]]
- - -
Since cache is almost a week old, it should automatically update whenever widget is loaded. Instead, it is fetching old cached data. Either:
- New cache is generated if current cache is old enough (Scheduled job)
- When actual vote data is updated, then somehow it is detected and cache is updated
- Cache is only updated manually
SP used to generate Tabulation Cache is: `uspCacheTabulationByCampaignID` which is called when 
- ***Create Meeting Tabulation Cache*** button is called on a meeting tools page
- There is a scheduled job which calls `uspCacheTabulation` (for all campaigns) everyday at 2 AM.
	- As well as for updating vote tracking cache: `uspCacheTabulationByCampaignID`
