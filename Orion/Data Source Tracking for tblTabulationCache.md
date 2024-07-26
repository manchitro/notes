### Columns of tblTabulationCache and their source
```
[CampaignID]                    BIGINT            <- 
[FundID]                        CHAR(10)          <- 
[TabulationDate]                DATETIME          <- 
^--MIN([TabulationDate]) FROM [Proxy].[ufnGetMeetingDateRange]
```
```
[RecordDateShare]               NUMERIC(16, 4)    <- 
^--SUM([OutStandingShares]) FROM [Proxy].[ufnGetTotalOutstandingSharesByCampaignIDAndSecurityID]([CampaignID], [SecurityID], @OfficialBroker);
```
```
[ForVotePerc]                   NUMERIC(16, 2)    <- 
^--MIN([ForVoteRepresent] FROM [Proxy].[vwProposalTabulationRegisterCache]
   ^--ForVotePerc=(TotalForSharesVoted + TotalAgainstOrWithholdSharesVoted + TotalAbstainSharesVoted + BrokerNonVoteTotalForSharesVoted​)×100 
```
```
[ForVoteOSPerc]                 NUMERIC(16, 2)    <- 
^--MIN([ForVoteOSPerc] = [TotalForSharesVoted] FROM @FundProposalDetails / [RecordDateShares] FROM Above)
```
```
[ShareVote]                     NUMERIC(16, 4)    <- 
^--@FundSummaryData
^--@SecuritySummaryData
^--[TotalSharesVoted] FROM @SecurityVoteSummary
^--[SharesVoted] FROM @RegisterSHVoteSummary UNION [MaxProposalVote] FROM @MaxMinBrokerProposalVote
^--SUM([Share]) FROM [vwREGShareholder] UNION 
[ShareVotePerc]                 NUMERIC(16, 2)    <- 
[ShareUnVoted]                  NUMERIC(16, 4)    <- 
^--@FundSummaryData
   ^--@SecuritySummaryData
[ShareUnVotedPerc]              NUMERIC(16, 4)    <- 
[ActualVote]                    NUMERIC(16, 4)    <- 
[ActualVotePerc]                NUMERIC(16, 2)    <- 
[BrokerShareNonVoted]           NUMERIC(16, 4)    <- 
[BrokerShareNonVotedPerc]       NUMERIC(16, 2)    <- 
[BrokerShareNonVotedOSPerc]     NUMERIC(16, 2)    <- 
[MeetingQuorumThresholdPercent] NUMERIC(5, 2)     <- 
[ShareNeededToPass]             NUMERIC(16, 4)    <- 
[ShareNeededToPassPerc]         NUMERIC(16, 2)    <- 
[ShareDailyChange]              NUMERIC(16, 4)    <- 
[ShareDailyChangePerc]          NUMERIC(16, 2)    <- 
[ReturnShareChangeSinceDate]    DATETIMEOFFSET(0) <- 
[Passed]                        BIT               <- 
[CalculationTypeCount]          INT               <- 
[OfficialBroker]                BIT               <- 
[Tabulated]                     BIT               <- 
[CreatedBy]                     VARCHAR(50)       <- 
[CreatedDate]                   DATETIMEOFFSET(0) <- 
```
