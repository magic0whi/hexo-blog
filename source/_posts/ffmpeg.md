---
title: ffmpeg
date: 2020-01-31 06:17:40
tags:
---
# ffmpeg常用命令

## 查看文件信息

`$ ffprobe -v quiet -print_format json -show_format -show_streams file.ext`

## 查看文件长度

`$ ffprobe -select_streams v:0 -show_entries stream=duration -of default=noprint_wrappers=1:nokey=1 file.ext`

## 通过VA-API压Hevc

`ffmpeg -hwaccel vaapi -hwaccel_device /dev/dri/renderD128 -vaapi_device /dev/dri/renderD128 -ss 0 -t 4:59 -i output.mkv -vf 'format=nv12,hwupload' -c:v hevc_vaapi -qp 26 -b:v 2000k -r 24 -acodec aac -strict -2 -ac 2 -ab 192k -ar 44100 -f mp4 -y sendready4.mp4`

## 通过Nvidia的cuvid压Hevc

只有meduim | slow | fast 三种present可选

`ffmpeg -hwaccel cuvid -c:v h264_cuvid -i input.mkv -c:v h264_nvenc -preset slow -b:v 2048 -bufsize 4096 -r 30 -profile:v high -level:v 4.1 -acodec aac -strict -2 -ac 2 -ab 192k -ar 44100 -pass 1 -f matroska output.mkv -y`

