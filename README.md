[![Build Status](https://travis-ci.org/kodi-pvr/pvr.vuplus.svg?branch=master)](https://travis-ci.org/kodi-pvr/pvr.vuplus)
[![Coverity Scan Build Status](https://scan.coverity.com/projects/5120/badge.svg)](https://scan.coverity.com/projects/5120)

Enigma2 is a open source TV-receiver/DVR platform which Linux-based firmware (OS images) can be loaded onto many Linux-based set-top boxes (satellite, terrestrial, cable or a combination of these) from different manufacturers.

This addon leverages the OpenWebIf project to interact with the Enigma2 device via Restful APIs: (https://github.com/E2OpenPlugins/e2openplugin-OpenWebif)

**Note:** Some images do not use OpenWebIf as the default web interface. In these images some standard functionality may still work but is not guaranteed. Some features that would not function include:
* Autotimers
* Drive Space Reporting
* Embedded EPG Genre IDs
* Full Tuner Signal Support (Including Service Providers) 

# Enigma2 PVR
VuPlus PVR client addon for [Kodi](https://kodi.tv)

## Build instructions

### Linux

1. `git clone https://github.com/xbmc/xbmc.git`
2. `git clone https://github.com/kodi-pvr/pvr.vuplus.git`
3. `cd pvr.vuplus && mkdir build && cd build`
4. `cmake -DADDONS_TO_BUILD=pvr.vuplus -DADDON_SRC_PREFIX=../.. -DCMAKE_BUILD_TYPE=Debug -DCMAKE_INSTALL_PREFIX=../../xbmc/addons -DPACKAGE_ZIP=1 ../../xbmc/cmake/addons`
5. `make`

## Support 

### Useful links

* [Kodi's PVR user support forum](https://forum.kodi.tv/forumdisplay.php?fid=167)
* [Report an issue on Github](https://github.com/kodi-pvr/pvr.vuplus/issues)
* [Kodi's PVR development support forum](https://forum.kodi.tv/forumdisplay.php?fid=136)

### Logging

When reporting issues a debug log should always be supplied. You can use the following guide: [Easy way to submit Kodi debug logs](https://kodi.wiki/view/Log_file/Easy)

For more detailed info on logging please see the appendix [here](#logging-detailed)

## Configuring the addon

### Connection
Within this tab the connection options need to be configured before it can be successfully enabled.

* **Enigma2 hostname or IP address**: The IP address or hostname of your enigma2 based set-top box.
* **Web interface port**: The port used to connect to the web interface
* **Use secure HTTP (https)**: Use https to connect to the web interface
* **Username**: If the webinterface of the set-top box is protected with a username / password combination this needs to be set in this option.
* **Password**: If the webinterface of the set-top box is protected with a username / password combination this needs to be set in this option.
* **Enable automatic configuration for live streams**: When enabled the stream URL will be read from an M3U file. When disabled it is constructed based on the filename.
* **Streaming port**: This option defines the streaming port the set-top box uses to stream live tv. The default is 8001 which should be fine if the user did not define a custom port within the webinterface.
Webinterface Port: This option defines the port that should be used to access the webinterface of the set-top box.
* **Use secure HTTP (https) for streams**: Use https to connect to streams
* **Use login for streams**: Use the login username and password for streams

### General
Within this tab general options are configured.

* **Fetch picons from web interface**: Fetch the picons straight from the Enigma 2 set-top box.
* **Use picons.eu file format**: Assume all picons files fetched from the set-top box start with `1_1_1_` and end with `_0_0_0`
* **Use OpenWebIf picon path**: Fetch the picon path from OpenWebIf instead of constructing from ServiceRef. Requires OpenWebIf 1.3.x or higher.
* **Icon path**: In order to have Kodi display channel logos you have to copy the picons from your set-top box onto your OpenELEC machine. You then need to specify this path in this property.
* **Update interval**: As the set-top box can also be used to modify timers, delete recordings etc. and the set-top box does not notify the Kodi installation, the addon needs to regularly check for updates (new channels, new/changed/deletes timers, deleted recordings, etc.) This property defines how often the addon checks for updates.
* **Update mode**: The mode used when the update interval is reached. Note that if there is any timer change detected a recordings update will always occur regardless of the update mode. Choose from one of the following two modes:
    - `Timers and Recordings` - Update all timers and recordings.
    - `Timers only` - Only update the timers.

### Channels
Within this tab options that refer to channel data can be set. When changing bouquets you may need to clear the channel cache to the settings to take effect. You can do this by going to the following in Kodi settings: `Settings->PVR & Live TV->General->Clear cache`.

* **Zap before channelswitch (i.e. for Single Tuner boxes)**: When using the addon with a single tuner box it may be necessary that the addon needs to be able to zap to another channel on the set-top box. If this option is enabled each channel switch in Kodi will also result in a channel switch on the set-top box. Please note that "allow channel switching" needs to be enabled in the webinterface on the set-top box.
* **TV bouquet fetch mode**: Choose from one of the following three modes:
    - `All bouquets` - Fetch all TV bouquets from the set-top box.
    - `Only one bouquet` - Only fetch the bouquet specified in the next option
    - `Favourites group` - Only fetch the system bouquet for TV favourites.
* **TV bouquet**: If the previous option has been has been set to `Only one bouquet` you need to specify the TV bouquet to be fetched from the set-top box. Please not that this is the bouquet-name as shown on the set-top box (i.e. "Favourites (TV)"). This setting is case-sensitive.
* **Fetch TV favourites bouquet**: If the fetch mode is `All bouquets` or `Only one bouquet` depending on your Enigma2 image you may need to explicitly fetch favourites if you require them. The options are:
    - `Disabled` - Don't explicitly fetch TV favourites.
    - `As first bouquet` - Explicitly fetch them as the first bouquet.
    - `As last bouquet` - Explicitly fetch them as the last bouquet.
* **Radio bouquet fetch mode**: Choose from one of the following three modes:
    - `All bouquets` - Fetch all Radio bouquets from the set-top box.
    - `Only one bouquet` - Only fetch the bouquet specified in the next option
    - `Favourites group` - Only fetch the system bouquet for Radio favourites.
* **Radio bouquet**: If the previous option has been has been set to `Only one bouquet` you need to specify the Radio bouquet to be fetched from the set-top box. Please not that this is the bouquet-name as shown on the set-top box (i.e. "Favourites (Radio)"). This setting is case-sensitive.
* **Fetch Radio favourites bouquet**: If the fetch mode is `All bouquets` or `Only one bouquet` depending on your Enigma2 image you may need to explicitly fetch favourites if you require them. The options are:
    - `Disabled` - Don't explicitly fetch Radio favourites.
    - `As first bouquet` - Explicitly fetch them as the first bouquet.
    - `As last bouquet` - Explicitly fetch them as the last bouquet.

### EPG
Within this tab options that refer to EPG data can be set. Excluding logging missing genre text mappings all other options will require clearing the EPG cache to take effect. This can be done by going to `Settings->PVR & Live TV->Guide->Clear cache` in Kodi after the addon restarts.

Information on customising the extraction and mapper configs can be found in the next section of the README.

* **Extract season, episode and year info where possible**: Check the description fields in the EPG data and attempt to extract season, episode and year info where possible. 
* **Extract show info file**: The config used to extract season, episode and year information. The default file is `English-ShowInfo.xml`. 
* **Enable genre ID mappings**: If the genre IDs sent with EPG data from your set-top box are not using the DVB standard, map from these to the DVB standard IDs. Sky UK for instance uses OpenTV, in that case without this option set the genre colouring and text would be incorrect in Kodi. 
* **Genre ID mappings file**: The config used to map set-top box EPG genre IDs to DVB standard IDs. The default file is `Sky-UK.xml`. 
* **Enable Rytec genre text mappings**: If you use Rytec XMLTV EPG data this option can be used to map the text genres to DVB standard IDs. 
* **Rytec genre text mappings file**: The config used to map Rytec Genre Text to DVB IDs. The default file is `Rytec-UK-Ireland.xml`. 
* **Log missing genre text mappings**: If you would like missing genre mappings to be logged so you can report them enable this option. Note: any genres found that don't have a mapping will still be extracted and sent to Kodi as strings. Currently genres are extracted by looking for text between square brackets, e.g. [TV Drama], or for major, minor genres using a dot (.) to separate [TV Drama. Soap Opera]
* **EPG update delay per channel**: For older Enigma2 devices EPG updates can effect streaming quality (such as buffer timeouts). A delay of between 250ms and 5000ms can be introduced to improve quality. Only recommended for older devices. Choose the lowest value that avoids buffer timeouts.

### Recordings & Timers

* **Recording folder on receiver**: Per default the addon does not specify the recording folder in newly created timers, so the default set in the set-top box will be used. If you want to specify a different folder (i.e. because you want all recordings scheduled via Kodi to be stored in a separate folder), then you need to set this option.
* **Use only the DVB boxes' current recording path**: If this option is not set the addon will fetch all available recordings from all configured paths from the set-top box. If this option is set then it will only list recordings that are stored within the "current recording path" on the set-top box.
* **Keep folder structure for records**: If enabled do not specify a recording folder, when disabled (defaut), check if the recording is in it's own folder or in the root of the recording path
* **Enable generate repeat timers**: Repeat timers will display as timer rules. Enabling this will make Kodi generate regular timers to match the repeat timer rules so the UI can show what's scheduled and currently recording for each repeat timer.
* **Number of repeat timers to generate**: The number of Kodi PVR timers to generate.
* **Enable autotimers**: When this is enabled there are some settings required on the set-top box to enable linking of AutoTimers (Timer Rules) to Timers in the Kodi UI. The addon attempts to set these automatically on boot. To set manually on the set-top box enable the following options (note that this feature supports OpenWebIf 1.3.x and higher):
  1. Hit `Menu` on the remote and go to `Timers->AutoTimers`
  2. Hit `Menu` again and then select `6 Setup`
  3. Set the following to option to `yes`
    * `Include "AutoTimer" in tags`
    * `Include AutoTimer name in tags`
* **Automatic timerlist cleanup**: If this option is set then the addon will send the command to delete completed timers from the set-top box after each update interval.

### Timeshift
Timeshifting allows you to pause live TV as well as move back and forward from your current position similar to playing back a recording. The buffer is deleted each time a channel is changed or stopped.

* **Enable timeshift**: What timeshift option do you want:
    - `Disabled` - No timeshifting
    - `On Pause` - Timeshifting starts when a live stream is paused. E.g. you want to continue from where you were at after pausing.
    - `On Playback` - Timeshifting starts when a live stream is opened. E.g. You can go to any point in the stream since it was opened.
* **Timeshift buffer path**: The path used to store the timeshift buffer. The default is the `addon_data/pvr.vuplus` folder in userdata. 

### Advanced
Within this tab more uncommon and advanced options can be configured.

* **Put outline (e.g. sub-title) before plot**: By default plot outline (short description on Enigma2) is not displayed in the UI. Can be
 displayed in EPG, Recordings or both. After changing this option you will need to clear the EPG cache `Settings->PVR & Live TV->Guide->Clear cache` for it to take effect.
* **Send powerstate mode on addon exit**: If this option is set to a value other than `DISABLED` then the addon will send a Powerstate command to the set-top box when Kodi will be closed (or the addon will be deactivated).
    - `Disabled` - No command sent when the addon exits
    - `Standby` - Send the standby command on exit
    - `Deep standby` - Send the deep standby command on exit. Note, the set-top box will not respond to Kodi after this command is sent.
    - `Wakeup, then standby` - Similar to standby but first sends a wakeup command. Can be useful if you want to ensure all streams have stopped. Note: if you use CEC this could cause your TV to wake.
* **Custom live TV timeout (0 to use default)**: The timemout to use when trying to read live streams
* **Stream read chunk size**: The chunk size used by Kodi for streams. Default 0 to leave it to Kodi to decide.
* **Enable trace logging in debug mode**: Very detailed and verbose log statements will display in addition to standard debug statements

## Customising Config Files

The various config files allow users to create their own, making it possible to support other languages and formats. Each different type of config file is detailed below. Best way to learn about them is to read the config files themselves. Each contains details of how the config file works.

All of the files listed below are overwritten each time the addon starts. Therefore if you are customising files please create new copies with different file names. Note: that only the files below are overwritten any new files you create will not be touched.

After adding and selecting new config files you will need to clear the EPG cache `Settings->PVR & Live TV->Guide->Clear cache` for it to take effect.

If you would like to support other formats/languages please raise an issue at the github project https://github.com/kodi-pvr/pvr.vuplus, where you can either create a PR or request your new configs be shipped with the addon.

There is one config file located here: `userdata/addon_data/pvr.vuplus/genres/kodiDvbGenres.xml`. This simply contains the DVB genre IDs that Kodi supports. Can be a useful reference if creating your own configs. This file is also overwritten each time the addon restarts.

### Season, Episode and Year Show Info

Config files are located in the `userdata/addon_data/pvr.vuplus/showInfo` folder.

The following files are currently available with the addon:
    - `English-ShowInfo.xml`

Note: the config file can contain as many pattern matches as are required. So if you need to support multiple languages in a single file that is possible. However there must be at least one <seasonEpisode> pattern and at least one <year> pattern. Proficiency in regular exressions is required!

### Genre ID Mappings

Config files are located in the `userdata/addon_data/pvr.vuplus/genres/genreIdMappings` folder.

The following files are currently available with the addon:
    - `AU-SAT.xml`
    - `Sky-IT.xml`
    - `Sky-NZ.xml`
    - `Sky-UK.xml`

Note: that each source genre ID can be mapped to a DVB ID. However multiple source IDs can be mapped to the same DVB ID. Therefore there are exactly 256 <mapping> elements in each file as a genre ID is 8 bits. All values are in Hex. The first fours bits are the genreType in Kodi PVR and the last four bits are the genreSubType.

### Rytec Genre Text Mappings

Config files are located in the `userdata/addon_data/pvr.vuplus/genres/genreRytecTextMappings` folder.

The following files are currently available with the addon:
    - `Rytec-UK-Ireland.xml`

Note: the config file can contain as many mappings as is required. Currently genres are extracted by looking for text between square brackets, e.g. [TV Drama], or for major, minor genres using a dot (.) to separate [TV Drama. Soap Opera]. The config file maps the text to a kodi DVB genre ID. If the full text cannot be matched it attempts to match just the major genre, i.e. "TV Drama" in the previous example. If a mapping cannot be found the text between the brackets will be used instead. However there will be no colouring in the Kodi EPG in this case.

## Appendix

### Logging detailed

Some of the most useful information in your log are the details output at the start of the log, both for Kodi and the addon.

The first 5 lines of the log give details on the exact kodi flavour and version you are running:

```
01:10:45.744 T:140736264741760  NOTICE: -----------------------------------------------------------------------
01:10:45.744 T:140736264741760  NOTICE: Starting Kodi (18.0-BETA2 Git:20180903-5d7e1dbd9c-dirty). Platform: OS X x86 64-bit
01:10:45.744 T:140736264741760  NOTICE: Using Debug Kodi x64 build
01:10:45.744 T:140736264741760  NOTICE: Kodi compiled Sep  5 2018 by Clang 9.1.0 (clang-902.0.39.2) for OS X x86 64-bit version 10.9.0 (1090)
01:10:45.745 T:140736264741760  NOTICE: Running on Apple Inc. MacBook10,1 with OS X 10.13.6, kernel: Darwin x86 64-bit version 17.7.0
```

Secondly all the information of the Enigma2 image, web interface version etc. is also output:

```
12:36:55.888 T:123145422725120  NOTICE: AddOnLog: Enigma2 Client: pvr.vuplus - Open - VU+ Addon Configuration options
12:36:55.888 T:123145422725120  NOTICE: AddOnLog: Enigma2 Client: pvr.vuplus - Open - Hostname: '192.168.1.201'
12:36:55.888 T:123145422725120  NOTICE: AddOnLog: Enigma2 Client: pvr.vuplus - Open - WebPort: '80'
12:36:55.888 T:123145422725120  NOTICE: AddOnLog: Enigma2 Client: pvr.vuplus - Open - StreamPort: '8001'
12:36:55.888 T:123145422725120  NOTICE: AddOnLog: Enigma2 Client: pvr.vuplus - Open Use HTTPS: 'false'
12:36:56.308 T:123145422725120  NOTICE: AddOnLog: Enigma2 Client: pvr.vuplus - LoadDeviceInfo - DeviceInfo
12:36:56.308 T:123145422725120  NOTICE: AddOnLog: Enigma2 Client: pvr.vuplus - LoadDeviceInfo - E2EnigmaVersion: 2019-01-03
12:36:56.308 T:123145422725120  NOTICE: AddOnLog: Enigma2 Client: pvr.vuplus - LoadDeviceInfo - E2ImageVersion: 5.2.022
12:36:56.309 T:123145422725120  NOTICE: AddOnLog: Enigma2 Client: pvr.vuplus - LoadDeviceInfo - E2DistroVersion: openvix
12:36:56.309 T:123145422725120  NOTICE: AddOnLog: Enigma2 Client: pvr.vuplus - LoadDeviceInfo - E2WebIfVersion: OWIF 1.3.5
12:36:56.309 T:123145422725120  NOTICE: AddOnLog: Enigma2 Client: pvr.vuplus - LoadDeviceInfo - E2DeviceName: Ultimo4K
```

#### Description of Log levels

* **Error**: Something that will require intervention to resolve
* **Notice**: Could be an important piece of information or a warning that something has occurred that might be undesirable but has not affected normal operation
* **Info**: Some information on normal operation
* **Debug**: More detailed information that can aid in diagnosing issues.
* **Trace**: Extremely verbose logging, should rarely be required. Note: can only be enabled from the addon settings in the ```Advanced``` section.

#### Cleaning up the log

(If you have a fresh install of version 3.15.0 or later you can ignore this)

As the addon was upgraded over time some old settings are no longer required. These old settings can create a lot of extra logging on startup. To date there are four of these and they will present like this in your log:

```
23:17:22.696 T:3990016880   DEBUG: CSettingsManager: requested setting (extracteventinfo) was not found.
23:17:22.696 T:3990016880   DEBUG: CAddonSettings[pvr.vuplus]: failed to find definition for setting extracteventinfo. Creating a setting on-the-fly...
23:17:22.696 T:3990016880   DEBUG: CSettingsManager: requested setting (onegroup) was not found.
23:17:22.696 T:3990016880   DEBUG: CAddonSettings[pvr.vuplus]: failed to find definition for setting onegroup. Creating a setting on-the-fly...
23:17:22.696 T:3990016880   DEBUG: CSettingsManager: requested setting (onlyonegroup) was not found.
23:17:22.696 T:3990016880   DEBUG: CAddonSettings[pvr.vuplus]: failed to find definition for setting onlyonegroup. Creating a setting on-the-fly...
23:17:22.697 T:3990016880   DEBUG: CSettingsManager: requested setting (setpowerstate) was not found.
23:17:22.697 T:3990016880   DEBUG: CAddonSettings[pvr.vuplus]: failed to find definition for setting setpowerstate. Creating a setting on-the-fly...
```

If you see these in debug mode and would like to clean them up you can either: 

1. Remove the offending lines from you settings.xml file OR
2. Delete you settings.xml and restart kodi. Note that if you use this option you will need to reconfigure the addon from scratch.

Your settings.xml file can be found here: ```userdata/addon_data/pvr.vuplus/settings.xml```
