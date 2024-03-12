# Get-WlanProfilesWithPasswords.ps1

function Get-WlanProfilesWithPasswords {
    $wlanProfiles = Invoke-Expression 'netsh wlan show profiles' | Select-String -Pattern "All User Profile     : (.*)$" | %{$_.Matches.Groups[1].Value.Trim()}
    foreach ($wlanProfile in $wlanProfiles) {
        $password = (Invoke-Expression "netsh wlan show profile name=`"$wlanProfile`" key=clear" | Select-String -Pattern "Key Content            : (.*)$").Matches.Groups[1].Value.Trim()
        Write-Output "Profile Name: $wlanProfile, Password: $password"
    }
}

# Call the function
Get-WlanProfilesWithPasswords
