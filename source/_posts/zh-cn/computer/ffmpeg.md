---
title: ffmpeg 常用命令
category: computer
date: 2020-02-08 14:12:31
tags:
---

## 查看文件信息

`$ ffprobe -v quiet -print_format json -show_format -show_streams output.mkv`

## 查看文件长度

`$ ffprobe -select_streams v:0 -show_entries stream=duration -of default=noprint_wrappers=1:nokey=1 output.mkv`

## 将动漫压成HEVC格式

```
$ <输入流> | x265 --y4m -D 10 --preset slower --deblock -1:-1 --ctu 32 --qg-size 8 --crf 15.0 --pbratio 1.2 --cbqpoffs -2 --crqpoffs -2 --no-sao --me 3 --subme 5 --merange 38 --b-intra --limit-tu 4 --no-amp --ref 4 --weightb --keyint 360 --min-keyint 1 --bframes 6 --aq-mode 1 --aq-strength 0.8 --rd 5 --psy-rd 2.0 --psy-rdoq 1.0 --rdoq-level 2 --no-open-gop --rc-lookahead 80 --scenecut 40 --qcomp 0.65 --no-strong-intra-smoothing --output "EP01.hevc" -
```

> 受 VCB-Studio 启发

## Windows 10 下我压游戏录像的ffmpeg参数

```
C:/> ffmpeg.exe -i input.mp4 -c:v libx265 -x265-params "y4m:depth=10:preset=slower:deblock=-1:-1:ctu=32:qg-size=8:crf=15.0:cbqpoffs=-2:crqpoffs=-2:no-sao:me=3:subme=5:merange=38:limit-tu=4:no-amp:ref=4:weightb:keyint=360:min-keyint=1:bframes=6:aq-mode=1:aq-strength=0.8:rd=5:psy-rd=2.0:psy-rdoq=1.0:rdoq-level=2:no-open-gop:rc-lookahead=80:scenecut=40:qcomp=0.65:no-strong-intra-smoothing=true" -acodec aac -strict -2 -ac 2 -ab 192k -ar 44100 output.mkv
```

> 受 VCB-Studio 启发

## 通过VA-API压Hevc (缺少很多参数设置)

`ffmpeg -hwaccel vaapi -hwaccel_device /dev/dri/renderD128 -vaapi_device /dev/dri/renderD128 -ss 0 -t 4:59 -i output.mkv -vf 'format=nv12,hwupload' -c:v hevc_vaapi -crf 26 -r 24 -acodec aac -strict -2 -ac 2 -ab 192k -ar 44100 -f mp4 -y sendready4.mp4`

## 通过Nvidia的nvenc压HEVC (缺少很多参数设置)

只有meduim | slow | fast 三种present可选

`ffmpeg -hwaccel cuvid -c:v h264_cuvid -i input.mkv -c:v h264_nvenc -preset slow -b:v 2048 -bufsize 4096 -r 30 -profile:v high -level:v 4.1 -acodec aac -strict -2 -ac 2 -ab 192k -ar 44100 -pass 1 -f matroska output.mkv -y`

## 选项解释

```
-qp Quantization Parameter, 使用这个时-b:v等限制固定比特率和文件大小的选项会无效, 建议不要用于流式传输
-crf 即Constant Rate Factor, 同"-cq", 使用这个时-b:v等限制固定比特率和文件大小的选项会无效, 建议不要用于流式传输
-qp与-crf区别在于-crf允许在运动较多的帧中gp增加，而在静止帧中下降，从而在保留压缩效率的同时，视频画质一样。
所以建议使用-crf

-global_quality 50 设置全局质量, 越低质量越好
```

### 来自 vcb-studio 的x265参数解释

```
--y4m  使用yuv420p颜色空间

-D 10/--depth 10  输出精度10bit

--preset slower

--deblock -1:-1  去色块-1:-1足够

--ctu 32  transform unit, 大概就是 !@#?**&@

--qg-size 8  表示 qp 值调整的最低单位是 8x8 的 coding unit, 越低x265 调整一帧内 qp 值的灵活度越高

--crf 15.0  -crf 依旧是调节体积/画质最有效，最直接的参数。默认的 28.0 大概是为了强调 x265 在低码率的优势。日常编码怎么着 23 以上吧。对于动漫的高画质编码，建议至少 8.0 起。

--cbqpoffs -2  x264会根据psy强度自动调整 chroma 平面的码率分配, x265无此机制所以经常出现 chroma 欠码导致的色彩纹理削弱。给-2 左右的 offset 可以较好的缓解问题

--crqpoffs -2  同上

--no-sao  VCB认为是“Smoothing All Objects”, 可以减少 !@#$!? 导致的欠码瑕疵, 但代价是极其暴力的涂抹效果, 所以要禁用它(除非你是极低码率编码)

--me 3  Motion-Estimation(动态分析), 3表示使用 star search。具体见vcb的x264参数文档

--subme 5  Subpixel Refinement(次像素级别优化), 0~11越大越好, 最低建议3 (preset = slow 时默认为3)

--merange 38  ME Range, 表示搜索的最大范围. 1080p 给 38 左右就绰绰有余

--no-amp/--no-rect  amp 和 rect 是 HEVC 规范中对 block 的创新. 一般来说 block 都是正方形, rect 启用 1x2/2x1 类型的; amp 允许 1x4/3x4/4x1/4x3 类型的, 且 amp 是 rect 的充分但不必要条件. 通常来说，<=1080p 下, 两个都没啥作用(其中amp更没作用), 但都很耗时, 所以建议至少关闭 amp.

--limit-tu 4  接上面, 如果 rect 开启, 建议开启--limit-tu 4 来限制 x265 对分块的（可能）无效尝试。

--ref 4  Number of Reference Frames(参考帧数量), 参考vcb的x264参数文档, vcb小组实测 ref 增加在 x265 中作用不明显。建议不超过 6

--weightb 允许 b 帧的加权预测，在一些渐变场景比较有用。

--keyint 360 规定了最远两个 IDR 帧之间的距离, 具体见vcb的x264参数文档中[IPB 相关的知识]

--min-keyint 1 规定了最近两个 IDR 帧之间的距离, 这里将他设置为最小, 以确保所有IDR帧都是I帧而不是允许P/B帧前后参照的i帧

--bframes 6 b 帧并不是越高越好，建议给 6~10 左右

--aq-mode 1 x265 目前有三种 aq 模式。aq-mode 1 是最安全稳定的 aq，适合高码率/高画质编码；aq-mode 2 相对来说效率最高，适合中低码率的编码；aq-mode 3 对暗场进行加强，适合 8bit 编码防止暗场压烂。一般 10bit编码根据 crf 高低决定 aq 选取，个人建议在 crf <= 16 时候使用 aq-mode 1，否则使用 aq-mode 2。注意同 crf 下，不同 aq-mode 出来的体积是不一样的，3>1>2。可对比下面快速索引中的描述

--aq-strength 0.8  aq 的强度，一般来说，动漫的 aq-strength 不用太高（太高了码率也会浪费）。通常，aq-mode=1，aq-strength 给 0.8 比较合理；aq-mode=2，aq-strength 给 0.9 左右，aq-mode=3，aq-strength 给 0.7 左右. 可对比下面快速索引中的描述

--rd 5  Rate distortion optimization(码率失真优化)的模式, 越高, 计算度越复杂。3 是一个比较平衡的选择。目前 5 是实际最高选择。根据官方 doc，--rd 3 和--rd 4 相同，--rd 5 和--rd 6 相同。

--psy-rd 2.0  psy 是目前 x265 调节锐利度和细节保留的重要工具，低了会糊高了会出现动态瑕疵。默认的 2.0 其实是个不错的数值。如果中低码率编码，可以考虑降低到 1.5 左右。

--psy-rdoq 1.0  作用类似 x264 种的 psy-trellis，开一点有助于保留细节和噪点。

--rdoq-level 2  注意默认的--preset medium 下它是 0，这时候 rdoq 是没有用的。slow 及以上自动开启。(不太清楚是指定了依旧会置为0还是默认下是0所以没有效果)

--no-open-gop  关闭 OpenGOP，屏蔽一些设备上不能正确解码 opengop 的问题。

--rc-lookahead 80  编码时候往前看多少帧来规划 Coding Unit Tree（CUTree，相当于 MBTree）一般设置为 60~80 比较合理；帧率越高的片源适合给的越高。

--scenecut 40  文档上没有解释

--qcomp 0.65  Quantizer Compression, 略高于默认的 0.6，对时域分配采取略保守的策略，来针对中高画质优化. 详情见下面快速索引

--no-strong-intra-smoothing  文档上没有解释, 减少涂抹感? (请注意用在ffmpeg上得这么写no-strong-intra-smoothing=true)

--output "EP01.hevc"  输出文件名
```

### Animation only

--pbratio 1.2 降低 p 帧和 b 帧间画质差距。动漫编码 b 帧数量庞大，且 pb 之间分工不明显，因此降低这个参数对全局有利。

--b-intra 允许 B 帧中出现 Intra Block。动画建议

## 快速索引

### AQ

Adaptive Quantizers，简称 AQ。没有 AQ，x264 会倾向于在平面和纹理处降低码率。造成的效果就是线条部分看上去还行，但是平面大幅度 block，纹理烂掉。AQ 的作用就是来防止码率在纹理和平面处被过分的降低。

AQ Mode: 选择 AQ 的算法。Disable(aq-mode=0,不用 aq)，Variance AQ(aq-mode=1)和 Auto-Variance AQ(aq-mode=2)。tMod 版 x264 还加了 mode=3/4/5，只不过在 megui 中需要用自定义命令行来启用。

一般来说，mode=3 最好，最适合动漫，但是码率稍高；mode=1 效果中等，比较安全，不容易出现教烂的帧。mode=2 比较省码率，但是偶尔容易出现烂帧。mode=4 是由 2 优化而来，更保险一点。

一般来说，日常压动漫就选 3，压真人特典选 1 或者 4

Strength，就是 aq 的强度。动漫选 0.6~1.0 左右，真人选 0.8~1.2 左右。越是高质量的编码，aq 的 strength 应该越高。

### MBTree

MB-tree(Marco-Block Tree)是 x264 后期引入的一种码率控制和决策的工具，在时域和空域都有重要的影响。
mbtree 的原理简单点说，就是在编码过程中，被大量参照的 block（被前后帧参照，或者被同一帧其他部分参照），给的 qp 值降低，画质更好，体积更大；反之，被参照少的帧，qp 增加，画质更烂，体积更小。其逻辑在于，被参照多的 block 理应有更好的画质，这可以让参照它的更多 block 受益。mbtree 的弊端主要在通常将平面和纹理涂抹的较为过分（这些block通常是直接参照别的block），mbtree也倾向于降低高动态部分的质量。如果关闭mbtree，通常细节会更好，但是线条会有些欠码。

### Quantizer Compression

--qcomp，这个参数决定了 qp 的时域变化灵活度。越低的数值代表灵活度越高，qp
值变动越大，效果就是高动态场景下烂的比较严重，因为 x264 会倾向于提高高动态下的 qp 值（特别是引入 mbtree
之后）。通常认为，越是倾向于高画质编码的，--qcomp 需要给的越高，反之亦然。qcomp=0 的时候效果接近固
定比特率，qcomp=1 的时候效果接近固定 qp 值。qcomp 的作用会受到 mbtree 的影响。这个我们下文细谈。

## 引用

[从vcb-studio了解魔法参数(x265)](https://vcb-s.nmm-hd.org/Dark%20Shrine/%5BVCB-Studio%5D%5B%E6%95%99%E7%A8%8B10%5Dx265%202.9%E5%8F%82%E6%95%B0%E8%AE%BE%E7%BD%AE/%5BVCB-Studio%5D%5B%E6%95%99%E7%A8%8B10%5Dx265%202.9%E5%8F%82%E6%95%B0%E8%AE%BE%E7%BD%AE.pdf)