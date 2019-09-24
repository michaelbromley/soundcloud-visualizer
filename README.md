# Soundcloud Visualizer

This is an experiment in using the web audio API together with canvas to make some interesting and cool-looking visualizations.

Since this is my first foray into the world of both canvas and web audio, I have slowly iterated over a number of ideas and trials which are all in the `/tests` folder.

The visual style was inspired by the artwork of the album ["The Resistance"](https://en.wikipedia.org/wiki/File:Theresistance.jpg) by Muse.

Thanks to [Soundcloud](https://soundcloud.com) for providing a great open API!

# Demo

[Here's a working demo](https://www.michaelbromley.co.uk/experiments/soundcloud-vis/#muse/undisclosed-desires). Enjoy and share!

**Update May 2015** - As of latest versions of Chrome (42+) and recent versions of Firefox, changes in the way cross-origin audio is handled mean this demo may not work. It's a well-known issue for everyone who's used the SoundCloud API for JavaScript visualizations (there are a lot of us). 

Basically, the CORS headers are set inconsistently by the various CDN servers used by Soundcloud. This means that some tracks will work, and some tracks will fail to play altogether :(

# Instructions

Go to [https://soundcloud.com](https://soundcloud.com) and find some music. From the individual song page, copy the url and paste it into the input of the visualizer, then hit enter or press the play button.

You'll notice that part of the the track URL is added to the visualizer's URL. This makes it possible to easily save or share the state. So, if you find a good track that looks cool with
 the visualization that you want to share, just give the new URL to someone and when they load it up, it'll automatically start streaming and visualizing that track.

## Playlists
Playlists (also known as "sets" in Soundcloud) can now be pasted into the visualizer, which will cause the whole playlist to play in sequence. You can navigate the playlist using the controls below.

## Controls
- `spacebar` = toggle play/pause
- `>` (right arrow key) = skip forward to next track in playlist
- `<` (left arrow key) = skip backward to previous track in playlist

# Issues

Since the web audio API is pretty new, browser support is patchy:

- *Chrome* Latest versions works well, that's what I've been building in.
- *Firefox* This *should* work, since it has supported web audio since version 25, but in my tests the audio fails to play. Any suggestions?
- *Safari* I haven't tested it since I'm stuck with the dead Windows version, but I guess it should work since it's WebKit.
- *IE* Nope.
- *Opera* Not tested but maybe now that they switched to WebKit.

# Improvements

I can think of a few improvements that I might implement, or someone else might want to:

~~- Support for playlists. So you can paste the playlist URL and it just keeps going through to the end of the playlist.~~ - DONE (thanks to [adg29](https://github.com/adg29))
- More types of visualization. I've written the code in a pretty modular way so it's simple to write a visualization that can plug into the app (see below)
- Better browser support. I'm sure there can be more done on this to get it working at least in Firefox and mobile WebKit browsers, with perhaps fallbacks for IE.

# Writing visualizations

If you want to fork this and write your own visualizations, they way I've set it up is pretty simple:
The audio data from Soundcloud is processed by the SoundCloudAudioSource object, which exposes two properties, `volume` and `streamData`, which are continuously updated in real-time.

`streamData` is an array of 128 integers from 0-255, representing the volume of the signal at each frequency.
`volume` is a single integer representing the overall volume, useful for things like pulsing effects that reflect the beat of the audio track.

Your visualization object just needs to consume this object and then it will be able to do whatever it wants to with the data provided. The visualization object should make
it's own canvas elements and append them to the `#visualizer` div.

# License

MIT

