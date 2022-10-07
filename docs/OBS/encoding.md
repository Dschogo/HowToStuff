# OBS - encoding comparison 

---

If you found that page, its more or less clear that you already know about OBS and the basics (Otherwise there are pleanty of great tutorials out there). I'Ve tested all folowing with veresion 28.0.3 (64bit) but since those are encoder settings, they should work with any version.

## Best settings for Twitch?

Okey first, what does Twitch recommend? [reference](https://stream.twitch.tv/encoding/)

> For 1080p60:
>
> |  | NVENC | x264  |
| ------------ | ------------ | ------------ |
| Bitrare | cbr 6000 kbps  | cbr  6000 kbps  |
| preset: | Quality  | veryfast <-> medium |
| Profile | NaN | Main/High |

<sub>*Nvenc = NVIDIA accelerated encoding on GPU | x264 = software encoding (CPU)</sub>

It's 2022 so im not gonna really bother testing lower resolutions, but it should mostly match the upcoming outcome.

## Testing method

Since the image quality is pretty close and we as humans cant watch to videos simultaneously, I'm relying on software to compare the encoders with different settings.

As Software tool I came across [VMAF](https://github.com/Netflix/vmaf) (Video Multimethod Assessment Fusion), made by Netflix. It tries to measure the perceptual video quality by comparing it to a reference video. The maintainers of the GitHub repo were kind and linked a functional GUI by fifonik called [FFMetrics](https://github.com/Netflix/vmaf).

<sub>*before using FFMetrics you have to install ffmpeg and add it to your Path</sub>
<sub>**I've used 1.3.1 beta 2 for this test.</sub>

1. ### We need a reference video

    This video should has a ~much higher bitrate than the target bitrate. For us the target is 6kb/s, 20kb/s should be fine, my test video had vbr_50kb/s+. Additionally my video was 4k but that doesnt matter for the test. (Also downscaled it to 1080p with 70kb/s_vbr to test against, same result)

2. ### Encode the videos

    Im using Adobe MediaEncoder (2022) for this, with the [voukoder](https://www.voukoder.org/forum/thread/783-downloads-instructions/) plugin. Any other software with NVENC support should work as well. (Handbrake, or bare bones ffmpeg works)

    Encoder set to CBR 6kb/s presets visible in name.  
    Note: X264 tries to break out of the 6kb/s limit (not sure why, 7kb/s would be still fine for twitch, and in real world bitrate will vary a bit anyway), just to keep a little in mind when comparing the results.  

    To omptimize NVENC I've used following:
    > b=6000000 b_adapt=1 bf=4 nonref_p=1 preset=p7 profile=main rc=cbr spatial-aq=1 temporal-aq=1
    
!> Following voukouder settings are found like that in obs:  
b = bitrate | b_adapt = Look-ahead | bf = Max B-frames | preset = p7 equals"Max quality" | profile = profile | rc = rate control | spatial-aq and temporal-aq = are both under "Psycho Visual Tuning"
    <sub>*kb/s are average over the whole 20 seconds</sub>

## Results

Why did I "optimize"/ took extra steps for NVENC? well look without and with:
![](/img/unoptimized_hvenc_max.png)
![](/img/optimized_hvenc_max.png)

Software x264 is better in such difficult scenarios with limited bitrate:
![](/img/slower_high_x264.png)

But there is still a big downside to Software x264, it's running completly on the CPU.  
My highest used setting was "slower" (there ist still very slow and placebo), but slower was already the point where the CPU was at 100% usage, so realtime rendering without hickups is not possible, playing a game next to it is not possible.  
My CPU is a Ryzen 5 3600 on 4.2ghz, but even with more cores its not really justifiable to use x264 for streaming, except for a dual system configuration.

## Technical results

![](/img/FFmetrics_results.png)


Additionally avoid the faster presets, because they vary from frame to frame with their quality, as seen in the following graph. Im guessing, the spikes are the B-Frames and the intermediate frames are with a lower quality.
![](/img/plot.png)


## Hold up - there is AV1 coming????

Yes, but you can complety forget software encoding - for the same quality of x264 you need atlast 5 times the render time. Intel Arc and the GeForce 4000 and probably upwards cards support AV1 hardware encoding, but none of the streaming providers support it yet. There is barely support for decoding (RTX 3000 supports its).

Suprisingly almost all samrtphone support AV1 decoding, and also encoding. Very good for IRL-livestreaming with low bandwith - Another topic.

## And What about the framerate?

Good thought, I thought about that too, like 6kb/s on 48fps or even 30 - 30, that should be doubled the quality with the same bitrate, right? Nope, not at all. I've run test with different framerates (60 high bitrate to 60 with 6kb/s, to 48 and to 30). The results were pretty much the same, almost negible difference in the real world.
Speaking of maybe max 1% difference, but that is not really noticable, plus with 48fps, if you want to record/ download your VOD and cut it, upload it to youtube, you will probably use 60, so frameinterpolation - and thats not worth it. Simply stick with 60fps.

## Conclusion

NVENC is the way to go, if you have a NVidia GPU. AMD has a similar feature called VCE (i think its called?), but I've never tested it myslef, should perform about the same, with the same benefits. Not sure if the extra options (Pyscho Visual Tuning and Look-ahead) are available for AMD, but there is probably something similar.  
Don't forget to enable them, otherwise enjoy the blocky mess :D

