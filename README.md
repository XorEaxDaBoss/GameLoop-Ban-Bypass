#GameLoop This Device has Been Placed on Violations List Bypass.
#UC / BINM7MD 2/14/2022
#Modify Device XML Manually im lazy to script it :slight_smile:
#Tutorial
#Delete Contents of which contains the snapshot of emulator C:\ProgramData\Tencent
#Ahead to AppData\Roaming\Tencent And Delete it
#Ahead to AppData\Local\Tencent And Delete it
#Delete Contents C:\Windows\SysWOW64\config\systemprofile\AppData\Roaming\Tencent\
#Registry Modficaiton (HKEY_CURRENT_USER\SOFTWARE\Tencent\MobileGamePC):
#VMDeviceManufacturer -> Or Manully Already Modfied in Script -> Any Manufacturer
#VMDeviceModel -> Or Manully Already Modfied in Script -> Any Model
#ReverseOrder -> HKEY_CURRENT_USER\SOFTWARE\Tencent\MobileGamePC\GameHistory -> Delete
#com.tencent.ig_ContentScale -> Delete
#com.tencent.ig_FPSLevel -> Delete
#com.tencent.ig_RenderQuality -> Delete
echo '[*] Current Android ID :'
content query --uri content://settings/secure --projection value --where "name='android_id'" # Get android_id For output
RandomString=$(eval xxd -l 32 -c 32 -p < /dev/random) # Didnt find uuid in loop :frowning:
echo '[*] Generated Android ID :'
echo ${RandomString:0:16} #Random 16 to bind with android_id
echo '[*] Deleting Android ID : '
content delete --uri content://settings/secure --where "name='android_id'" # Deleting android_id
echo '[*] Updating Android ID : '
 
content insert --uri content://settings/secure --bind name:s:android_id --bind value:s:${RandomString:0:16} # Inserting new generated string as id
echo '[*] Update Output Android ID :'
content query --uri content://settings/secure --projection value --where "name='android_id'" # Get android_id For output
echo '[*] Modifying prop'
#It's better to modify from registry prop will reset in next reboot / start
setprop ro.product.device ZTE # prop modify
setprop ro.product.brand ZTE # prop modify
setprop ro.product.model ZTE # prop modify
setprop ro.product.device ZTE # prop modify
echo '[*] Modify Done'
echo '[*] Deleting Json Files'
find /storage/emulated/0/Android/data/com.tencent.ig/* -name '*.json' -delete #  MailPhoneLogin.json / loginInfoFile.json / And other json files
rm -rf /storage/emulated/0/Android/data/com.tencent.ig/cache/* # clearing cache content
rm -rf /data/data/com.tencent.ig/cache/* # clearing /data/data/com.tencent.ig/cache content
rm -rf /data/data/com.tencent.ig/databases/*  # clearing databases content
rm -rf /data/data/com.tencent.ig/code_cache/* # clearing code_cache content
rm -rf /data/data/com.tencent.ig/app_bugly/* # clearing app_bugly content
echo '[*] GBye'
