# wirelesspasswordviewer
This simple application show you wireless passwords which is you connected before in windows 
Just Run this app and select the SSID that you want to view the password! 
This app use the Netsh method to fetch the windows wirless profiles inforamtion and select key content to show the password of profiles.
It tested in Windows 10 ( all version) Windows 8, Windows server 2012, 2016 and 2019.
 Source Code:
 
 ------------------------------------------------
$formWIPASS_Load={
	#TODO: Initialize Form Controls here
	#Get wireless profile list and trim to name of SSID
	$listProfiles = netsh wlan show profiles | Select-String -Pattern "All User Profile" | %{ ($_ -split ":")[-1].Trim() };
	#Update combo list with SSID names with $listprofiles 
	Update-ComboBox $combobox_Profile_SSID ($listProfiles) 
	
}



$button_Password_Click={
	#TODO: Place custom script here
	#Get selected text from Combobox with SSID names 
	$SSID = $combobox_Profile_SSID.Text
	#Use netsh command for get key content in clear text 
	$Password = netsh.exe wlan show profile name=$SSID key=clear | Select-String -Pattern "Key Content"
	#Trim content befor show in text box 
	$PW = ($Password -split ":")[-1].Trim() -replace '"'
	$Show_Password_Textbox.Text = $PW
}

------------------------------------------------------------
