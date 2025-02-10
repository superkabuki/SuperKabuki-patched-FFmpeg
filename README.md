# FFmpeg with the SuperKabuki SCTE-35 patch applied.

## ...why?
If you work with SCTE-35 and you use ffmpeg,<br> having the SCTE-35 stream type 0x86 changed to bin data 0x06, makes your life hard.

<br>


## What does it do?

* The patch is only nine lines of code, it allows you copy a SCTE-35 stream over as SCTE-35, when you're encoding with ffmpeg.
* The patch also adds the SCTE-35 Descriptor __(CUEI / 0x49455543)__ , just to be fancy.
* Everything else works just like unpatched ffmpeg.
---


## Install 


1.    `git clone https://github.com/superkabuki/FFmpeg_SCTE35.git`

2.    `cd FFmpeg_SCTE35`

3.    `./configure --enable-shared --extra-version=-SuperKabuki-patch` ( you can customize this as needed )

4.    `make all`

5.    `sudo make install`

---

## How to use:

Use it just like unpatched FFmpeg.

---

# Examples

1.  Re-encode video to h.265, audio to aac, copy over the SCTE-35, and keep the timestamps.


```smalltalk
ffmpeg -copyts -i input.ts -map 0  -c:v h265 -c:a aac -c:d copy -muxpreload 0 -muxdelay 0 output.ts
```



* original file
![image](https://github.com/user-attachments/assets/b8816336-37a8-439e-87a1-d904f2815d7c)

* ffmpeg command
![image](https://github.com/user-attachments/assets/3c0190b0-479e-40ce-9c2e-9168919489a8)

* new file
![image](https://github.com/user-attachments/assets/2b76b386-814f-431b-a07a-a6eaa7001a12)

---

2. Copy all streams including SCTE-35, cut the first 200 seconds, and keep the timestamps.

```smalltalk
ffmpeg -copyts -ss 200 -i input.ts -map 0  -c copy -muxpreload 0 -muxdelay 0 output.ts
```

* original file
![image](https://github.com/user-attachments/assets/30d88882-0814-4609-92fc-53ef29e77bae)

* ffmpeg command
 ![image](https://github.com/user-attachments/assets/21b1b49a-c9a2-4e8b-8322-2b4f5755a51e)

* new file
![image](https://github.com/user-attachments/assets/f2cf31c6-90a4-428c-97bd-4ca82823fc71)


> Notice the start time and duration have both changed by ~200 seconds.

* old
```js
 Duration: 00:04:56.30, start: 72667.595200, bitrate: 962 kb/s
```
* new
```js
  Duration: 00:01:37.75, start: 72866.141867, bitrate: 962 kb/s
```
---

