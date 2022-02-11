---
title: ffmpeg 常用命令
category: computer
date: 2020-02-08 14:12:31
tags:
toc: true
---

记一些 ffmpeg 的使用

本页面有些命令过长, 可以使用`Shift + 鼠标滚轮`的方式左右滚动查看

<!-- more -->

## 常用操作

* 查看文件信息: `$ ffprobe -v quiet -print_format json -show_format -show_streams output.mkv`
  查看文件长度: `$ ffprobe -select_streams v:0 -show_entries stream=duration -of default=noprint_wrappers=1:nokey=1 output.mkv`
  调整视频分辨率: `-vf scale=854x480`
* VCB-Studio x265 压冻鳗常用参数:
  ```console
  $ <stdout> | x265 --y4m -D 10 --preset slower --deblock -1:-1 --ctu 32 --qg-size 8 --crf 15.0 --pbratio 1.2 --cbqpoffs -2 --crqpoffs -2 --no-sao --me 3 --subme 5 --merange 38 --b-intra --limit-tu 4 --no-amp --ref 4 --weightb --keyint 360 --min-keyint 1 --bframes 6 --aq-mode 1 --aq-strength 0.8 --rd 5 --psy-rd 2.0 --psy-rdoq 1.0 --rdoq-level 2 --no-open-gop --rc-lookahead 80 --scenecut 40 --qcomp 0.65 --no-strong-intra-smoothing --output "output.hevc" -
  ```
* 钱桑の常用参数 x264
  ```console
  ffmpeg -i example.mp4 \
   -c:v libx264 \
   -preset slow \
   -pix_fmt yuv420p \
   -x264-params \
  "crf=28 \
  :threads=4 \
  :deblock=-1,-1 \
  :keyint=600 \
  :min-keyint=1 \
  :bframes=8 \
  :ref=4 \
  :qcomp=0.55 \
  :rc-lookahead=70 \
  :aq-mode=1 \
  :aq-strength=0.8 \
  :me=umh \
  :subme=7 \
  :me_range=16 \
  :psy-rd=1.3,0.15" \
   -c:a copy \
   output.mp4
  ```
* 钱桑の常用参数 x265
  ```console
  ffmpeg -i example.mp4 \
   -c:v libx265 \
   -x265-params \
   "y4m=true \
   :depth=10 \
   :preset=slower \
   :deblock=-1,-1 \
   :ctu=32 \
   :qg-size=8 \
   :crf=28.0 \
   :cbqpoffs=-2 \
   :crqpoffs=-2 \
   :me=3 \
   :subme=3 \
   :merange=20 \
   :limit-tu=4 \
   :no-amp=true \
   :ref=4 \
   :weightb=true \
   :keyint=600 \
   :min-keyint=1 \
   :bframes=6 \
   :aq-mode=1 \
   :aq-strength=0.8 \
   :rd=5 \
   :psy-rd=1.5 \
   :psy-rdoq=1.0 \
   :rdoq-level=1 \
   :rc-lookahead=60 \
   :scenecut=40 \
   :qcomp=0.65" \
    -acodec aac \
    -ac 2 \
    -ab 79k \
    -ar 48000 \
   output.mp4
  ```
   Windows 平台下 ffmpeg 的 libx265 貌似没有 `y4m`、`depth`、`preset` 等参数, 此时可用 ffmpeg 参数 `-pix_fmt yuv420p` (10bit 为 `yuv420p10le`)、`-preset slower` 来解决.
* 通过 VA-API 压 HEVC (相比软压缺少很多参数)
  ```console
  $ ffmpeg -hwaccel vaapi -hwaccel_device /dev/dri/renderD128 -vaapi_device /dev/dri/renderD128 -ss 0 -t 4:59 -i example.mkv -vf 'format=nv12,hwupload' -c:v hevc_vaapi -crf 26 -r 24 -acodec aac -strict -2 -ac 2 -ab 192k -ar 44100 -f mp4 -y output.mp4
  ```
* 通过 nvenc 压 H264 (相比软压缺少很多参数, 只有 meduim, slow, fast 三种 present 可选)
  ```console
  $ ffmpeg -hwaccel cuvid -c:v h264_cuvid -i example.mkv -c:v h264_nvenc -preset slow -b:v 2048 -bufsize 4096 -r 30 -profile:v high -level:v 4.1 -acodec aac -strict -2 -ac 2 -ab 192k -ar 44100 -pass 1 -f matroska output.mkv -y
  ```

## 选项解释

主要参考VCB-Studio的文档<sup id="cite_ref-2"><a href="#cite_note-2">[2]</a></sup><sup id="cite_ref-4"><a href="#cite_note-4">[4]</a></sup>.

* Encoding Mode (编码模式)
  `--qp`&emsp;Quantization Parameter, 量化参数对应 CQ 模式, 默认值为 23.
   CQ 模式 (也称 CQP): Const Quantizer, 固定量化模式. 所有 P 帧采用一个固定的 Quantizer.
   Quantizer, 量化, 是一种衡量图像压缩程度的方法, 用 0-69 的浮点数表示, 0 为无损,
   量化值越大, 图像被压缩的越多, 码率越低.
   注意量化值不一定代表目视质量, 比如说一个纯色的图像可以以很高的量化值被量化,
   占用的体积很小, 而一个很复杂的图像就算量化值不高. 但是压缩后观感也可能很差.
   
   因为 quantizer 不能够较好的体现质量, 所以该模式一般由 Const Quality 模式替代.
   就算不用 CQ 模式, Quantizer 这个概念依旧在编码中存在, 只不过编码器可以智能的浮动 QP 值.
* `-crf`&emsp;CRF 模式: Constant Rate Factor 固定质量因子. x264 默认为 23.0, x265 默认为 28.0.
  x264 用一种结合人心理学估算出来的值, 来衡量视频的目测质量, 即是 RF (Rate Factor), 用浮点数表示, 0 为无损, 越高质量越差.
  CRF 就是在视频前后采用恒定的 RF 从而使视频前后的目测质量几乎一致. CRF 模式下，码率的时间分配效果是最理想的，也是最常用的模式.
  
  x264下, 对于一般视频, CRF 设置在 18-26 左右. 通常采用 19-21.5 就能使得 Rip 看上去很不错. 如需要绝对好的质量可以降低到 16, 但是码率也会很高.
  
  x265下, 日常编码建议 23 以上. 对于动漫的高画质编码, 建议至少 8.0 起.
  
  注意, CRF 在搭配不同的参数前提下, 实际造成的目测效果还是有差距的, 甚至可能很大, 因此不能一概的认为 CRF 代表目视质量.
  一般 vcb-s 用的 10bit 1080p BDRip，CRF 选择在 16~18 之间.
* `--preset`&emsp;Preset 效率预设, x264 会自动设置很多参数来调节你对速度与效率的取舍.
  最快的 ultrafast 速度媲美 GPU 加速, 最慢的 placebo 所需的时间可以是 ultrafast 的近百倍. Preset 越慢，x264 的码率效率越高，意味着单位码率能做到的画质越好. 一般推荐在 slow/slower/veryslow 三档.
  vcb-s 所用的参数一般基于 `--preset "veryslow"`
* `--y4m`&emsp;使用yuv420p颜色空间
* `-D/--depth`&emsp;输出精度, 如输出 10bit: --depth 10
* `--deblock`&emsp;Deblocking 去色块. 现代编码器都是基于 MacroBlock 编码, 把画面划分为一块块的区域,
  有损编码后, 区域之间就可能出现可见的 "分界线". Deblock 的作用就是去除它们, 开启是肯定的.
  deblock 的两个参数分别是 alpha 和 beta 参数, 这两个参数默认是 0:0.
  一般认为合理的范围是-3:-3~3:3 之间. 越是高码率的压片越建议调低; 因为设置高了, deblock 的强度会变大, 对画面有模糊的效果. 通常设置 -1:-1 就行了. 默认 0:0 已经算是比较合理的选择.
  deblock 的强度和 QP 值有关，所以 deblock 设置不变时, QP 值越低 (码率越高), deblock 的强度就会相应降低.
* `--ctu`&emsp;Code Tree Unit, 指定最大允许 CTU 的尺寸.
  虽然 x265 允许 64x64，但是过大的 TU 会增加平面的涂抹，增加运算量, 降低多线程优化可能,
  总体来说在<=1080p 的编码下弊大于利, 因此限制为 32 比较好.
              
  HEVC divides the picture into CTUs (Coding Tree Unit).
  In HEVC standard, if something is called xxxUnit, it indicates a coding logical unit which is in turn encoded into an HEVC bit stream. On the other hand, if something is called xxxBlock, it indicates a portion of video frame buffer where a process is target to.
  
  Coding Tree Unit is therefore a logical unit. It usually consists of three blocks, namely luma (Y) and two chroma samples (Cb and Cr), and associated syntax elements. Each block is called CTB (Coding Tree Block). CTB can be split into CBs<sup id="cite_ref-1"><a href="#cite_note-1">[1]</a></sup>
  
  `--qg-size`&emsp;表示用于调整 QP 值的 CU (Coding Unit) 的最小单位, 越低 x265 调整一帧内 QP 值的灵活度越高. 推荐设置为最小值 8.
  A coding tree unit can be divided into coding units. CU consists of three CBs (Y, Cb, and Cr) and associated syntax elements. <sup id="cite_ref-1"><a href="#cite_note-1">[1]</a></sup>
* `--cbqpoffs`&emsp;x264 会根据 Psy 强度自动调整 Chroma 平面的码率分配, x265 无此机制所以经常出现 Chroma 欠码导致的色彩纹理削弱. 给 -2 左右的 offset 可以较好的缓解问题.
  `--crqpoffs`&emsp;同上, 建议 -2.
* `--no-sao`&emsp;Sample Adatpive Offset, 然而也可称 Smoothing All Objects 可以减少 DCT ringing<sup id="cite_ref-3"><a href="#cite_note-3">[3]</a></sup> 等欠码瑕疵, 但代价是极其暴力的涂抹效果, 所以要禁用它 (除非你是极低码率编码).
* Motion-Estimation (动态分析)
  `--me`&emsp;ME Algorithm 动态搜索算法, 可选:
  Diamond (`--me dia`, x265 为 `--me 0`), 菱形搜索, 不推荐;
  Hex (`--me hex`, x265 为 `--me 1`), 六边形搜索, 默认选项. 只在非常追求速度的时候推荐;
  Multi Hex (`--me umh`, x265 为 `--me 2`), 多重六边形搜索. 非常均衡的算法, 效果非常好, 速度适中. 推荐一般情况下使用;
  Exhaustive (`--me esa`, x265 为 `--me 4`), 全面搜索. 很慢.;
  SATD Exhaustive(`--me tesa`) 经过转换的全面搜索. 速度比 esa 稍微慢一点点, 但是效果好一点, 所以一般有时间浪费的都选 tesa 而不是 esa.
  tesa 比起 umh, 提升在 5%~10%左右. 时间花费上则是 150%~200% 的增加.
  x265 下增加两个模式 (x265没有 tesa):
  Star (`--me 3`), Star is a three-step search adapted from the HM encoder: a star-pattern search followed by an optional radix scan followed by an optional star-search refinement;
  Full (`--me 5`), Full is an exhaustive search; an order of magnitude slower than all other searches but not much better than umh or star.<sup id="cite_ref-5"><a href="#cite_note-5">[5]</a></sup>
  Star 综合来说好于 umh，但是不要试图用 Full。因为 x265 目前还没有对 Full 做优化，太慢了.
  
  `--subme`&emsp;Subpixel Refinement 次像素级别优化.
  从 0~11 效果越来越好, 速度也越来越慢. 追求速度推荐用 7, 追求质量推荐用 10, 最低建议 3 (preset = slow 时默认为 3).
  
  `--merange`&emsp;ME Range, 表示搜索的最大范围. 一般 720p 给 16~24, 1080p 给 24~38. 值越大速度越慢.

  `--me`、`--subme`、`--me_range` 三个选项共同组成 ME 的参数组合.
  ME 是以编码时间换效率的另一套参数 (另一个是`--ref`、`--bframes` 构成的 Frame).
  一般来说, 动态不高的番, 应该是用较强的 Frame 参数, 动态高的番则是用较强的 ME 参数.
  一般来说，对于 x264, 下面三个组合是很好的快中慢三种选择:
  `--me umh --subme 7 --me_range 16`
  `--me umh --subme 10 --me_range 24`
  `--me tesa --sumbe 10 --me_range 24`
* `--no-amp`、`--no-rect`&emsp;amp 和 rect 是 HEVC 规范中对 block 的创新.
  一般来说 block 都是正方形, rect 启用 1x2/2x1 类型的;
  amp 允许 1x4/3x4/4x1/4x3 类型的, 且 amp 是 rect 的充分但不必要条件.
  通常来说，<=1080p 下, 两个都没啥作用 (其中 amp 更没作用), 但都很耗时, 所以建议至少关闭 amp.
  `--limit-tu`&emsp;接上面, 如果 rect 开启, 建议开启 `--limit-tu 4` 来限制 x265 对分块的（可能）无效尝试.
* Reference Frames
  `--ref`&emsp;Number of Reference Frames 参考帧数量.
  表示某一帧最多允许参照多少其他的帧. ref 对编码所需时间的影响是线性的, ref 能带来的收益则是非线性的. 也即越高的 ReF, 额外收益越低. ref 是一个以编码时间换取编码效率的参数. ref 的数量不仅影响编码效率, 而且影响解码效率, 以及解码时最大缓存帧数. ref 和分辨率与帧率相乘后决定了播放设备需要达到的解码能力,
  x264 各个 Level 对其 ref 上限的支持有相应的限制. 对于日常的动漫, 一般来说，ref 设置如下:
  追求速度的编码可以用 ref=4. 速度会很理想;
  日常编码, ref=6, 相对 ref=4 可以节省 2%~4% 左右的体积.
  追求压缩比的编码, ref=8, 相对 ref=4 可以节省 4%~8% 的体积.
  追求极致压缩比的编码, ref 可以选择 13 左右. 相对 ref=4 可以节省 6%~15% 的体积.
  特别静态的场景, 或者特别动态的场景 (比如噪点多的真人电影), ref 开高的意义均不大. 反而是动态程度一般, 场景混杂的, 开高 ref 效果比较好.
  vcb小组实测 ref 增加在 x265 中作用不明显. 建议不超过 6, 建议值为 4.
  
  `--scene-cut` (x264), `--scenecut` (x265)&emsp;Number of Extra I-frames. 
                这是让 x264 决定是否切换场景的灵敏度. x264 会在场景切换的时候插入 I 帧，这个参数越低, x264 越容易判断场景做了切换, 越鼓励插入 I 帧. 通常保持默认 40 就好.
  
  `--keyint`&emsp;Maximum GOP Size, 规定了最远两个 IDR 帧之间的距离.
  这个值越大, 给编码器灵活度越高, 因为编码器不用被迫在不需要的情况下插入 IDR 帧; 但是也意味着拖动进度条的时候压力越大. 特别是 `keyint=inf` (x265 为 `-1`) 的时候, 一些真人访谈场景, 如果不切换场景, 很可能从头到尾只有一个第 0 帧是 IDR.
  一般来说设置在 250~900 之间. 对于在线视频 360~480 是不错的选择. 本地视频 480~720 会比较合适.
  
  `--min-keyint`&emsp;Minimum GOP Size, 规定了最近两个 IDR 帧之间的距离.
  一般编码器在检测到场景转换的时候, 会插入 I 帧. 如果新插入的 I 帧距离上个 IDR 帧的距离 >=min-keyint, 编码器就会将它设置为 IDR 帧. 否则会插入非 IDR 的 i 帧. 一般选择将 min-keyint 设置成 1, 所有 I 帧都会变为 IDR 帧.
                
  关于 min-keyint 的设置有一种说法是考虑到画面中出现反复闪烁时, 设置一定大小的 min-keyint 可以避免插入 I 帧，而是插入 i 帧.
  但实际上, x264 的 scene change 检测是十分智能的, 如果前后的场景可以有互相参考的价值, 那么即便中间有几个突变帧, 也不会贸然地插入 I 帧. 实际上, 这种情况在闪烁的地方插入 P 帧是完全没问题的, 因为即便是 P 帧, 其中的各个 macro-block 依然可以作为 I Block (独立编码), 所以没有使用 i 帧的必要性. (P 帧和 B 帧都可以有 Intra Block, 表现为某个 macroblock 不借助前后帧进行参考.)
  事实上插入 i 帧还会导致兼容性问题, 例如为了 blu-ray compat 就需要设为 1, 所以始终用 `--min-keyint 1` 是最佳的选择.
* Bframes
  `--open-gop`, `--no-open-gop`&emsp;Open GOP. 如果开启, 那么前一个 GOP 的 B 帧将可以参照下一个 GOP 的里面的帧.
  GOP 规定后面的不允许参照前面的, 但是前面的能否参照后面的, 则是由 OpenGOP 决定的. 如果开启, 那么在特定场景下可以增加编码效率, 但是一些播放设备和播放器不支持. 一般来说开启与否问题都不大.
  
  `--weightb`&emsp;Weighted Prediction for B frame 允许 B 帧的加权预测, 在一些渐变场景比较有用. 百利无一害的选项, 默认开启.
  
  `--bframes`&emsp;Number of B-Frames, 最大允许的连续 B 帧数量. 这个值越大, 编码时间稍有提高, 对压缩率也有帮助. B 帧并不是越高越好, 一般认为真人电影设置为 3~8, 动漫设置为 6~12 左右比较合适.
  
  `--b-adapt`&emsp;Adaptive B-Frames, 采用什么算法来决定是否采用 B 帧.
  对于 x264, 0-off 表示永远能用就用, 1-fast 是快速算法, 2-normal 采用的是常规算法. 2 比较优秀, 也会较慢. 一般推荐 `--preset slow` 及以上时候使用 2.
  x265 下有似乎有不同的实现<sup id="cite_ref-5"><a href="#cite_note-5">[5]</a></sup>:
  With b-adapt 0, the GOP structure is fixed based on the values of --keyint and --bframes.
  With b-adapt 1 a light lookahead is used to choose B frame placement.
  With b-adapt 2 (trellis) a viterbi B path selection is performed. (Default)
  
  `--b-pyramid`&emsp;B-Pyramid, B 帧参照其他 B 帧的方式. Disable 表示不允许 B 帧参照其他的 B 帧, Strict 为了 BD 原盘播放兼容性而设置, normal 则不加限制. 一般选择 normal 以最大化编码效率.
  x265 下只有启用和禁用.
* Rate Control
  `--aq-mode`&emsp;Adaptive Quantizers, 简称 AQ. 没有 AQ, x264 会倾向于在平面和纹理处降低码率. 造成的效果就是线条部分看上去还行, 但是平面大幅度 block, 纹理烂掉. AQ 的作用就是来防止码率在纹理和平面处被过分的降低.
  AQ Mode, 选择 AQ 的算法. x264 有 Disable (aq-mode=0), Variance AQ (aq-mode=1) 和 Auto-Variance AQ(aq-mode=2). 一般来说, mode=1 效果中等, 比较安全, 不容易出现较烂的帧. mode=2 比较省码率, 但是偶尔容易出现烂帧.
  x265 有三种 aq 模式. aq-mode 1 是最安全稳定的 AQ, 适合高码率/高画质编码; aq-mode=2 相对来说效率最高, 适合中低码率的编码; aq-mode=3 对暗场进行加强, 适合 8bit 编码防止暗场压烂. 一般 10bit 编码根据 crf 高低决定 AQ 选取, 个人建议在 crf<=16 时候使用 aq-mode=1, 否则使用 aq-mode 2. 注意同 crf 下, 不同 aq-mode 出来的体积是不一样的, 3>1>2.

  `--aq-strength`&emsp;AQ Strength, AQ 强度.
  对于 x264, 动漫选 0.6-1.0 左右; 真人选 0.8-1.2 左右. 越是高质量的编码, AQ 的 Strength 应该越高.
  对于 x265, 动漫的 aq-strength 不用太高 (太高了码率也会浪费). 通常 aq-mode=1, aq-strength 给 0.8 比较合理; aq-mode=2, aq-strength 给 0.9 左右, aq-mode=3, aq-strength 给 0.7 左右.

  `--qcomp 0.65`&emsp;Quantizer Compression, 这个参数决定了 QP 的时域变化灵活度.
  越低的数值代表灵活度越高, QP 值变动越大, 效果就是高动态场景下烂的比较严重, 因为 x264 会倾向于提高高动态下的 QP 值 (特别是引入 mbtree 之后).
  通常认为, 越是倾向于高画质编码的, qcomp 需要给的越高, 反之亦然. qcomp=0 的时候效果接近固定比特率, qcomp=1 的时候效果接近固定 qp 值. qcomp 的作用会受到 mbtree 的影响.
  建议设置为略高于默认 0.6 的 0.65, 对时域分配采取略保守的策略，来针对中高画质优化.

  `--mbtree`, `--no-mbtree`&emsp;MB-tree (Marco-Block Tree) 是 x264 后期引入的一种码率控制和决策的工具, 在时域和空域都有重要的影响.
  MB-tree 的原理简单点说, 就是在编码过程中, 被大量参照的 block (被前后帧参照, 或者被同一帧其他部分参照) 给的 QP 值降低, 画质更好, 体积更大; 反之, 被参照少的帧 QP 增加, 画质更烂, 体积更小. 其逻辑在于, 被参照多的 block 理应有更好的画质, 这可以让参照它的更多 block 受益. MB-tree 的弊端主要在通常将平面和纹理涂抹的较为过分 (这些 block 通常是直接参照别的 block), MB-tree 也倾向于降低高动态部分的质量. 如果关闭 MB-tree, 通常细节会更好, 但是线条会有些欠码.
                           
  普遍认为，在中低码率的编码中 (crf>18), MB-tree 永远是利大于弊的. 在极高码率的编码中 (crf<16), MB-tree 永远是弊大于利的. 在高码率 (crf=16-18), 越是动态多, 噪点多, 线条锐利的片子, MB-tree 的正面作用越强.

  开启 MB-tree 体积会减少很多. 所以一般开 MB-tree, crf 可以相比于关 MB-tree 低 1.0 左右. 比如 crf=16.5 mbtree=1、crf=17.5 mbtree=0 是 vcb-s 常用的选择.
                     
  MB-tree 开启的时候, qcomp 的灵活度会被放大. 所以一般开 MB-tree 需要增加 qcomp (qcomp 数值增加, 灵活度降低).
  比如 vcb-s 常用的参数:
  crf=16.0 mbtree=1 qcomp=0.80, crf=17.0 mbtree=0 qcomp=0.70;
  crf=16.5 mbtree=1 qcomp=0.75, crf=17.5 mbtree=0 qcomp=0.60;
  crf=17.0 mbtree=1 qcomp=0.75, crf=18.0 mbtree=0 qcomp=0.60;
  crf=19.0 mbtree=1 qcomp=0.70 (crf=19 的时候不宜关闭 MB-tree)

  `--pbratio` 降低 P 帧和 B 帧间画质差距. 动漫编码 B 帧数量庞大, 且 P、B 之间分工不明显, 因此降低这个参数对全局有利.
* Psycho-visual
  `--psy-rd`&emsp;Psy 相关是一种 x264 引入的主观优化: 在欠码的时候, 人眼宁愿看到失真, 也不愿看到大范围的模糊. 虽然这种失真对客观的还原度来说不利, 但是它有利于保留画面纹理, 编码前后的图像看上去违和感较低, 细节锐度较好.
  在 x264 中, 它有两个参数, 第一个是Psy-rd Strength, 心理学优化强度. 
  一般压制动漫选择 0.4-1.0，压制真人选择 0.7-1.3.越是高质量的编码, 可以开的越高. 但是在 CRF 模式下, 开高了 psy-rd 也会提高码率.
  第二个参数是 Psy Trellis, 在 psy-rd 的基础上, 微调保留的噪点之类的微小细节的保留度.
  一般开 MB-tree 时候, psy-trellis 可以给 0.1 左右. 关闭 MB-tree 时建议关闭.psy-trellis 的值越高, 会相应的调低 `--chroma-qp-offset` 以求让 Chroma 平面的细节得以保留. 所以在关闭 MB-tree 的时候, 一方面将 `--psy-trellis` 设置为 0，一方面将 `--chroma-qp-offset` 往下调 1 左右.
  对于 x264 推荐设置为 `0.6:0.15`.
  
  对于 x265, Psy 是目前调节锐利度和细节保留的重要工具, 低了会糊高了会出现动态瑕疵. 默认的 2.0 其实是个不错的数值. 如果中低码率编码，可以考虑降低到 1.5 左右.
  
  `--psy-rdoq` (x265)&emsp;作用类似 x264 中的 `--psy-trellis`, 默认 0.0, 设置为 1.0 有助于保留细节和噪点.
* Mode decision / Analysis
  `--rd`&emsp;Rate distortion optimization 码率失真优化, 值越高计算度越复杂. 3 是一个比较平衡的选择. 目前 5 是实际最高选择. 根据官方 doc, `--rd 3` 和 `--rd 4` 相同, `--rd 5` 和 `--rd 6` 相同.

  `--rdoq-level`&emsp;Specify the amount of rate-distortion analysis to use within quantization:<sup id="cite_ref-5"><a href="#cite_note-5">[5]</a></sup>
  At level 0 rate-distortion cost is not considered in quant.
  At level 1 rate-distortion cost is used to find optimal rounding values for each level (and allows psy-rdoq to be effective). It trades-off the signaling cost of the coefficient vs its post-inverse quant distortion from the pre-quant coefficient. When --psy-rdoq is enabled, this formula is biased in favor of more energy in the residual (larger coefficient absolute levels).
  At level 2 rate-distortion cost is used to make decimate decisions on each 4x4 coding group, including the cost of signaling the group within the group bitmap. If the total distortion of not signaling the entire coding group is less than the rate cost, the block is decimated. Next, it applies rate-distortion cost analysis to the last non-zero coefficient, which can result in many (or all) of the coding groups being decimated. Psy-rdoq is less effective at preserving energy when RDOQ is at level 2, since it only has influence over the level distortion costs.
  注意默认的 `--preset medium` 下它是 0, 这时候 rdoq 是没有用的. slow 及以上自动开启.
  
  `--b-intra` 允许 B 帧中出现 Intra Block. 动画建议.
* `--rc-lookahead`  编码时候往前看多少帧来规划 Coding Unit Tree (CUTree, 相当于 MBTree), 一般设置为 60-80 比较合理; 帧率越高的片源适合给的越高.
* `--strong-intra-smoothing`, `--no-strong-intra-smoothing`  
                Enable strong intra smoothing for 32x32 intra blocks. This flag performs bi-linear interpolation of the corner reference samples for a strong smoothing effect. The purpose is to prevent blocking or banding artifacts in regions with few/zero AC coefficients. Default enabled
`--output`  输出文件名



## 一些姿势

1. IPB 相关知识
   IBP 帧: 视频压缩的过程中, 对于一段时间内相似的帧, 采用记录第一张, 后续几张只记录和第一张的区别, 这种想法是很自然的. 由此引申出两种帧:
   I 帧 (Independent Frame), 独立编码的帧. I 帧相当于记录一张 jpg, 一般常见于一个场景开头. 后续的帧就需要依赖 I 帧.
   P 帧(Predictive Frame), 需要依赖其他帧来编码的类型. P 帧需要参照在它之前的 I 帧或者其他的 P 帧, 因为它只记录差别, 所以对于那种前后差别很小甚至没有的帧, 使用 P 帧编码能极大地减少体积.
   MPEG2 之后，引入了第三种帧:
   B 帧 (Bi-directional Frame), 双向预测帧. B 帧跟 P 帧类似, 都是需要参照别的帧, 区别在于 B 帧需要参照它后面的帧, B 帧的记录和预测方式也做了调整, 使得 B 帧的预测方式更灵活, 对付高度静态规律的场景可以更有效的降低体积.
   一种典型的排列方式: IPBBPBPIPPB...
   视频开头一定是一个 I 帧.
2. IDR 帧与 GOP 区间
   I 帧中, 有一类特殊的 I 帧, 叫做 IDR 帧. IDR 帧的性质是, 比如第 1000 帧是 IDR 帧, 那么这一帧相当于一个分水岭, 从 1001 帧开始, 所有的帧都不能再参照 1000 帧之前的帧. 在 closed GOP 规定下 (x264 设置中可以指定, 并且 vcb-s 一直指定), 0~999 帧也不允许参照这个 IDR 帧以及之后的帧. 等于说 IDR 帧将视频分割成两个独立的部分: 前面的不能参照后面的(closed GOP 规定下), 后面的不能参照前面的.
   这个性质在播放的时候额外有用: 如果我直接从第 1000 帧开始播放, 我可以毫无问题的播放下去, 因为我不需要参照 1000 帧之前的内容完成解码. 我从开头播放, 直到 999 帧的时候, 我都不需要参照 1000 帧及它后面的东西; 1000 帧之后的数据都损坏了, 0~999 帧也能正常播放.
   IDR 的全称叫做 Instantaneous Decoder Refresh, 意思是，解码到当前帧, 解码器就可以把缓存全清了, 之前的所有帧信息都没用了, 后续帧不会再去参照它们.
   视频开头的 I 帧一定是 IDR 帧.
   有时候, 我们用 I 帧表示 IDR 帧, i 帧表示非 IDR 的 I 帧. i 帧和 IDR 帧都只进行 intra prediction (所有参照信息都是帧内, 不借助前后帧). 区别在于 i 帧的前后 P、B 帧可以互相参考, 而 I 帧不允许.

   从一个 IDR 帧开始, 到下一个 IDR 前的帧结束, 叫做 IDR 区间, 又叫做 GOP 区间. closed GOP 设定下, GOP 区间可以看做是独立的一段视频: 它里面的所有帧, 都不需要参照任何区间之外的东西, 只要一个 GOP 区间是齐全的, 区间里面所有的帧都能被解码. 我们平时看的视频就是多段 GOP 区间连接起来的.
   在我们拖动进度条的时候，为啥有时候会卡顿，或者拖不准呢？其实是播放器干了这些事：
   1. 计算你拖动进度条的时间, 找出它应该是哪一帧, 假设为 N;
   2. 通过搜索视频索引信息, 找出 N 帧前面最近的一个 IDR 帧, 假设为 M (M<=N). 很显然, M 和 N 同属于一个 GOP 区间, 这个区间开头的帧是 M;
   3. 如果你的播放器设置了快速索引, 视频将直接从 M 帧开始播放, 因为 M 帧是 IDR, 它不需要参照任何帧, 所以立刻可以开始播放. 这是为啥你会发现开始的地方总是在之前一点;
   4. 否则, 如果你的播放器设置了精确索引, 解码器会从 M 帧一直开始解码, 解码到 N 帧, 然后显示 N 帧的画面, 并继续播放.
   
   当 N-M 比较大的时候, 播放器可能需要先解码几百甚至上千帧, 才能继续播放.如果视频允许很长的 GOP 区间, 这个视频在播放的时候, 拖动进度条就特容易卡顿.
   如果设置了 `--min-keyint 1`, 那么所有 I 帧都是 IDR 帧 (vcb-s 也一直在用). 而 GOP 区间最大的范围则通过 `--keyint` 指定.

## 参考文献

<ol>
<li id="cite_note-1"><b><a href="#cite_ref-1">^</a></b> <a href="https://codesequoia.wordpress.com/2012/10/28/hevc-ctu-cu-ctb-cb-pb-and-tb/">"HEVC – What are CTU, CU, CTB, CB, PB, and TB?"</a>. <i>codesequoia.wordpress.com</i>. Retrieved 2022-2-10.
</li>
<li id="cite_note-2"><b><a href="#cite_ref-2">^</a></b> <a href="https://vcb-s.nmm-hd.org/Dark%20Shrine/%5BVCB-Studio%5D%5B%E6%95%99%E7%A8%8B10%5Dx265%202.9%E5%8F%82%E6%95%B0%E8%AE%BE%E7%BD%AE/%5BVCB-Studio%5D%5B%E6%95%99%E7%A8%8B10%5Dx265%202.9%E5%8F%82%E6%95%B0%E8%AE%BE%E7%BD%AE.pdf">"VCB-Studio 教程 10 x265 v2.9 参数设置"</a>. <i>vcb-s.nmm-hd.org</i>. Retrieved 2022-2-10.
</li>
<li id="cite_note-3"><b><a href="#cite_ref-3">^</a></b> Yu Yuan, David Feng, and Yu-Zhuo Zhong. <a href="https://publications.waset.org/3309/pdf">"The Causation and Solution of Ringing Effect in DCT-based Video Coding"</a>. <i>World Academy of Science, Engineering and Technology, International Journal of Computer, Electrical, Automation, Control and Information Engineering</i>, vol. 2, no. 1, pp. 231-236, 2008
</li>
<li id="cite_note-4"><b><a href="#cite_ref-4">^</a></b> <a href="https://vcb-s.nmm-hd.org/Dark%20Shrine/%5BVCB-Studio%5D%5B%E6%95%99%E7%A8%8B09%5Dx264%E5%8F%82%E6%95%B0%E8%AE%BE%E7%BD%AE/%5BVCB-Studio%5D%5B%E6%95%99%E7%A8%8B09%5Dx264%E5%8F%82%E6%95%B0%E8%AE%BE%E7%BD%AE.pdf">"VCB-Studio 教程 09 x264 参数设置"</a>. <i>vcb-s.nmm-hd.org</i>. Retrieved 2022-2-10.
</li>
<li id="cite_note-5"><b><a href="#cite_ref-5">^</a></b> <a href="https://x265.readthedocs.io/en/stable/cli.html">"Command Line Options - x265 documentation"</a>. <i>x265.readthedocs.io</i>. Retrieved 2022-2-10.

</ol>