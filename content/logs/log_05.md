---
date: 2018-12-20T15:54:00+08:00
description: ""
featured_image: ""
tags: []
title: "Converting Video to ASCII via LibCACA"
featured_image: '/images/log_05/voCACA_00.jpg'
---

## Method 1: Screen Recording
> 在 CPU 性能足够的情况下，可以通过录制屏幕的方式来获得 LibCACA 输出的 ASCII 视频

### 1. List Devices
> 首先列举出 AVFoundation 的输入设备

```
ffmpeg -f avfoundation -list_devices true -i ""
```

### 2. Rec Screen
> 在第一步的基础上，使用 FFmpeg 选择需要的输入设备进行录制

- e.g. 1

	```
ffmpeg -y -f avfoundation -i 1:3 -framerate 30 -c:v libx264 -r 60 -pix_fmt uyvy422 -preset 0 -crf 19 -c:a aac -b:a 192k "$INPUT $(date "+%Y-%m-%d %H-%M-%S").mp4"
	```

- e.g. 2

	```
ffmpeg -y -f avfoundation -i 2:3 -framerate 30 -c:v libx264 -r 60 -pix_fmt yuv420p -preset 0 -crf 19 -c:a aac -b:a 192k "$INPUT $(date "+%Y-%m-%d %H-%M-%S").mp4" 
	```

### * Rec Terminal
> 另一种办法是录制终端

- e.g.

	```
ffmpeg -f x11grab -s wxga -r 25 -i :0.0 -sameq /tmp/out.mpg
	```


## Method 2: Format Converting
> Process: Video --> JPG --> HTML --> JPG --> Video
> 
> 此方法主要针对 CPU 性能不足的情况（会在录制时掉帧），通过对视频内容的逐帧转换来完成

### 1. Video to JPG via `ffmpeg`
> 通过 FFmpeg 完成对视频的逐帧截取，截图格式为 JPG

```
ffmpeg -i $video_fps -r 30 -s $video_resolution -f image2 %7d.jpg
```

### 2. JPG to HTML via `img2txt`
> 通过 img2txt 逐帧完成视频的 ASCII 风格转换，存为 HTML

```
for ascii in $(ls *.jpg ) do ...
	img2txt -W 128 -H 36 -x 3 -y 5 $(basename $ascii .jpg) -f html > _$ascii.html;
```

- "-W 128" means 128 ASCII columns
- "-H 36" means 36 rows
- "-x 3 -y 5" means font size 3x5

### 3. HTML to PNG via `webkit2png`
> 通过 webkit2png 完成对 HTML 到 PNG 的逐帧转换

```
for LibCaca in $(ls *.html) do ...
	webkit2png -o $(basename $LibCaca .jpg.html) $LibCaca;
```

### 4. PNG to Video via `ffmpeg`
> 最后再次使用 FFmpeg，完成对图片的重新封装，导出成视频
> 
> 可以先用 convert 将 PNG 图片先转换为 JPG 图片

```
ffmpeg -threads $thread_amount -r $video_fps -i %07d.jpg png_packed_up.mp4


# YUV444 to YUV420
ffmpeg -i $INPUT -c:v libx264 -profile:v high -crf 20 -pix_fmt yuv420p $OUTPUT

# Frame Size Modify
ffmpeg -i $INPUT -c:a copy -vf crop=1920:1080:0:0 $OUTPUT

# Scale and Pad
ffmpeg -i $INPUT -c:a copy -vf "scale=1920:-1:force_original_aspect_ratio=decrease, pad=1920:1080:0:(1080-in_h)/2:black" -movflags +faststart $OUTPUT
```

> Ref: 4:3 --> 16:9
> 
> e.g.
```
-vf cropdetect, pad=ih*16/9:ih:(ow-iw)/2:0:black, scale=1920:1080
```

## Other
### 1. Screenshot Capturing
> 用于截取视频中的一帧用于视频封面等

```
ffmpeg -ss 00:04:03 -i $INPUT -vframes 1 -f mjpeg sample.jpg
```

### 2. `DISPLAY` Setting
> 在使用如`mpv --vo=caca` 命令时，有关在 XQuartz 的窗口或是在终端内输出视频的偏好设置

```
echo $DISPLAY
DISPLAY=/tmp/com.apple.launchd.cZDh63T4aX/org.macosforge.xquartz:0

CACA_GEOMETRY=240x68 mpv -vo caca $INPUT
```
## My Output Exhibition
> 因为视频网站码率限制，被视频网站二压后，视频欠码，较模糊

- Evangelion OP 
	- YouTube: <https://youtu.be/JbMo_qHuyRw>
	- Bilibili：<https://www.bilibili.com/video/av30360792>

## Reference
> 以下为本人最初找到的有关此问题的解决方案<br>
> 在此表示感谢
	
- <http://stariocek.asuscomm.com/watch-ascii-libcaca.html>