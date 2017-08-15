---
title: "Making noise in Python"
date: 2013-02-16T14:44:00-07:00
categories: [code, Python, PyAudio, sound, synthesizers]
---

I've been working with [PyAudio](https://github.com/bastibe/PyAudio) lately, on a project to synchronize sound streams across multiple devices. Nothing to say on that front yet, but I do have a nice snippet for programatically generating a tone:

```python
import math
import numpy
import pyaudio


def sine(frequency, length, rate):
    length = int(length * rate)
    factor = float(frequency) * (math.pi * 2) / rate
    return numpy.sin(numpy.arange(length) * factor)


def play_tone(stream, frequency=440, length=1, rate=44100):
    chunks = []
    chunks.append(sine(frequency, length, rate))

    chunk = numpy.concatenate(chunks) * 0.25

    stream.write(chunk.astype(numpy.float32).tostring())


if __name__ == '__main__':
    p = pyaudio.PyAudio()
    stream = p.open(format=pyaudio.paFloat32,
                    channels=1, rate=44100, output=1)

    play_tone(stream)

    stream.close()
    p.terminate()
```

This simply generates a sine wave of a specified frequency and length, and writes it out to an already open PyAudio stream. A pleasant tone is produced. It's not too fancy, but it beats loading a wave file from disk.
