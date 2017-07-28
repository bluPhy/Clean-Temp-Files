#Calling Powershell as Admin and setting Execution Policy to Bypass to avoid Cannot run Scripts error
([switch]$Elevated)
function CheckAdmin {
    $currentUser = New-Object Security.Principal.WindowsPrincipal $([Security.Principal.WindowsIdentity]::GetCurrent())
    $currentUser.IsInRole([Security.Principal.WindowsBuiltinRole]::Administrator)
}
if ((CheckAdmin) -eq $false) {
    if ($elevated) {
        # could not elevate, quit
    }
    else {
        Start-Process powershell.exe -Verb RunAs -ArgumentList ('-noprofile -ExecutionPolicy Bypass -file "{0}" -elevated' -f ($myinvocation.MyCommand.Definition))
    }
    Exit
}

# Rename Title Window
$host.ui.RawUI.WindowTitle = "Clean Browser Temp Files"

Function Cleanup {
    # Set Date for Log
    $LogDate = Get-Date -format "MM-d-yy-HHmm"

    # Ask for Confirmation to Empty Recycle Bin for All Users
    $CleanBin = Read-Host "Would you like to empty the Recycle Bin for All Users? (Y/N)"

    # Get Disk Size
    $Before = Get-WmiObject Win32_LogicalDisk | Where-Object { $_.DriveType -eq "3" } | Select-Object SystemName,
    @{ Name = "Drive" ; Expression = { ( $_.DeviceID ) } },
    @{ Name = "Size (GB)" ; Expression = {"{0:N1}" -f ( $_.Size / 1gb)}},
    @{ Name = "FreeSpace (GB)" ; Expression = {"{0:N1}" -f ( $_.Freespace / 1gb ) } },
    @{ Name = "PercentFree" ; Expression = {"{0:P1}" -f ( $_.FreeSpace / $_.Size ) } } |
        Format-Table -AutoSize | Out-String

    Start-Transcript -Path C:\Windows\Temp\$LogDate.log

    Write-Host -ForegroundColor Green "Getting the list of Users1`n"
    # Write Information to the screen
    Write-Host -ForegroundColor Yellow "Exporting the list of Users to $env:USERPROFILE\users.csv`n"
    # List the users in c:\users and export to the local profile for calling later
    Get-ChildItem C:\Users | Select-Object Name | Export-Csv -Path $env:USERPROFILE\users.csv -NoTypeInformation
    $list = Test-Path $env:USERPROFILE\users.csv

    Write-Host -ForegroundColor Green "Beginning Script...`n"

    if ($list) {

        # Clear Firefox Cache
        Write-Host -ForegroundColor Green "Clearing Firefox Cache`n"
        Import-CSV -Path $env:USERPROFILE\users.csv -Header Name | ForEach-Object {
            Remove-Item -path "C:\Users\$($_.Name)\AppData\Local\Mozilla\Firefox\Profiles\*.default\cache\*" -Recurse -Force -EA SilentlyContinue -Verbose
            Remove-Item -path "C:\Users\$($_.Name)\AppData\Local\Mozilla\Firefox\Profiles\*.default\cache\*.*" -Recurse -Force -EA SilentlyContinue -Verbose
            Remove-Item -path "C:\Users\$($_.Name)\AppData\Local\Mozilla\Firefox\Profiles\*.default\cache2\entries\*.*" -Recurse -Force -EA SilentlyContinue -Verbose
            Remove-Item -path "C:\Users\$($_.Name)\AppData\Local\Mozilla\Firefox\Profiles\*.default\thumbnails\*" -Recurse -Force -EA SilentlyContinue -Verbose
            Remove-Item -path "C:\Users\$($_.Name)\AppData\Local\Mozilla\Firefox\Profiles\*.default\cookies.sqlite" -Recurse -Force -EA SilentlyContinue -Verbose
            Remove-Item -path "C:\Users\$($_.Name)\AppData\Local\Mozilla\Firefox\Profiles\*.default\webappsstore.sqlite" -Recurse -Force -EA SilentlyContinue -Verbose
            Remove-Item -path "C:\Users\$($_.Name)\AppData\Local\Mozilla\Firefox\Profiles\*.default\chromeappsstore.sqlite" -Recurse -Force -EA SilentlyContinue -Verbose
        }
        Write-Host -ForegroundColor Yellow "Done...`n"

        # Clear Google Chrome
        Write-Host -ForegroundColor Green "Clearing Google Chrome Cache`n"
        Import-CSV -Path $env:USERPROFILE\users.csv -Header Name | ForEach-Object {
            Remove-Item -path "C:\Users\$($_.Name)\AppData\Local\Google\Chrome\User Data\Default\Cache\*" -Recurse -Force -EA SilentlyContinue -Verbose
            Remove-Item -path "C:\Users\$($_.Name)\AppData\Local\Google\Chrome\User Data\Default\Cache2\entries\*" -Recurse -Force -EA SilentlyContinue -Verbose
            Remove-Item -path "C:\Users\$($_.Name)\AppData\Local\Google\Chrome\User Data\Default\Cookies" -Recurse -Force -EA SilentlyContinue -Verbose
            Remove-Item -path "C:\Users\$($_.Name)\AppData\Local\Google\Chrome\User Data\Default\Media Cache" -Recurse -Force -EA SilentlyContinue -Verbose
            Remove-Item -path "C:\Users\$($_.Name)\AppData\Local\Google\Chrome\User Data\Default\Cookies-Journal" -Recurse -Force -EA SilentlyContinue -Verbose
            Remove-Item -path "C:\Users\$($_.Name)\AppData\Local\Google\Chrome\User Data\Default\JumpListIconsOld" -Recurse -Force -EA SilentlyContinue -Verbose
            # Comment out the following line to remove the Chrome Write Font Cache too.
            # Remove-Item -path "C:\Users\$($_.Name)\AppData\Local\Google\Chrome\User Data\Default\ChromeDWriteFontCache" -Recurse -Force -EA SilentlyContinue -Verbose
        }
        Write-Host -ForegroundColor Yellow "Done...`n"

        # Clear Internet Explorer
        Write-Host -ForegroundColor Yellow "Clearing Internet Explorer Cache`n"
        Import-CSV -Path $env:USERPROFILE\users.csv | ForEach-Object {
            Remove-Item -path "C:\Users\$($_.Name)\AppData\Local\Microsoft\Windows\Temporary Internet Files\*" -Recurse -Force -EA SilentlyContinue -Verbose
            Remove-Item -path "C:\Users\$($_.Name)\AppData\Local\Microsoft\Windows\WER\*" -Recurse -Force -EA SilentlyContinue -Verbose
            Remove-Item -path "C:\Users\$($_.Name)\AppData\Local\Temp\*" -Recurse -Force -EA SilentlyContinue -Verbose
            Remove-Item -path "C:\Windows\Temp\*" -Recurse -Force -EA SilentlyContinue -Verbose
        }
        Write-Host -ForegroundColor Yellow "Done...`n"

        # Clear Opera & Chromium
        Write-Host -ForegroundColor Yellow "Clearing Opera & Chromium Cache`n"
        Import-CSV -Path $env:USERPROFILE\users.csv -Header Name | ForEach-Object {
            Remove-Item -Path "C:\Users\$($_.Name)\AppData\Local\Chromium\User Data\Default\Cache\*" -Recurse -Force -EA SilentlyContinue -Verbose
            Remove-Item -Path "C:\Users\$($_.Name)\AppData\Local\Chromium\User Data\Default\GPUCache\*" -Recurse -Force -EA SilentlyContinue -Verbose
            Remove-Item -Path "C:\Users\$($_.Name)\AppData\Local\Chromium\User Data\Default\Media Cache" -Recurse -Force -EA SilentlyContinue -Verbose
            Remove-Item -Path "C:\Users\$($_.Name)\AppData\Local\Chromium\User Data\Default\Pepper Data" -Recurse -Force -EA SilentlyContinue -Verbose
            Remove-Item -Path "C:\Users\$($_.Name)\AppData\Local\Chromium\User Data\Default\Application Cache" -Recurse -Force -EA SilentlyContinue -Verbose
            Remove-Item -Path "C:\Users\$($_.Name)\AppData\Local\Opera Software\Opera Stable\Cache\*" -Recurse -Force -EA SilentlyContinue -Verbose
        }
        Write-Host -ForegroundColor Yellow "Done...`n"


        Write-Host -ForegroundColor Green "All Tasks Done!`n`n"
    }
    else {
        Write-Host -ForegroundColor Yellow "Session Cancelled`n"
        Exit
    }

    # Empty Recycle Bin
    if ($Cleanbin -eq 'Y') {
        Write-Host -ForegroundColor Green "Cleaning Recycle Bin`n"
        $ErrorActionPreference = 'SilentlyContinue'
        $RecycleBin = "C:\`$Recycle.Bin"
        $BinFolders = Get-ChildItem $RecycleBin -Directory -Force

        Foreach ($Folder in $BinFolders) {
            # Translate the SID to a User Account
            $objSID = New-Object System.Security.Principal.SecurityIdentifier ($folder)
            try {
                $objUser = $objSID.Translate( [System.Security.Principal.NTAccount])
                Write-Host -Foreground Yellow -Background Black "Cleaning $objUser Recycle Bin"
            }
            # If SID cannot be Translated, Throw out the SID instead of error
            catch {
                $objUser = $objSID.Value
                Write-Host -Foreground Yellow -Background Black "$objUser"
            }
            $Files = @()

            if ($PSVersionTable.PSVersion -Like "*2*") {
                $Files = Get-ChildItem $Folder.FullName -Recurse -Force
            }
            else {
                $Files = Get-ChildItem $Folder.FullName -File -Recurse -Force
                $Files += Get-ChildItem $Folder.FullName -Directory -Recurse -Force
            }

            $FileTotal = $Files.Count

            for ($i = 1; $i -le $Files.Count; $i++) {
                $FileName = Select-Object -InputObject $Files[($i - 1)]
                Write-Progress -Activity "Recycle Bin Clean-up" -Status "Attempting to Delete File [$i / $FileTotal]: $FileName" -PercentComplete (($i / $Files.count) * 100) -Id 1
                Remove-Item -Path $Files[($i - 1)].FullName -Recurse -Force
            }
            Write-Progress -Activity "Recycle Bin Clean-up" -Status "Complete" -Completed -Id 1
        }
        Write-Host -ForegroundColor Green "Done`n `n"
    }

    # Get Drive size after clean
    $After = Get-WmiObject Win32_LogicalDisk | Where-Object { $_.DriveType -eq "3" } | Select-Object SystemName,
    @{ Name = "Drive" ; Expression = { ( $_.DeviceID ) } },
    @{ Name = "Size (GB)" ; Expression = {"{0:N1}" -f ( $_.Size / 1gb)}},
    @{ Name = "FreeSpace (GB)" ; Expression = {"{0:N1}" -f ( $_.Freespace / 1gb ) } },
    @{ Name = "PercentFree" ; Expression = {"{0:P1}" -f ( $_.FreeSpace / $_.Size ) } } |
        Format-Table -AutoSize | Out-String

    # Sends some before and after info for ticketing purposes
    Write-Host -ForegroundColor Green "Before: $Before"
    Write-Host -ForegroundColor Green "After: $After"
    Start-Sleep -s 15

    # Completed Successfully!
    # Open Text File
    Invoke-Item c:\windows\temp\$LogDate.log

    # Stop Script
    Stop-Transcript
} Cleanup
