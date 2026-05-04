# Instructions
Uses the Bambu Studio version of the library and sets some headers like Bambu Studio does.

This is a proof of concept. Just slicing and printing has been tested. Viewing the camera didn't work.

## Test features
1. Slicing
2. Printing to Bambu printers, not in LAN/Developer mode, through the cloud
3. Viewing the printer's camera

## Getting BambuStudio libraries and extracting them
1. mkdir ~/BambuStudioLibraries
2. cd ~/BambuStudioLibraries
3. wget https://public-cdn.bblmw.com/upgrade/studio/plugins/02.06.00.50/7459f94f40/linux_02.06.00.50.zip ( Command to get the URL if it changes: curl -i 'https://api.bambulab.com/v1/iot-service/api/slicer/resource?slicer/plugins/cloud=02.06.00.00' -H 'User-Agent: BambuStudio/02.06.00.51' -H 'X-BBL-Client-Type: slicer' -H 'X-BBL-Client-Name: BambuStudio' -H 'X-BBL-Client-Version: 02.06.00.51' -H 'X-BBL-OS-Type: linux' -H 'X-BBL-OS-Version: 6.8.0' -H 'X-BBL-Language: en' )
4. unzip linux_02.06.00.50.zip

### Command to update the plugins zip URL
```
curl -i 'https://api.bambulab.com/v1/iot-service/api/slicer/resource?slicer/plugins/cloud=02.06.00.00' \
  -H 'User-Agent: BambuStudio/02.06.00.51' -H 'X-BBL-Client-Type: slicer' \
  -H 'X-BBL-Client-Name: BambuStudio' -H 'X-BBL-Client-Version: 02.06.00.51' -H 'X-BBL-OS-Type: linux' \
  -H 'X-BBL-OS-Version: 6.8.0' -H 'X-BBL-Language: en'
```

## OrcaSlicer compile and install
1. git clone https://github.com/malleaklea/OrcaSlicer.git 
2. cd OrcaSlicer
3. ./build_flatpak.sh -i -j X  Where is is how many cores you want to use for the compile, and takes a while. Wise to cut the number of cores to half to prevent running out of memory.
4. flatpak install --user OrcaSlicer-Linux-flatpak_V2.4.0-dev_x86_64.flatpak

## OrcaSlicer initial run
1. flatpak run com.orcaslicer.OrcaSlicer
2. Let the normal plugin install
3. Close OrcaSlicer

## Backup libraries
1. mkdir ~/.var/app/com.orcaslicer.OrcaSlicer/config/OrcaSlicer/plugins/backup
2. mv ~/.var/app/com.orcaslicer.OrcaSlicer/config/OrcaSlicer/plugins/*.so ~/.var/app/com.orcaslicer.OrcaSlicer/config/OrcaSlicer/plugins/backup

## Installing the latest BambuStudio libraries
1. cp -a ~/BambuStudioLibraries/* ~/.var/app/com.orcaslicer.OrcaSlicer/config/OrcaSlicer/plugins
2. mv ~/.var/app/com.orcaslicer.OrcaSlicer/config/OrcaSlicer/plugins/libbambu_networking.so ~/.var/app/com.orcaslicer.OrcaSlicer/config/OrcaSlicer/plugins/libbambu_networking_02.06.00.50.so  <-- This filename has to match the version of the library. Normally the filename is libbambu_networking_02.03.00.62.so.

## Making sure stealth_mode is false
1. grep -i stealth ~/.var/app/com.orcaslicer.OrcaSlicer/config/OrcaSlicer/OrcaSlicer.conf
2. If `true`, edit ~/.var/app/com.orcaslicer.OrcaSlicer/config/OrcaSlicer/OrcaSlicer.conf and make it `false`

## Using OrcaSlicer
1. Open OrcaSlicer, if any error/dialog about the plugin, double check the instructions above
2. Login to your Bambu account in OrcaSlicer
3. Go to the Device tab, confirm your printer appears, is connected, and functioning properly
4. Slice and print a model. It will go through the cloud.


