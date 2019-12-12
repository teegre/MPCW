# **MPCW** version 0.1.0 (12-2019)

**MPCW** is a wrapper (written in bash) for Music Player Daemon's client, mpc.

It enables notification on song change, *music non-stop* and playback statistics.

It also provides many aliases/commands to easily control the music player from a terminal.

*Dependencies: mpd, mpc, dunst (or similar)*

*Optional dependencies: espeak, zscroll*


## 1. Installation

First, clone this repository then install *mpcw*:

`./install.sh`


## 2. Configuration

This script has to be sourced in your .shellrc file

`source $HOME/.local/bin/mpcw`


## 3. Usage

Commands can be invoked from the command line. Also, it is possible to call mpcw functions (ie. as keyboard shortcuts to be used in a tiling window manager), by entering:

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
|next_album    |nxa        | When in album mode, play another album.


### 3.2 Playlist control

|Command    |Alias              |Description
|:----------|:------------------|:----------
|pls        |-                  |Display current playlist.
|hist       |-                  |Display playback history (latest first).
|-          |add [song(s)]      |Add song(s) to the playlist.
|-          |new [song(s)]      |Same as add, but clears playlist before adding songs.
|-          |ins [song(s)]      |Insert song(s) after current song.
|-          |del [trk(-trk)]    |Delete song(s) from the playlist.
|-          |cr                 |Crop playlist. Delete all songs except current one.
|-          |clr                |Clear playlist.
|see_album  |seeal              |Display album.
|see_artist |seear              |Display artist's albums.


### 3.3 Volume control

There are two commands for controlling volume:

- **vol** [(+/-)value]: set volume. If no argument is given, display actual volume.
- **dim**: decrease volume by 50% or set volume back to its previous value. (Volume can be dimmed only when a song is playing. It is disabled after playback is paused or stopped.)


### 3.4 Music Non-Stop

Music non-stop enables non-stop playback of songs or albums.

*Note: in song mode, consume, random, and crossfade are enabled.*

It's also possible to play songs/album by artist, genre, date, etc.


#### 3.4.1 Music Non-Stop command

`musicnonstop <album|song|off> <tag value... tag value|off>`

|Command            |Alias |Description
|:------------------|:-----|:----------
|musicnonstop       |mns   | Display current music non-stop status.
|musicnonstop album |mnsa  | Enable album mode.
|musicnonstop song  |mnss  | Enable song mode.
|musicnonstop off   |mnso  | Disable music non-stop

*Example 1: play random pop songs from the '80s*

`mns song genre pop date 198`

*Example 2: play random flac files*

`mns song filename .flac`



#### 3.4.2 Music Non-Stop SIMA mode

|Command            |Description
|:------------------|:----------
| musicnonstop sima | Enable SIMilar Artists mode.

SIMA mode automatically create artist based playlists.

You can choose what artist to start with by adding one song to the playlist.

If playlist is empty, mpcw picks a random song and adds similar artists.

When the last song is played, a random song is added and similar artists for this song are added as well.


#### 3.4.3 Jingles *(optional)* / Voice over

To make *song mode* more lively and sound like an actual radio, I've created a bunch of jingles.

They must be added to your library in order to be used.

One jingle is added for each 10 tracks.

There's also a voice over feature (using espeak) that randomly occurs when a song starts playing.


### 3.5 Rating songs

- **rating**: show current rating.
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


Songs that are listed in the history won't be added to the playlist in song or SIMA mode.

You can set a time limit for song to remain in history. By default it's set to 1 month.

To do so:

`clean_freq [num (day(s)|week(s)|month(s)|year(s))]`

Example: clean history for song older than 2 weeks.
`clean_freq 2 weeks`