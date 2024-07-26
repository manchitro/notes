## ToDo List:
1. check if `ufnGetCacheTabulationStatus` ever returns that cache needs refreshing (returns `> 0`) 
	1. it might always be returning `0`, which causes older entries in `tblTabulationCache` entries to not be deleted when refreshing cache
2. In `ufnGetMeetingDateRange`, what is `CTEDateRange`?
## Flowchart of UspCacheTabulationByCampaignId
```mermaid
flowchart TD

	%% NODES %%
	start[Start]
	set_running_true[4. Set campaign's cache\n running status to TRUE]
	check_table_tab_cache{5. Campaign has entry\n in `tblTabulationCache`}
	check_tab_status[6. Check if data changed from last run\n and cache needs refreshing using\n `ufnGetCacheTabulationStatus` **see ToDo 1**]
	run_ufnGetCacheTabulationStatus{Need to \n refresh cache?}
	delete_cache_entries[7. Delete entries from TabulationCache and \n ProposalTabulationRegisterCache tables]
	get_funds[8. Get Funds List for Campaign]
	calc_date[9. Calculate Tabulation Date and\n 10. `DayBeforeSelectedDate` \n `ufnGetMeetingDateRange` **see ToDo 2**]
	check_table_tab_cache_and_create{11. Campaign has previous\n tabulations calculated in \n `tblTabulationCache`}
	create_tab_entry[12. Create empty `tblTabulationCache` \n entry for campaign\nwith calculated tabulation date]
	last_day_tabulation_exists{13. Is there yesterday's\n tabulation in\n `tblTabulationCache`?}
	create_tab_entry_yesterday[14. Create empty `tblTabulationCache` \n entry for campaign with yesterday's date\n as yet to be calculated]
	create_untabulated_per_fund[15. Set each fund's\n entry in `tblTabulationCache`\n as yet to be calculated]
	uspCacheVoteFundSummaryData[16. While there are yet to be\n calculated rows, for each,\n 17. select MIN of TabulationDate and\n 18. call `uspCacheVoteFundSummaryData`\n and 19. mark as calculated\n.]
	set_running_false[20. Set campaign's cache\n running status to FALSE]

	%% LINKS %%
	start --> set_running_true --> check_table_tab_cache
	check_table_tab_cache -- yes --> check_tab_status -->
		run_ufnGetCacheTabulationStatus -- yes (returns > 0) --> delete_cache_entries --> get_funds
		run_ufnGetCacheTabulationStatus -- no (returns 0) --> get_funds
	check_table_tab_cache -- no --> get_funds
	get_funds --> calc_date --> check_table_tab_cache_and_create
	check_table_tab_cache_and_create -- yes --> last_day_tabulation_exists
	check_table_tab_cache_and_create -- no --> create_tab_entry --> uspCacheVoteFundSummaryData
	last_day_tabulation_exists -- no --> create_tab_entry_yesterday --> uspCacheVoteFundSummaryData
	last_day_tabulation_exists -- yes --> create_untabulated_per_fund --> uspCacheVoteFundSummaryData
 	uspCacheVoteFundSummaryData --> set_running_false --> finish[End]
```
