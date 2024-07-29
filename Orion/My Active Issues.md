# [AP20-3788](https://nuarca.atlassian.net/browse/AP20-3788)
## Reduce shareholder portal Custom URL loading time 
### Status: In Progress (Complete AP20-3789 First)
#### ToDo
- Try to find another way to load fetched CSS properties on page (ask perplexity). If no other way can be found
	- [Perplexity suggested, among other things, the ngStyle directive](https://www.perplexity.ai/search/in-my-angular-app-i-have-to-fe-qq0lmz4nT3CxvPHuT5DhZQ)
	- Try `ngStyle`
- Continue with Inserting one by one with jQuery
#### Blocking
- Both ngStyle and jQuery is unable to work with media queries. So far, the only way to insert custom theme properties inside media queries is to create CSS text and inject into HTML. 
	- One approach is that we can probably trim most of the lines of the CSS where custom properties are not used.
		- This will only work if only those CSS lines are overridden from the main CSS file.
		- Will not work if overriding doesn't work and the only option is to replace the whole CSS.
	- Another approach is using Angular Breakpoint Directive, which will probably require a lot of revamp and will be time consuming
	- Ask perplexity how to inject dynamic CSS into @media queries
#### Example custom theme settings JSON
```
{
    "base_background": "#FFFFFF",
    "brand_color": "#EE3398",
    "brand_image": "base64",
    "logo_height": "100px",
    "logo_radius": "0%",
    "logo_path": "base64",
    "logo_path_retina": "143",
    "logo_width": "100px",
    "secondary_color": "#FFFFFF",
    "hideHelpButton": "false",
    "hideContactInfo": "true"
}
```
#### Investigation
- On [Local QA main login page](https://qa.web.proxyvoting.com:25005/), loading times widely varies between 9-24 seconds, most of which is spent on fetching `javascript` code
- On [Local QA custom login page](https://qa.web.proxyvoting.com:25005/masisa/Nuarca123), loading times were between 20-22 seconds
	- On same incognito window after initial loading: 17, 13, 15, 16, 28
	- On different incognito window: 17
		- With `/theme` API call taking the most time, almost 10 seconds
##### Conclusion
- Loading timely is not consistent and custom login page doesn't always take longer to load that main login page
- - -

# [AP20-3789](https://nuarca.atlassian.net/browse/AP20-3789)
## Allow meeting address to show without refreshing details
### Status: In Progress
#### ToDo
- Requirement: As an admin user when I enter meeting address I should see the address , without it getting removed, even if i move to another address field.
- [x] Check how and when address gets removed
	- [x] check a meeting with default email address and try to change
- [x] Remove auto-deleting of default address
%% ### Investigation
- `campaign-dialog.component.ts.mailAddressVerify()` if the campaign's address is set to the default address, then when the user changes any of the fields, each field that contains a property of the default address is set to empty.
- Default address values:
```
	this.cMail1Addr1 = 'AST Fund Solutions Proxy Services';
	this.cMail1Addr2 = '55 Challenger Road, Suite 201';
	this.cMail1City = 'Ridgefield Park';
	this.cMail1State = 'NJ';
	this.cMail1ZIP = '07660';
	this.cMail2Addr1 = '48 Wall Street';
	this.cMail2Addr2 = '22nd Floor';
	this.cMail2City = 'New York';
	this.cMail2State = 'NY';
	this.cMail2ZIP = '10005';
``` %%
- - -

# [AP20-3771](https://nuarca.atlassian.net/browse/AP20-3771)
## Show correct message when meeting is closed and shareholder tries to access the common/custom SHP link
### Status: Bug Fixing / Implementing new requirements
#### ToDo
- [x] Implement new message on custom url and main login page:
%% 	- [x] Test if both is showing correctly
		- Main login page message working
			- ![[Pasted image 20240728123427.png]]
		- Custom URL message is working
			- ![[Pasted image 20240728123525.png]]
	- ![[Pasted image 20240728092004.png]] %%
- [x] Display generic phone number on contact page when meeting is closed
%% 	- For active custom meeting contact page custom number is showing:
		- ![[Pasted image 20240728133747.png]]
	- For inactive custom meeting contact page generic number is showing:
		- ![[Pasted image 20240728134135.png]]
	- The contact us on right corner should show generic number and address and not the meeting specific number in case of custom URL. 
	- ![[Pasted image 20240728092319.png]] %%
- - -
# [AP20-3776](https://nuarca.atlassian.net/browse/AP20-3776)
## Show meeting date in meetings dropdown
### Status: Testing
%% #### ToDo
- [ ] [Match date format to that of meeting list page](https://nuarca.atlassian.net/browse/AP20-3776?focusedCommentId=22779)
	- ![[Pasted image 20240728092638.png]] %%
- - -
# [AP20-3045](https://nuarca.atlassian.net/browse/AP20-3045)
## Add time for adjournment time on admin page  
### Status: Testing
- - -

