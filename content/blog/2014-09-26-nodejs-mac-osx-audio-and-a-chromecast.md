---
title: Mac OS X Audio in node.js, and a Chromecast
date: 2014-09-26T12:05:00-07:00
categories: [code, Javascript, projects, node.js]
---

A few weeks ago, a simple idea formed: why can't you easily stream system audio to a Chromecast? Sure, if your chosen listening application already supports it, you're set. Otherwise you're left out.

A solution seemed simple: node.js has [streams](http://nodejs.org/api/stream.html), and they're awesome! Take system audio, pipe it to an encoder, pipe that to an HTTP response, and tell the Chromecast where to look. Easy!

Except, there's no node.js package that supports OS X audio input, except via [bindings to another library](https://www.npmjs.org/package/node-core-audio). I wanted to write something that was only dependent on the OS X core frameworks.

## Hello [osx-audio][osx-audio]

A few weeks later, and there's a solution (writing your first node.js native binding is hard!). [osx-audio][osx-audio] will allow you to get a readable stream from your currently-selected OS X audio input. It only depends on the frameworks available in Mac OS X, so if you can compile any other native node.js extension, you should be able to install it!

You should check it out if audio input's the thing you need. Output will be implemented in the future too!

[osx-audio]: https://www.npmjs.org/package/osx-audio

## How about the Chromecast?

[Solved][chromecast-osx-audio], but it still requires an additional piece of software. You see, OS X doesn't provide native methods for accessing system audio (the output of the system mixer) so anything that's not an input (like the microphone) is inaccessible. However, there's a open source utility called [Soundflower][soundflower] that solves this for you. If you're unfamiliar with it, here's a step by step to get it working:

1. Install [Soundflower][soundflower]. Reboot.
2. Install [chromecast-osx-audio][chromecast-osx-audio] globally, with an `npm install -g chromecast-osx-audio`
3. Open your *System Preferences -> Sound* preference pane, and select "Soundflower (2ch)" as both your input and output.
4. Run `chromecast` in your terminal. It will find the first-available Chromecast, and stream your system audio to it.
5. Play your music/audio as normal. There is a 5–15	second delay due to how the Chromecast buffers.
6. Enjoy

[chromecast-osx-audio]: https://www.npmjs.org/package/chromecast-osx-audio
[webcast-osx-audio]: https://www.npmjs.org/package/webcast-osx-audio
[soundflower]: http://rogueamoeba.com/freebies/soundflower/

## Ok, great. I don't have a Chromecast

They are pretty cheap! But, I wrote a generic module that just exposes a live mp3 stream of your system audio. It's called [webcast-osx-audio][webcast-osx-audio], and it's what [chromecast-osx-audio][chromecast-osx-audio] relies on for it's streaming component. You can install it just as above, and it exposes a `webcast-audio` command when installed globally. Connect to *http://your_local_ip:3000/stream.mp3* and you're good to go!

## What's next?

- I still need to write the <del>input-side</del> output-side of [osx-audio][osx-audio] before I consider it complete. And tests!
- [chromecast-osx-audio][chromecast-osx-audio] will only stream to the first Chromecast it finds. I plan to implement an interface that allows you to select which you'd like to stream to if multiple are found. That'll be in before v1.0.

Note that both modules command-line interfaces have usage information! Just run `chromecast --help` or `webcast-audio --help` to see what options are available!

*Note: What we go through to solve problems, eh? All of this still assumes some familiarity with node.js—this is definitely not a solution for the non-technical. Maybe someday!*
