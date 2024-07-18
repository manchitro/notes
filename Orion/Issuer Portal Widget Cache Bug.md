For meeting ID = 9811
To get current caching status:
```
SELECT *
FROM [Proxy].[ufnGetSettingStore]('meeting_tabulation_cache_running', '0', 9811, '0', 0)
WHERE [CampaignID] <> 0
```

if no rows exists, cache not found, need to cache
else if  `[Data] = 'TRUE'`, cache is in progress
else if `[Data] = 'FALSE'`, cache exists (this is the current state which was last modified on 11th July)
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
Since cache is almost a week old, it should automatically update whenever widget is loaded. Instead, it is fetching old cached data.
SP used to generate Tabulation Cache is: `uspCacheTabulationByCampaignID` which is called when 
- ***Create Meeting Tabulation Cache*** button is called on a meeting tools page
	- Cache refresh was executed at `2024-07-17 11:49:28 -05:00` and ended at `2024-07-17 12:05:35 -05:00` took around `15 minutes`
- There is a scheduled job which calls `uspCacheTabulation` (for all campaigns) everyday at 2 AM.
- - -
Submitted vote for:

| Registered | shirin134 | 714221687518 | MELVIN HAMRE | VERNON HILLS | IL  | 85,144.0000 | Web/Internet | 7/17/2024 |       |
| ---------- | --------- | ------------ | ------------ | ------------ | --- | ----------- | ------------ | --------- | ----- |
| Registered | shirin134 | 147800290507 | VERN ALTEMUS | CARENCRO     | LA  | 61,292.0000 | Web/Internet | 7/17/2024 | - - - |
And then tabulation and vote tracking cache are refreshed manually but cache was not updated according to new votes. (Probably blockchain issue, will try again after updating blockchain status)
- - -
After updating all blockchain status to `40000` and manually refreshing cache once more. new data is coming to widget:

| Fund Name             |     | % Voted of O/S | % For  | % For of O/S | % Needed of O/S | % Change  | Status  |
| --------------------- | --- | -------------- | ------ | ------------ | --------------- | --------- | ------- |
| "Fund Quot Test"      |     | 14,713.94%     | 41.24% | 612.92%      | --              | 1,464.36% | Passed  |
| Fund - Mediant - Test |     | 188.11%        | 80.95% | 152.28%      | --              | 0.00%     | Passed  |
| Fund - mixed          |     | 0.00%          | 0.00%  | 0.00%        | 53.00%          | 0.00%     | Not Yet |
- - -
After submitting another vote:

| NOBO<br><br> | shirin156 | 6364246053   | MICHELLE SPEED   | SETAUKET | NY  | 1,334,227.0000 | Web/Internet | 7/18/2024 |     |
| ------------ | --------- | ------------ | ---------------- | -------- | --- | -------------- | ------------ | --------- | --- |
| Registered   | shirin134 | 265375878439 | LAVERNE RANDOLPH | CHESWICK | PA  | 50,642.0000    | Web/Internet | 7/18/2024 |     |
The widget remains unchanged. After updating blockchain status to `400000`:

| Fund Name             |     | % Voted of O/S | % For  | % For of O/S | % Needed of O/S | % Change  | Status  |
| --------------------- | --- | -------------- | ------ | ------------ | --------------- | --------- | ------- |
| "Fund Quot Test"      |     | 15,220.36%     | 41.24% | 612.92%      | --              | 1,970.78% | Passed  |
| Fund - Mediant - Test |     | 188.11%        | 80.95% | 152.28%      | --              | 0.00%     | Passed  |
| Fund - mixed          |     | 0.00%          | 0.00%  | 0.00%        | 53.00%          | 0.00%     | Not Yet |
