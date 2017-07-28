# Clean Browser Cache and Recycle Bin
This Powershell script was created by by [Lemtek](https://github.com/lemtek/Powershell/blob/master/Clear_Browser_Caches) and has been edited with changes and additions from other users.

Powershell script to delete cache & cookies in Firefox, Chrome, Chromium, Opera & IE browsers. Also empty Recycle Bin for All Users

v2.7: 
* Borrowed Chromium and Opera Cleaning - Credit [Anst-foto](https://github.com/anst-foto/Powershell)
* Redone Recycle Bin cleaning. Will ask for confirmation at the start of the script then will clean All Users Recycle Bin - Credit [Chris Rakowitz](https://community.spiceworks.com/scripts/show_download/3677-empty-recycle-bins)

v2.6: 
* Fixes from Github which were not pulled from Master
* Fixed C:\users\\%username% could not be found if the profiledir points to another directory - Credit [Mahagon](https://github.com/Mahagon/Powershell)
* Amend Clear Internet Explorer Output - Credit [Watnabe](https://github.com/Watnabe/Powershell)
             
v2.5: 
* Added Disk Size, Free Space, % Free. Before and After - Code Borrowed from [Technet Article](https://gallery.technet.microsoft.com/scriptcenter/Clean-up-your-C-Drive-bc7bb3ed)
* Write to Text File
* Tabbed in code, cleaner to read
* Updated Alias' to Full Content for easier maintaince
    
v2.4: 
* Resolved *.default issue, issue was with the file path name not with *.default, but issue resolved

v2.3: 
* Added Cache2 to Mozilla directories but found that *.default is not working

v2.2: 
* Added Cyan colour to verbose output

v2.1: 
* Added the location 'C:\Windows\Temp\*' and 'C:\`$recycle.bin\'

v2: 
* Changed the retrieval of user list to dir the c:\users folder and export to csv

v1: 
* Compiled script

