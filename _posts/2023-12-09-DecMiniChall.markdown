---
layout: post
title: "CSIT December 2023 Mini Challenge: Audio pre-processing"
date: 2023-12-09 21:44:00 +0800
author: Elijah
categories: jekyll update
tags: forensics
---

<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

The CSIT Mini Challenge is a monthly challenge organised by CSIT, aimed to raise awareness on the various tech focus areas of CSIT. Participants earn a digital badge on completing the challenge, which they can feature on their social media platforms.

## December Mini Challenge

[This month's Mini Challenge](https://dse-mini-challenge.csit-events.sg/) focuses on audio data pre-processing, which is an important part in the data science workflow. It involves cleaning and transforming raw data into a format suitable for analysis and modelling.

This improves the accuracy of models and reduces the risk of biased or unreliable results. This allows data science to obtain meaningful insights and make informed decisions.

We are given 3 tasks involving audio data preprocessing. We will have to manipulate sound data and obtain hidden information. I used [Jupyter Notebook](https://jupyter.org/) for most of this Mini Challenge as it's easy to use and this challenge mostly involves Python.

The challenge files can be downloaded [here](/assets/DEC_CSIT_CHALL/CSIT_DS_MINI_CHALL.zip).

## Task 1

<img src = "/assets/DEC_CSIT_CHALL/DEC_CSIT_CHALL_TASK1.png" alt="Task 1 Description" style="width: 650px; height: 500px">

In the first task, we are asked to use basic audio manipulation techniques to obtain a geographic location. The resources provided are as follows:

- [Basic audio processing library for data scientists](https://musicinformationretrieval.com/ipython_audio.html)
- [Get Sampling Rate of an Audio File](https://librosa.org/doc/main/generated/librosa.get_samplerate.html)
- [Load Audio into a 1-D Sound Array](https://librosa.org/doc/main/generated/librosa.load.html)
- [Reverse an Array](https://www.askpython.com/python/array/reverse-an-array-in-python)
- [Speed Up / Slow Down a 1-D Sound Array](https://librosa.org/doc/main/generated/librosa.effects.time_stretch.html)
- [Write a 1-D Sound Array into an Audio File](https://www.programcreek.com/python/example/95990/soundfile.write)
- [Google Maps](https://www.google.com/maps)

From the learning resources provided, it seems like we have to extract the audio into a 1-D Sound Array we can manipulate, then reverse that array, speed it up or slow it down, and write it into an audio file.

The following code can be used to obtain the result:

```python
import soundfile as sf
import librosa
y, sr = librosa.load('Task_1/T1_audio.wav') # Obtain 1-D sound array and sampling rate
y_rev = y[::-1] # Reverse the 1-D sound array
rev_y_fast = librosa.effects.time_stretch(y_rev, rate = 2.0) # Double the speed of the audio
sf.write('output1.wav', rev_y_fast, 22050) # Output the audio
```

The resultant audio file can be found [here](/assets/DEC_CSIT_CHALL/output1.wav).

We are given the coordinates 67.9222N, 26.5046E. We can google this and find that it's the location of Lapland, a place in Finland home to Santa Claus.

## Task 2

<img src = "/assets/DEC_CSIT_CHALL/DEC_CSIT_CHALL_TASK2.png" alt="Task 2 Description" style="width: 700px; height: 500px">

In the second task, we are given four audio files which sound like snippets from popular music tracks, but with additional noises resembling bird sounds. The learning resources given are as follows:

- [What else can be hidden in an audio clip?](https://www.kaggle.com/code/jaseemck/audio-processing-using-librosa-for-beginners)
- [Understanding visual audio](https://www.pnsn.org/spectrograms/what-is-a-spectrogram)

The first resource contains several ways of audio visualisation, while the second resource tells us more about spectrograms. We can execute the following code to make and view spectrograms from the provided audio files:

```python
import matplotlib.pyplot as plt
import librosa.display

# Load the audio files
x1, sr1 = librosa.load('Task_2/T2_audio_a.wav')
x2, sr2 = librosa.load('Task_2/T2_audio_b.wav')
x3, sr3 = librosa.load('Task_2/T2_audio_c.wav')
x4, sr4 = librosa.load('Task_2/T2_audio_d.wav')

# Obtain spectrograms of each audio file
X1 = librosa.stft(x1) # stft = short time fourier transformation
Xdb = librosa.amplitude_to_db(abs(X1)) # convert amplitude to decibels
plt.figure(figsize=(14, 5))
librosa.display.specshow(Xdb, sr=sr1, x_axis='time', y_axis='hz') # plots the spectrogram

X2 = librosa.stft(x2)
Xdb = librosa.amplitude_to_db(abs(X2))
plt.figure(figsize=(14, 5))
librosa.display.specshow(Xdb, sr=sr2, x_axis='time', y_axis='hz')

X3 = librosa.stft(x3)
Xdb = librosa.amplitude_to_db(abs(X3))
plt.figure(figsize=(14, 5))
librosa.display.specshow(Xdb, sr=sr3, x_axis='time', y_axis='hz')

X4 = librosa.stft(x4)
Xdb = librosa.amplitude_to_db(abs(X4))
plt.figure(figsize=(14, 5))
librosa.display.specshow(Xdb, sr=sr4, x_axis='time', y_axis='hz')
```

We then obtain the following spectrograms:

### From audio a:

<img src = "/assets/DEC_CSIT_CHALL/A_Spectrogram.png" alt="Audio A interpretation" style="">

This spells "MINS".

### From audio b:

<img src = "/assets/DEC_CSIT_CHALL/B_Spectrogram.png" alt="Audio B interpretation" style="">

This spells "NOON".

### From audio c:

<img src = "/assets/DEC_CSIT_CHALL/C_Spectrogram.png" alt="Audio C interpretation" style="">

This spells "SEVEN".

### From audio d:

<img src = "/assets/DEC_CSIT_CHALL/D_Spectrogram.png" alt="Audio D interpretation" style="">

This spells "PAST".

### Solution

Rearranging the words, we get "Seven minutes past noon", so our answer is simply 12:07.

## Task 3

<img src = "/assets/DEC_CSIT_CHALL/DEC_CSIT_CHALL_TASK3.png" alt="Task 3 Description" style="width: 600px; height: 500px">

In the third task, we are given an audio file that can't be comprehended. The learning resources provided are as follows:

- [Here, have this broom... And clean up this mess of an audio clip!](https://realpython.com/python-scipy-fft/#using-the-fast-fourier-transform-fft)
- [Retaining audio data between 100Hz to 2000Hz should suffice to decipher the hidden message. In practice, applying the Fourier Transform to real world signals is not ideal. To delve into practical signal filtering, please explore windowing and Short-Time Fourier Transform.](https://www.youtube.com/watch?v=-Yxj3yfvY-4)
- [It might get a little too soft after all the cleaning up...](https://stackoverflow.com/questions/42492246/how-to-normalize-the-volume-of-an-audio-file-in-python)
- [Need some help with a foreign language? Try OpenAI's Whisper AI, an open-source state-of-the-art Speech-to-text model. Consider using the "large-v2" model for the best performance.](https://github.com/openai/whisper/blob/main/README.md)

We can first obtain a power spectrogram of the audio to see how it has been manipulated:

```python
import numpy as np

y, sr = librosa.load('Task_3/C.Noisy_Voice.wav')
S = np.abs(librosa.stft(y))
fig, ax = plt.subplots()
img = librosa.display.specshow(librosa.amplitude_to_db(S,ref=np.max),y_axis='log', x_axis='time', ax=ax)
ax.set_title('Power spectrogram')
fig.colorbar(img, ax=ax, format="%+2.0f dB")
```

We obtain the following image:

<img src = "/assets/DEC_CSIT_CHALL/Task3_Power_Spec.png" alt="Task 3 Power Spectrogram" style="">

Clearly, additional noise has been added above 2048Hz and below 20Hz. In order to hear the concealed audio, we need to remove the unwanted noise. We can use the `scipy` library to do this:

```python
from scipy.io import wavfile
from scipy.fft import rfft, rfftfreq, irfft

samplerate, data = wavfile.read('Task_3/C.Noisy_Voice.wav')
N = len(data)
yf = rfft(data)
xf = rfftfreq(N, 1 / samplerate)

# The maximum frequency is half the sample rate
points_per_freq = len(xf) / (samplerate / 2)
target_idx = int(points_per_freq * 100) # target 100Hz frequency
target_idx2 = int(points_per_freq * 2000) # target 2000Hz frequency
yf[:target_idx] = 0 # Filter out all frequencies below 100Hz
yf[target_idx2:] = 0 # Filter out all frequencies above 2000Hz
new_sig = irfft(yf) # Apply inverse FFT
norm_new_sig = np.int16(new_sig * (32767 / new_sig.max())) # Normalize the signal
write("clean.wav", samplerate, norm_new_sig) # Write the signal to a file
```

Upon listening to the produced audio file, it is too soft to be heard, so we can use a code snippet from the stackoverflow resource as follows:

```python
from pydub import AudioSegment

def match_target_amplitude(sound, target_dBFS):
    change_in_dBFS = target_dBFS - sound.dBFS
    return sound.apply_gain(change_in_dBFS)

sound = AudioSegment.from_file("clean.wav", "wav")
normalized_sound = match_target_amplitude(sound, -20.0)
normalized_sound.export("normalizedAudio.wav", format="wav")
```

The audio obtained can be found [here](/assets/DEC_CSIT_CHALL/normalizedAudio.wav). Since it's not in english, we need to use the whisper AI to translate the audio to english.

After following the setup instructions from the `whisper` GitHub page, we can run the following command in terminal. Here, we use the large-v2 model as recommended in the task description.

```bash
whisper normalizedAudio.wav --task translate --model large-v2
```

We obtain the following results:

<img src = "/assets/DEC_CSIT_CHALL/Task3_Whisper_Results.png" alt="Task 3 Whisper Result" style="">

These are lyrics to the song "Do You Want to Build a Snowman?". We can guess that the answer object is snowman.

## Conclusion

I had alot of fun with the challenges, and understanding how to use the `librosa`, `whisper` and `scipy` libraries. I also appreciate the resources and hints provided so we could solve the challenges more efficiently.
