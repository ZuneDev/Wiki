# Radio
The Zune software contains unused functionality for managing and listening to internet radio stations.

# Demo Video
[View on Catbox.](https://files.catbox.moe/fdiruk.mp4)

# Accessing

There are two methods of gaining access to the Radio view.

## Changing the Startup View
The easiest way of accessing the Radio view is by changing the registry key used by the Startup View setting.

If using the Registry Editor, navigate to `HKCU/Software/Microsoft/Zune/Shell`.
Find the StartupView string value, and set it to `Collection/Radio`.

When you launch the Zune software, the Radio screen of the Collection will be displayed.

## Enabling the Radio tab
<img width="827" height="125" alt="1254241490 or 109376" src="https://github.com/user-attachments/assets/e21a53d4-cf0a-4291-b034-c62d7712efa8" />

As discovered by Rafael Rivera, there is a hidden Radio tab in the Collection view that is normally permanently disabled.

Enabling this tab requires dissassembling and modifying `ZuneNativeLib.dll`.

Rafael's process on enabling the Radio tab can be found [here.](https://web.archive.org/web/20110718070449/http://www.withinwindows.com/2009/09/28/tinkering-with-zune-4-0-enabling-the-unfinished-radio/) [(alt)](https://www.betaarchive.com/forum/viewtopic.php?t=9329)

# Stations
Stations are stored as registry keys in `HKCU/Software/Microsoft/Zune/Radio`.

Each station in your collection is stored as a sub-key within the Radio key.
Within that key are two string values, SourceURL and Image.

When the Zune software is installed, it automatically adds three stations to your collection;
107.7 The End, KEXP, and National Public Radio.

Below is their values in the registry. This can be imported like a .reg file in order to restore these stations as well.
```
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Microsoft\Zune\Radio]

[HKEY_CURRENT_USER\Software\Microsoft\Zune\Radio\107.7 The End]
"SourceURL"="http://38.116.147.101:80/KNDD?MSWMExt=.asf"
"Image"="res://ZuneShellResources!Radio.TheEnd.png"
@=""

[HKEY_CURRENT_USER\Software\Microsoft\Zune\Radio\KEXP]
"SourceURL"="http://kexp.org/audio/kexp-uncomp.asx"
"Image"="res://ZuneShellResources!Radio.KEXP.jpg"
@=""

[HKEY_CURRENT_USER\Software\Microsoft\Zune\Radio\National Public Radio]
"SourceURL"="http://128.208.34.65/multi"
"Image"="res://ZuneShellResources!Radio.NPR.jpg"
@=""
```

These stations are only added when the Zune software is installed, they do not restore themselves even if the Zune software is launched for the first time by another user account.

All three stations reference thumbnail images that would have been stored in the Zune software itself.
These images are not present, so they don't load and instead display the default cover art.

# Functionality
In it's current state, Internet radio is non-functional, as the Zune software cannot play internet radio streams.
Trying to play a station will act as if the Zune software failed to play a file.

<img width="1086" height="713" alt="image" src="https://github.com/user-attachments/assets/04b73096-19fb-40e5-9bf5-ca75d90c9a79" />


The view for managing radio stations is simplistic, and missing a lot of features in other Collection views.

Adding a station is done by inputing the URL into a basic text field, and clicking the Add Station button.
This is in contrast to the Podcasts collection view for example, where adding a podcast by URL is done in a pop-up dialog.
Furthermore, there are no checks to make sure that an inputted URL is valid; clicking Add Station will immediately add a station with the contents of the URL box as the source URL, even if it isn't valid.


<img width="274" height="242" alt="image" src="https://github.com/user-attachments/assets/0eb6fda2-5b2f-407b-8d51-4fc3b3158bfd" />

There is a context menu available when multiple stations are selected, however the only option is to delete the selected stations. Selecting Delete will immeditately delete the stations without any warning, unlike in other Collection views.

<img width="1086" height="713" alt="image" src="https://github.com/user-attachments/assets/45c66da9-c865-4f6a-8043-7f4474cfbb21" />

When there are no stations in the Collection, there are no extra links for searching the Zune Marketplace or adding a station by URL. Since there is no way to add a station in this state, it must be added manually to the registry.

# Bugs

## Now Playing
<img width="1086" height="713" alt="image" src="https://github.com/user-attachments/assets/8a05f3b9-36a3-4368-98d6-531b143a735e" />

When switching to the Now Playing view and back to the Collection, all stations will disappear from the view.
Attempting to select all stations by clicking the "* STATIONS" text while in this state will crash the Zune software.

## Unimplemented Sorting feature
In other Collection views, you can click the "by *" text to change how the content is sorted.
This text appears in the Radio view, but clicking it here does nothing.

## Other Crashes
* Right-clicking a single station will crash the Zune software.
* Playing a station with an invalid SourceURL will crash the Zune software.
