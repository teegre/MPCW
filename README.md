# **MPCW** (12-2019)

**MPCW** is a wrapper+ for Music Player Daemon's client, mpc.<br>
It enables notification on song change, *music non-stop* and playback statistics.<br>
It also provides many aliases/commands to easily control the music player from a terminal.<br>

*Dependencies: mpd, mpc, dunst (or similar), wget, jq*<br>
*Optional dependencies: espeak and mpv (for voice over), zscroll*



## 1. Installation
First, clone this repository 

`git clone https://github/teegre/MPCW.git`

then install **MPCW**:

`./install.sh`



## 2. Configuration

**MPCW** consists in three scripts (written in Bash):

- *mpcw*, which contains commands and aliases,
- *mpcwd*, the daemon that takes care of notifications and statistics.
- *mpcwt*, meant to be used as a module in *polybar* (or similar).

*mpcw* script has to be sourced in your .shellrc file

`source $HOME/.local/bin/mpcw`

To start or stop the **MPCW** daemon:

`mpcwd`


### 2.1 Files

**MPCW** stores files in *$HOME/.config/mpcw*.

|File |Description
|:----|:----------
|mpcw.settings |Settings file.
|mpcw.log |Log file.
|mpcw.hist |Played songs history.
|mpcw.hist.backup|History backup file.
|mpcw.pid | Daemon process id.
|mpcw.wpid | Daemon process id.



## 3. Usage

Commands can be invoked directly from the command line.<br>
Also, it is possible to call **MPCW** functions (ie. as keyboard shortcuts to be used in a tiling window manager), by entering:

`mpcw [command]`



### 3.1 Playback control

|Command       |Alias      |Description
|:-------------|:----------|:----------
|play [track]  |pl         |Start playback. If a track number is provided, play this track.
|pause         |-          |Pause/resume playback.
|toggle        |p          |Play/pause.
|stop          |st         |Stop playback.
|-             |sta        |Stop playback after current song.
|next          |nx         |Go to next song.
|prev          |pv         |Go to previous song.
|skip          |sk         |Skip current song.
|unskip        |usk        |Reset skip count.
|-             |seek [pos] |Seek through current song (use percentage or H:M:S).
|play_album    |pla        |Play current song's album. If song mode is enabled (see 3.4 below), go back to that mode when album is over.
|add_album     |aa         |Add current song's album.
|next_album    |nxa        |When in album mode, play another album.



#### 3.1.1 About play_album and add_album commands

Let's say you are listening to random songs. Then you hear one that you really like and you think you would like to listen to the full album.<br>
So type: `pla` and the album starts to play. If the song you are listening to is the first track of the album, playback continues seamlessly.<br>
Otherwise, playback stops and the first song of the album plays.<br>
But what if you want to listen to the entire song and play the album afterwards? Then type: `aa`, and you're done.

#### 3.1.2 Icons

|Icon |Meaning
|:----|:------
|[>> |Playing.
|`[||`|Paused.
|`[|]`|Stopped.
|(=)|Album mode.
|(-)|Song mode.
|(s)|SIMA mode.

These icons are visible in the notifications.



### 3.2 Playlist control

|Command    |Alias              |Description
|:----------|:------------------|:----------
|pls        |-                  |Display current playlist.
|hist       |-                  |Display playback history (latest first).
|-          |add [song(s)]      |Add song(s) to the playlist.
|-          |new [song(s)]      |Same as add, but clears playlist before adding songs.
|-          |ins [song(s)]      |Insert song(s) after current song.
|-          |move [trk] [trk]   |Move a track.
|-          |del [trk(-trk)]    |Delete song(s) from the playlist.
|-          |cr                 |Crop playlist. Delete all songs except current one.
|-          |clr                |Clear playlist. If song, album or SIMA mode is enabled, new tracks are added to the playlist.
|getrnd [count] [song/album]|rnd|Return [count] song(s) or album(s).
|see_album  |seeal              |Display album.
|see_artist |seear              |Display artist's albums.
|-          |np                 |Display current song info.



### 3.3 Volume control

There are two commands for controlling volume:

- **vol** [-n] [(+/-)value]: set volume. If n option is provided, display a notification. If no argument is given, display actual volume.
- **dim** [-n]: decrease volume by 50% or set volume back to its previous value.<br>If n option is provided, display a notification.<br>*Volume can be dimmed only when a song is playing. It is disabled after playback is stopped*.



### 3.4 Music Non-Stop

Music non-stop enables non-stop playback of songs or albums.

*Note: in song mode, consume, random, and crossfade are enabled.*

It's also possible to play songs/album by artist, genre, date, etc.



#### 3.4.1 Music Non-Stop command

`musicnonstop <album|song|off> <tag value... tag value>`

|Command            |Alias |Description
|:------------------|:-----|:----------
|musicnonstop       |mns   | Display current music non-stop status.
|musicnonstop album |mnsa  | Enable album mode.
|musicnonstop song  |mnss  | Enable song mode.
|musicnonstop off   |mnso  | Disable music non-stop

*Example 1: play random pop songs from the '80s*

`mnss genre pop date 198`

*Example 2: play random flac files*

`mnss filename .flac`

*Note: if the playlist is cleared while playing, new songs will be added according to the current active mode.*



#### 3.4.2 Music Non-Stop SIMA mode

|Command |Alias |Description
|:-------|:-----|:----------
| musicnonstop sima |mnsi |Enable SIMilar Artists mode.

SIMA mode automatically creates artist based playlists.<br>
You can choose what artist to start with by adding one song to an empty playlist.<br>
If playlist is empty, **MPCW** picks a random song and adds similar artists.<br>
When the last song is played, a random song is added and similar artists for this song are added as well.<br>

*Note: an internet connection is required to use this mode.*



#### 3.4.3 Jingles *(optional)* / Voice over

To make *song mode* more lively and sound like an actual radio, I've created a bunch of jingles.<br>
They must be added to your library in order to be used → *directory: mnsr/jingles*<br>
One jingle is added to the playlist every 10 tracks.<br>

There's also a voice over feature (using espeak and mpv) that randomly occurs when a song starts playing (song and SIMA modes only).



### 3.5 Rating songs

- **rating**: show current song's rating.
- **love**, **like**, **tsok**, **soso**, **nope**: rate the song 5, 4, 3, 2 or 1 respectively.
- **unrate**: remove rating.



### 3.6 Statistics

- **playcount**: display song's play count.
- **lastplayed**: display the last date and time the song was played.
- **skipcount**: display song's skip count.
- **reset_stats** [file]: reset statistics for a given file.



#### 3.6.1 About skipping and history.

It's possible to set a skip limit, so the skipped songs won't be added to the playlist in song or SIMA mode.

By default the limit is set to 2.

To change it:

|Command                | Alias
|:----------------------|:-----
|set_skip_limit [count] | skl


Also, songs that are listed in the history won't be added to the playlist in song or SIMA mode.<br>
You can set a time limit for song to remain in history. By default it's set to 1 month.<br>
To do so:

`clean_freq [num (day(s)|week(s)|month(s)|year(s))]`

Example: set clean frequency to 2 weeks.
`clean_freq 2 weeks`

### 3.7 Play mode control

|Command|Alias|Decription
|:------|:----|:---------
|- |cs |Toggle consume mode.
|- |rn |Toggle random mode.
|- |rp |Toggle repeat mode.
|- |xf [duration] |Crossfade.
|- |xfo |Turn crossfade off.
|- |rpg |Display replaygain status.
|- |rpga |Enable replaygain album mode.
|- |rpgt |Enable replaygain track mode.
|- |rpgo |Enable replaygain auto mode.



### 3.8 Database

|Command|Alias|Description
|:------|:----|:----------
|search [str...str]|-|Fuzzy search in the database. Try to match any tag.
|search_tag [tag] [value] ... [tag] [value]|-|Search for given tags.
|find_tag [tag] [value] ... [tag] [value]|-|Search for the exact match (case sensitive).
|-|upd |Update the database.

Examples:

    ~ search kraftwerk metal on metal
    kraftwerk/3-D The Catalogue/03-01 Trans-Europe Express _ Metal On Metal _ Abzug 3-D.flac
    kraftwerk/3-D The Catalogue/07-06 Trans-Europe Express _ Metal On Metal _ Abzug Headphone Surround 3-D Mix.flac
    kraftwerk/Minimum-Maximum/2-03 Metal On Metal.mp3
    kraftwerk/The Mix/09 Metal On Metal.mp3
    kraftwerk/trans_europe_express/5_metal_on_metal.flac
    ~

    ~ search_tag artist kraftwerk album catalogue disc 7
    kraftwerk/3-D The Catalogue/07-01 The Robots Headphone Surround 3-D Mix.flac
    kraftwerk/3-D The Catalogue/07-02 Computer Love Headphone Surround 3-D Mix.flac
    kraftwerk/3-D The Catalogue/07-03 Pocket Calculator _ Dentaku Headphone Surround 3-D Mix.flac
    kraftwerk/3-D The Catalogue/07-04 Autobahn Headphone Surround 3-D Mix.flac
    kraftwerk/3-D The Catalogue/07-05 Geiger Counter _ Radioactivity Headphone Surround 3-D Mix.flac
    kraftwerk/3-D The Catalogue/07-06 Trans-Europe Express _ Metal On Metal _ Abzug Headphone Surround 3-D Mix.flac
    kraftwerk/3-D The Catalogue/07-07 It's More Fun To Compute _ Home Computer Headphone Surround 3-D Mix.flac
    kraftwerk/3-D The Catalogue/07-08 Boing Boom Tschak _ Techno Pop _ Music Non Stop  Headphone Surround 3-D Mix.flac
    kraftwerk/3-D The Catalogue/07-09 Planet Of Visions Headphone Surround 3-D Mix.flac
    ~

    ~ find_tag artist Kraftwerk title "Metal On Metal"
    kraftwerk/Minimum-Maximum/2-03 Metal On Metal.mp3
    kraftwerk/The Mix/09 Metal On Metal.mp3
    kraftwerk/trans_europe_express/5_metal_on_metal.flac
    ~

### 3.9 mpcwt

This is how to set up mpcwt in polybar:

    [module/mpcwt]
    type = custom/script
    exec = mpcwt
    tail = true
    interval = 0

*Note: zscroll is needed to use this script.*

> | [>> KTL: Last Spring: A Prequel`



## 5. Uninstall

`./uninstall.sh`

