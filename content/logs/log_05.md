---
date: 2018-12-20T15:54:00+08:00
description: ""
featured_image: ""
tags: []
title: "Converting Video to ASCII via LibCACA"
featured_image: '/images/log_05/voCACA_00.jpg'
---

## Method 1: Screen Recording
在 CPU 性能足够的情况下，可以通过录制屏幕的方式来获得 LibCACA 输出的 ASCII 视频

### 1. List Devices
列举出 AVFoundation 的输入设备

```
ffmpeg -f avfoundation -list_devices true -i ""
```

### 2. Rec Screen
在第一步的基础上，使用 FFmpeg 选择需要的输入设备进行录制

```
# e.g.
ffmpeg -y -f avfoundation \
-i 1:3 -framerate 30 \
-c:v libx264 -r 30 -pix_fmt uyvy422 \
-preset slow -crf 19 \
-c:a aac -b:a 192k \
"$INPUT $(date "+%Y-%m-%d %H-%M-%S").mp4"
```


### * Rec Terminal
另一种办法是录制终端字符，在此使用现成的终端录制项目:

- asciinema: <https://asciinema.org/>
- ScreenShooter: <https://github.com/Jamesits/ScreenShooter.git>

## Method 2: Format Converting
> Process: Video --> JPG --> HTML --> JPG --> Video
 
此方法主要针对 CPU 性能不足的情况（会在录制时掉帧），通过对视频内容的逐帧转换来完成

### 0. Requirement
- FFmpeg
- img2txt
- webkit2png

### 1. Video to JPG via `ffmpeg`
通过 FFmpeg 完成对视频的逐帧截取，截图格式为 JPG

```
ffmpeg -threads $thread_amount \
-i "$RAW_INPUT" \
-r $video_fps -s $video_resolution -f image2 \
%07d.jpg
```

### 2. JPG to HTML via `img2txt`
通过 img2txt 逐帧完成视频的 ASCII 风格转换，存为 HTML

```
for ascii in *.jpg; do
	img2txt -W 128 -H 36 -x 3 -y 5 $ascii -f html > $(basename $ascii .jpg).html;
done
```

- "-W 128" 表示 128 列 ASCII 字符
- "-H 36" 表示 36 行 ASCII 字符
- "-x 3 -y 5" 表示每个字符使用 3x5 像素


### 3. HTML to PNG via `webkit2png`
通过 webkit2png 完成对 HTML 到 PNG 的逐帧转换

```
for LibCaca in *.html; do
	webkit2png -F -o $(basename $LibCaca .html) $LibCaca;
done
```

### 4. PNG to Video via `ffmpeg`
最后再次使用 FFmpeg，完成对图片的重新封装，导出成视频

> 有关 FFmpeg 的命令:<br>
> -vf crop 可参考: <https://ffmpeg.org/ffmpeg-all.html#crop><br>
> -vf scale 可参考: <https://ffmpeg.org/ffmpeg-all.html#scale> 或 <https://trac.ffmpeg.org/wiki/Scaling><br>
> -vf pad 可参考: <https://ffmpeg.org/ffmpeg-all.html#pad-1><br>


```
# Packup the images and encapsulate
# Crop 为裁剪命令，
# 例子中为裁剪分辨率为 2458*1296，从视频左上角 (x,y)=(16,16) 处开始裁剪
ffmpeg -threads $thread_amount -start_number 1 \
-i %07d-full.png \
-i "$RAW_INPUT" \
-movflags +faststart \
-c:v libx264 -pix_fmt yuv420p -crf 20 -r $video_fps \
-preset slower -profile:v high -level 5.0 \
-vf "crop=2458:1296:16:16" \
-map 0:v:0 -map 1:a:0 output_yuv420.mp4

# Packup 和 Scale&Pad 步骤可写为一个步骤，避免二次编码，此处为方便展示 -vf 分开写
# Scale and Pad
# Scale 此处写为按比例缩放到 1080P，在不满足目标分辨率的情况下，降低分辨率
# Pad 为视频填充黑色底（可不添加，视频分辨率即为 1920*width，width < 1080）
INPUT=output_yuv420.mp4
OUTPUT=output_scaled.mp4

ffmpeg -threads $thread_amount \
-i $INPUT -c:a copy \
-vf "scale=1920:1080:force_original_aspect_ratio=decrease, pad=1920:1080:0:(1080-in_h)/2:black" \
$OUTPUT
```

### Automation Script
将上述步骤写成 Shell Script，并提供了几种输出模式 

Shell Script：<https://github.com/ItoSchum/video_output_CACA>

## Other
### 1. Screenshot Capturing
以下命令可用于截取视频中的一帧用于视频封面等

```
ffmpeg -ss 00:04:03 -i $INPUT -vframes 1 -f mjpeg sample.jpg
```

### 2. `DISPLAY` Setting
在使用如`mpv --vo=caca` 命令时，有关在 XQuartz 的窗口或是在终端内输出视频的偏好设置

- 如果希望使用 X11 作为输出：

```
echo $DISPLAY
DISPLAY=/private/tmp/com.apple.launchd.RTIBTlQGVu/org.macosforge.xquartz:0
```

- 输出到 X11 时，也可以设定输出的字符矩阵大小：

```
# e.g. 显示 240 行 * 68 列字符
export CACA_GEOMETRY=240x68
mpv -vo caca $INPUT
```

- 如果希望直接在终端内输出：

```
export DISPLAY=
```

## Demo

- Evangelion OP 
	- YouTube: <https://www.youtube.com/watch?v=6QspMamFR9U&list=PLOWxt7icmzKwRIqAnzFNOwedE3PVvKS7B>
	- Bilibili：<https://www.bilibili.com/video/av40204551>

## Reference
以下为本人最初找到的有关此问题的解决方案，在此表示感谢

- <http://stariocek.asuscomm.com/watch-ascii-libcaca.html>