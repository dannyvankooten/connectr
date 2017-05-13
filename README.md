# ![icon](connectr_80px_300dpi.png) connectr ![icon](connectr_80px_300dpi.png)

connectr is a Rust library and systray/menubar application for controlling and monitoring [Spotify Connect](https://www.spotify.com/se/connect/) devices.

As a library, connectr exposes the official [Spotify 'Player' Web API](https://developer.spotify.com/web-api/web-api-connect-endpoint-reference/) for controlling Spotify Connect devices.

As an application, connectr provides a minimal "systray" application to observe the currently playing track, control Spotify playback, switch playback devices, and start preset playlists.  The goal is very low memory usage, so the basic Spotify functionality can always be available without keeping the massive Spotify desktop application resident in memory.

*Note:* connectr is not an audio playback tool; it's just a remote control.  Spotify has not publicly released a library for implementing audio playback with Spotify Connect support.  There's a reverse engineering effort, coincidentally also in Rust, at [librespot](https://github.com/plietar/librespot).

### Development Status
Alpha / Hobby / Experimental

### Code Status
Sloppy; this is my first Rust.

### Cargo Crate
[connectr on crates.io](https://crates.io/crates/connectr)

### Platform Status

The underlying library should be fully cross-platform, though I'm only testing x86_64 Windows and OS X.  Let me know if you run it on an ARM; I'd like to know if that works.

*Web API Library*:
Fully functional and pretty stable for the requirements of the connectr menu bar app.  Error handling isn't extremely robust, and it doesn't implement retries or exponential backoff, which it should.  The Spotify API can, of course, do plenty more than connectr exposes.

*Systray/Menubar Application*:
* Mac OS X - Done
* Windows - Done
* Linux - Maybe someday

### Screenshot
<img src="https://github.com/mrmekon/connectr/blob/master/docs/screenshot.png" width="300">
<img src="https://github.com/mrmekon/connectr/blob/master/docs/screenshot_windows.png" width="300">

### Instructions

No binaries are provided.  You must build from source with Cargo.

Create a Spotify application here: https://developer.spotify.com/my-applications
**Note:** Spotify Premium is required to create an application.  You must have Premium to use connectr.
You must add 'http://127.0.0.1:5432' as a Redirect URI for your application.
Copy the Client ID and Client Secret to connectr's configuration file (see below).

Something like this:
```
$ git clone https://github.com/mrmekon/connectr.git
$ cd connectr
$ cp connectr.ini.in ~/.connectr.ini
$ ./clientid_prompt.sh
$ cargo run
```

You must provide your Spotify application's client ID and secret in connectr's configuration file.  This is handled by the `clientid_prompt.sh` script, or can be done manually.

**Note:** connectr uses `~/.connectr.ini` if it exists.  If it does _not_ exist, connectr will fallback to trying `connectr.ini` in the directory it is run from.  If built as an OS X application, connectr will create `~/.connectr.ini` on first launch, but will fail to run until you add your Client ID and Secret.  The included script `clientid_prompt.sh` can optionally be used to generate `~/.connectr.ini`; it will prompt for your Client ID and Secret when run.  A template is provided in `connectr.ini.in`.

### Configuration file (connectr.ini) format

connectr's configuration is read from a regular INI file with these sections:

#### [connectr]
* port - Port to temporarily run web server on when requesting initial OAuth tokens (integer)

#### [application]
* client_id - Spotify web application's Client ID (string)
* secret - Spotify web application's Client Secret (string)

#### [presets]
* [name] - Key name is the display name of a playable preset, the value must be a Spotify URI to play. (string)

_ex: `Bakesale=spotify:album:70XjdLKH7HHsFVWoQipP0T` will show as 'Bakesale' in the menu, and will play the specified Sebadoh album when clicked._

#### [tokens]
* access - Spotify Web API access token
* refresh - Spotify Web API refresh token
* expire - Expiration time (UTC) for access token

_note: this section is auto-generated and auto-updated at runtime_

#### Example connectr.ini
```
[connectr]
port=5432

[application]
secret=xxxxxyyyyyaaaaabbbbbcccccddddd
client_id=xXxXxyYyYynNnNnNmMmMmMpPpPpP

[presets]
Discover Weekly=spotify:user:spotify:playlist:37i9dQZEVXcOmDhsenkuCu
Edge Detector=spotify:user:mrmekon:playlist:4SKkpDbZwNGklpIILmEZAg
Play Today=spotify:user:mrmekon:playlist:4c8eKK6kKrcdt1HToEX7Jc

[tokens]
access=this-is-autogenerated
refresh=this-is-also-autogenerated
expire=1492766270
```

### Progress

| Feature                                | OS X                    | Windows                 | Linux                   |
| ---                                    | ---                     | ---                     | ---                     |
|                                        |
|                                        |
| **API**                                |
| Fetch list of devices                  | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> |
| Fetch current playback information     | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> |
| Transfer playback to device            | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> |
| Start new playback on device           | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> |
| Pause/Resume                           | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> |
| Skip next/previous                     | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> |
| Seek in track                          | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> |
| Change volume                          | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> |
| Change repeat state                    | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> |
| Change shuffle state                   | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> |
| Fetch album art                        | <ul><li> [ ] </li></ul> | <ul><li> [ ] </li></ul> | <ul><li> [ ] </li></ul> |
|                                        |
|                                        |
| **UI**                                 |
| Display current track                  | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> | <ul><li> [ ] </li></ul> |
| Current track in tooltip               | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> | <ul><li> [ ] </li></ul> |
| Playback controls                      | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> | <ul><li> [ ] </li></ul> |
| Device selection                       | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> | <ul><li> [ ] </li></ul> |
| Volume control                         | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> | <ul><li> [ ] </li></ul> |
| Presets                                | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> | <ul><li> [ ] </li></ul> |
| Save current track to playlist         | <ul><li> [ ] </li></ul> | <ul><li> [ ] </li></ul> | <ul><li> [ ] </li></ul> |
|                                        |
|                                        |
| **System**                             |
| Persistent configuration               | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> | <ul><li> [ ] </li></ul> |
| System logging                         | <ul><li> [x] </li></ul> | <ul><li> [x] </li></ul> | <ul><li> [ ] </li></ul> |
| Installer                              | <ul><li> [x] </li></ul> | <ul><li> [ ] </li></ul> | <ul><li> [ ] </li></ul> |

