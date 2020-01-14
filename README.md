# **MPCW** (01-2020)

A wrapper+ for Music Player Daemon's client mpc.



# Description

**Take control** of **MPD** with **MPCW** and **enjoy** your **music** collection like you never did before!  
Enable **music non-stop** and get ready to **listen** to your own... **personal**... **radio station**!  
As you rate the songs, playlists are automatically generated according to your music tastes.



# Features

- **Full control** of **MPD** inside the **terminal** or via keyboard shortcuts (not included), thanks to numerous **aliases** and **useful commands**.
- **Music non-stop**: listen to **random songs**, **albums** or **similar artists** based **playlists**... **non-stop**.
- **Playback statistics**: **rate songs** to make your **listening experience** even more **enjoyable**.
- **Notification** on song change.
- **Voice over**: occasionally say current song's title or the actual time.
- **Title display** with scrolling effect in *polybar* or similar.

*Dependencies: mpd, mpc, dunst, wget, jq, sqlite3*  
*Optional dependencies: espeak and mpv (for voice over)*

*Note: **MPCW** assumes songs are stored in directories such as "**artist/album**".*



## 1. Installation

First, clone this repository 

`git clone https://github/teegre/MPCW.git`

then install **MPCW**:

`./install.sh`



## 2. Configuration

**MPCW** consists in three *bash scripts*:

- **mpcw**, which contains **commands** and **aliases**,
- **mpcwd**, the **daemon** that handles of **notifications**, **playlists** and **statistics**.
- **mpcwt**, meant to be used as a module in *polybar* (or similar).

**mpcw** script has to be sourced in your .bashrc file.  
It also works fine in zsh.

`source $HOME/.local/bin/mpcw`

To start or stop **MPCW** daemon:

`mpcwd`

To start it automatically at startup, add it to your windows manager/desktop environment config file or .xinitrc.



### 2.1 Files

**MPCW** stores files in *$HOME/.config/mpcw*.

|File |Description
|:----|:----------
|settings |Settings file.
|log      |Log file.
|hist     |Played songs history.
|hist.backup|History backup file.
|mpcw.pid | Daemon process id.
|mpcw.wpid| Daemon process id.

*currentmedia* file is stored in *$HOME/.config* and is updated by *mpcwd*.  
It is used by *mpcwt* to display current song.  
*(**Snotify** use it too)*



## 3. Usage

Commands can be invoked directly from the command line.  
Also, it is possible to call **MPCW** functions (ie. as keyboard shortcuts to be used in a tiling window manager), by entering:

`mpcw COMMAND [OPTIONS]`



### 3.1 Playback control

|Command           |Alias            |Description
|:-----------------|:----------------|:----------
|play [TRACK]      |**pl**           |**Start playback**. If a track number is provided, play this track.
|**pause**         |-                |**Pause/resume** playback.
|toggle            |**p**            |**Same as pause**.
|stop              |**st**           |**Stop playback**.
|stop_after_current|**sta**          |**Stop playback after current song**.
|next              |**nx**           |Go to **next song**.
|prev              |**pv**           |Go to **previous song**.|                                    |
|skip              |**sk**           |**Skip current song**.
|-                 |**seek** POSITION|**Seek through** current song (use percentage, seconds, or hh:mm:ss).
|play_album [ARTIST] [ALBUM]         |**pla**    |**Play current song's album**.<br>If an artist and an album name are provided, album plays immediately.<br>Go back to **music non-stop** song mode when album is over.
|add_album         |**aa**           |Add current song's album.<br>Go back to **music non-stop** song mode when album is over.
|ins_album ARTIST ALBUM              |**insa**|**Add an album after current song**.<br>Go back to **music non-stop** mode when album is over.
|next_album        |**nxa**          |When in album mode, **play another random album**.



#### 3.1.1 About play_album, add_album and ins_album commands

Let's say you are **listening** to **random songs**. Then you **hear** one that **you really like** and you think you would like to **listen to the full album**.  
So type: **pla** and the **album** starts to **play**. If the song you are listening to is the first track of the album, playback continues seamlessly.  
Otherwise, playback stops and the first song of the album plays.  
But what if you want to **listen** to the **entire song** and **play** the **album afterwards**? Then type: **aa**, and you're done.  
If you want to **play any album without deactivating music non-stop**, clear the playlist and add the songs to the playlist; use  
**insa ARTIST ALBUM**. The album will start to play after the current song.

No need to write the full artist and album names. For instance:

```insa werk bahn```

Will add Kraftwerk, Autobahn...


#### 3.1.3 Player status

The **status** command displays information about player state and current track:

     [song] ---c- ****- x1 [flac]  
    Kraftwerk: Autobahn [46%]  
    Autobahn | 1974

The first line shows player state, music non-stop mode, play mode(s), song rating, song playcount and file format.  
The second shows artist, song title and progress. Then on the last is the album and year.

Play modes:

- r: repeat
- z: random
- s: single
- c: consume
- x: crossfade

This information are also visible in notifications.



### 3.2 Playlist control

|Command   |Alias                       |Description
|:---------|:---------------------------|:----------
|**pls**   |-                           |**Display current playlist**.
|**hist**  |-                           |**Display playback history** (latest first).
|-         |**add** URI                 |**Add song(s)** to the playlist.
|-         |**new** URI                 |Same as **add**, but **clears playlist before** adding songs.
|-         |**ins** URI                 |**Insert song(s)** after current song.
|-         |**move** FROM_POS TO_POS    |**Move track.**
|-         |**del** [FROM_]POS [-TO_POS]|Delete song(s) from the playlist.
|**cr**    |-                           |Crop playlist. Delete all songs except current one.
|**clr**   |-                           |Clear playlist. If song, album or SIMA mode is enabled, new tracks are added to the playlist.
|**getrnd** COUNT SONG|ALBUM|rnd        |Return [count] song(s) or album(s).
|see_album |**ca**                      |Display album for the currently playing song.
|see_artist|**cl**                      |Display albums for the currently playing artist.
|status    |**si**                      |Display current song info.



### 3.3 Volume control

There are two commands for controlling volume:

- **vol** [+|-]VALUE: set volume. If no argument is given, display actual volume.
- **dim**: decrease volume by 50% or set volume back to its previous value.<br>
*Volume can be dimmed only when a song is playing. It is disabled after playback is paused or stopped*.



### 3.4 Music Non-Stop

Music non-stop enables non-stop playback of songs or albums.  
When song mode is on, playlists are generated according to your music tastes, if songs have been rated (cf. 3.5).  
By adding a filter, it's also possible to play songs/album by artist, genre, date, etc.  

*Note: in song mode, consume, random, and crossfade are enabled.*  




#### 3.4.1 Music Non-Stop command

`musicnonstop ALBUM|SONG|SIMA [TAG::VALUE::...::TAG::VALUE]`

|Command            |Alias   |Description
|:------------------|:-------|:----------
|musicnonstop       |**mns** | Display **current music non-stop status**.
|musicnonstop album |**mnsa**| Enable **album mode**.
|musicnonstop song  |**mnss**| Enable **song mode**.
|musicnonstop sima  |**mnsi**| Enable **SIMilar Artist mode** (cf. 3.4.2).
|musicnonstop off   |**mnso**| **Disable** music non-stop

*Example 1: play random pop songs from the '80s*

`mnss genre::pop::date::198`

*Example 2: play random flac files*

`mnss filename::.flac`

*Note: if the playlist is cleared or cropped while playing, new songs will be added according to the current active mode.*  
*If you want to stop playback after the current song, you can use **sta** command.*



#### 3.4.2 Music Non-Stop SIMA mode

SIMA mode automatically creates artist based playlists.  
You can choose what artist to start with by adding one song to an empty playlist.  
If playlist is empty, **MPCW** picks a random song and adds similar artists.  
When the last song is played, a random song is added and similar artists for this song are added as well.  

*Note: an internet connection is required to use this mode.*



#### 3.4.3 Voice over / Radio station name.

To make *song mode* more lively and sound like an actual radio, a voice over feature (using espeak and mpv)   
randomly occurs when a song starts playing (song and SIMA modes only).

Basically, it says: "You are listening to ARTIST: TITLE, on [insert your radio name]."

So to name your radio station:

```set_radio_name NAME```

The default name is *music non-stop radio*.



### 3.5 Rating songs

- **rating**: show current song's rating.
- **love**, **like**, **tsok**, **soso**, **nope**: rate the song 5, 4, 3, 2 or 1 respectively.
- **unrate**: remove rating.

*Note: rating is compatible with **MPDroid**.*


### 3.6 Statistics

- **playcount**: display song's play count.
- **lastplayed**: display the last date and time the song was played.
- **skipcount**: display song's skip count.
- **reset_stats** URI: reset statistics for a given file.



#### 3.6.1 About skipping and history.

It's possible to set a skip limit, so the skipped songs won't be added to the playlist in song or SIMA mode.  
By default the limit is set to 2. In other words, if a song is skipped twice it will never show again.

To change it:

|Command             | Alias
|:-------------------|:-----
|set_skip_limit COUNT| **skl**


Also, songs that are listed in the history won't be added to the playlist in song or SIMA mode.  
You can set a time limit for song to remain in the history. By default it's set to 1 month.  
To do so:

`clean_freq COUNT day(s)|week(s)|month(s)|year(s)`

Example: set clean frequency to 2 weeks.
`clean_freq 2 weeks`

### 3.7 Play mode control

|Command    |Alias          |Decription
|:----------|:--------------|:---------
|- |**cs**  |Toggle **consume mode**.
|- |**rn**  |Toggle **random mode**.
|- |**rp**  |Toggle **repeat mode**.
|- |**sn**  |Toggle **single mode**.
|- |**xf** DURATION_IN_SEC  |Set **crossfade** duration.
|- |**xfo** |Turn **crossfade off**.
|- |**rpg** |Display **replaygain status**.
|- |**rpga**|Enable **replaygain album mode**.
|- |**rpgt**|Enable **replaygain track mode**.
|- |**rpgo**|Enable **replaygain auto mode**.



### 3.8 Database

|Command  |Alias       |Description
|:--------|:-----------|:----------
|**search** STR...STR  |-|**Fuzzy search** in the **database**. Try to match any tag.
|**search_tag** TAG VALUE ... TAG VALUE     |-|**Fuzzy search** for given tags.
|**find_tag** TAG VALUE ... TAG VALUE|-     |**Search** for the **exact match** (case sensitive).
|-|**upd**|Update the database.

Examples:

    ~ search kraftwerk metal on metal
    kraftwerk/3-D The Catalogue/03-01 Trans-Europe Express_Metal On Metal_Abzug 3-D.flac
    kraftwerk/3-D The Catalogue/07-06 Trans-Europe Express_Metal On Metal_ Abzug Headphone Surround 3-D Mix.flac
    kraftwerk/Minimum-Maximum/2-03 Metal On Metal.mp3
    kraftwerk/The Mix/09 Metal On Metal.mp3
    kraftwerk/trans_europe_express/5_metal_on_metal.flac
    ~

    ~ search_tag artist kraftwerk album catalogue disc 7
    kraftwerk/3-D The Catalogue/07-01 The Robots Headphone Surround 3-D Mix.flac
    kraftwerk/3-D The Catalogue/07-02 Computer Love Headphone Surround 3-D Mix.flac
    kraftwerk/3-D The Catalogue/07-03 Pocket Calculator _ Dentaku Headphone Surround 3-D Mix.flac
    kraftwerk/3-D The Catalogue/07-04 Autobahn Headphone Surround 3-D Mix.flac
    kraftwerk/3-D The Catalogue/07-05 Geiger Counter_Radioactivity Headphone Surround 3-D Mix.flac
    kraftwerk/3-D The Catalogue/07-06 Trans-Europe Express_Metal On Metal_Abzug Headphone Surround 3-D Mix.flac
    kraftwerk/3-D The Catalogue/07-07 It's More Fun To Compute_Home Computer Headphone Surround 3-D Mix.flac
    kraftwerk/3-D The Catalogue/07-08 Boing Boom Tschak_Techno Pop_Music Non Stop  Headphone Surround 3-D Mix.flac
    kraftwerk/3-D The Catalogue/07-09 Planet Of Visions Headphone Surround 3-D Mix.flac
    ~

    ~ find_tag artist Kraftwerk title "Metal On Metal"
    kraftwerk/Minimum-Maximum/2-03 Metal On Metal.mp3
    kraftwerk/The Mix/09 Metal On Metal.mp3
    kraftwerk/trans_europe_express/5_metal_on_metal.flac
    ~

### 3.9 MPCWT

This is how to set up mpcwt in polybar:

    [module/mpcwt]
    type = custom/script
    exec = mpcwt
    tail = true
    interval = 0


## 5. Uninstall

`./uninstall.sh`

## 6. TODO

- A decent man page (!)
- Implement customisable aliases (!)
- Dmenu integration (?)
- Enable scroller customisation via mpcwt options.
- Implement error management if no album can be found in **music non-stop** when filtered