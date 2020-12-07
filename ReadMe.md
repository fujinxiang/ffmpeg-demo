
## 水印


``` shell
# 在右下角添加水印
ffmpeg -i source.mp4 -vf "movie=watermark.png[watermark];[in][watermark] overlay=main_w-overlay_w-10:main_h-overlay_h-10[out] " output.mp4

# 在左下角添加水印
ffmpeg -y -i source.mp4 -i watermark.png -filter_complex 'overlay=x=10:y=main_h-overlay_h-10' output.mp4

# 在右上角添加水印
ffmpeg -y -i source.mp4 -i watermark.png -filter_complex 'overlay=x=main_w-overlay_w-10:y=10' output.mp4

# 使用华文行楷字体，64字号，红色字，在右上角写水印文字，main_w-text_w-10 就是视频宽度减文字宽度再减 10
ffmpeg -y -i source.mp4 -vf "drawtext=fontfile=STXINGKA.ttf: text='水印文字':x=main_w-text_w-10:y=10:fontsize=64:fontcolor=red:shadowy=2" output.mp4

# 文字下面使用黄色底框
ffmpeg -y -i source.mp4 -vf "drawtext=fontfile=STXINGKA.ttf: text='水印文字':x=main_w-text_w-10:y=10:fontsize=64:fontcolor=red:box=1:boxcolor=yellow" output.mp4

# 水印透明度 0.5
ffmpeg -y -i source.mp4 -vf "drawtext=fontfile=STXINGKA.ttf: text='水印文字':x=main_w-text_w-10:y=10:fontsize=64:fontcolor=red:alpha=0.5" output.mp4

ffmpeg -y -i source.mp4 -vf "drawtext=fontfile=STXINGKA.ttf: text='水印文字':x=main_w-text_w-10:y=10:fontsize=64:fontcolor=red:enable='lte(t\,1)" output.mp4

# 同时使用两个文字滤镜
ffmpeg -i source.mp4 -vf drawtext=fontcolor=red:fontsize=64:text='hello':x=50:y=50,drawtext=fontcolor=red:fontsize=64:text='world':x=150:y=150 -y output.mp4

# 添加滚动文字的水印
ffmpeg -i source.mp4 -vf drawtext=fontcolor=red:fontsize=64:text='滚动文字':x=50+n*10:y=50 -y output.mp4
``` 

- main_w 代表视频宽度
- overlay_w 代表水印图片宽度
- main_w-overlay_w-10 就是视频宽度减水印图片宽度再减 10
- 
- main_h 代表视频高度
- overlay_h, 代表水印图片高度
- main_h-overlay_h-10 就是视频高度减水印图片高度再减 10


## 音频、图片处理

``` shell
# 将图片设为视频封面，不是添加到视频帧
ffmpeg -i source.mp4 -i poster.png \
-map 0 -map 1 \
-c copy -c:v:1 png \
-disposition:v:1 attached_pic \
-y output.mp4

# 将图片转为视频
ffmpeg -loop 1 -f image2 -i test2.png -vcodec libx264 -r 30 -t 3 test2.mp4

# 合并两个视频
ffmpeg -f concat -i input.txt -c copy -y output.mp4

# 图片和音频合成视频
ffmpeg -loop 1 -y -i poster.png -i trll.mp3 -shortest -acodec copy -vcodec libx264 result2.mp4

# 音频合并
ffmpeg -f concat -i audio.txt -c copy -y output.mp3

# 视频添加音频
ffmpeg -i source.mp4 -i ma.mp3 -filter_complex amix=inputs=2:duration=first:dropout_transition=2 -y output.mp4

# 替换视频中的音频轨 https://stackoverflow.com/questions/11779490/how-to-add-a-new-audio-not-mixing-into-a-video-using-ffmpeg
ffmpeg -i input.mp4 -i trll.mp3 -map 0:v -map 1:a -c:v copy -shortest -y output.mp4

# 添加音频轨到视频，混合音轨
ffmpeg -i input.mp4 -i trll.mp3 -filter_complex "[0:a][1:a]amerge=inputs=2[a]" -map 0:v -map "[a]" -c:v copy -ac 2 -shortest -y output.mp4

```

## 视频处理
``` shell
# 压缩，帧率改为 500k，分辨率改为 854*480
ffmpeg -i puba.mp4 -b:v 500k -s 854*480 pubb.mp4
```
-y 意思是不用询问，直接同名覆盖

[ffmpeg drawtext 滤镜](https://ffmpeg.org/ffmpeg-filters.html#drawtext-1)


## 字幕

``` shell

ffmpeg -i input.mp4 -vf ass=demo.ass -y output.mp4

ffmpeg -i input.mp4 -vf subtitles=demo.srt -y output.mp4

``` 

## 截取视频帧为图片
``` shell

ffmpeg -ss 00:00:01 -i input.mp4 -frames:v 1 -y test.jpg

``` 