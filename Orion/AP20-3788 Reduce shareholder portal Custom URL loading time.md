- - - 
source: https://nuarca.atlassian.net/browse/AP20-3788
## Description

As a shareholder portal user, when I use custom meeting URL, I should be redirected to login page quicker without showing the generic login page.
- - -
Solution: Instead of replacing theme entries on base.txt file, fetch the theme settings to the frontend and use jquery to find and replace by \#id