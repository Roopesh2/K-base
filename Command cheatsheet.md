---
title: Command cheetsheet
documentclass: scrartcl
fontsize: 14pt
geometry: "left=2cm,right=2cm,top=1cm,bottom=1cm"
header-includes:
    - \usepackage{titlesec}
    - \titleformat*{\section}{\itshape}
---
# ImageMagic

## &nbsp;&nbsp; Scale to a dimension:
> `convert i.png -resize 100x100 o.png`

## &nbsp;&nbsp; Extend image to another dimension
> `convert original.png -resize 100x100^ -gravity center -extent 100x100 new.png`

## &nbsp;&nbsp; Resizing files in place
> `mogrify -resize 100x100 original.png`
> 
> `convert i.jpg -resize 75% o.jpg`

## &nbsp;&nbsp; Batch convertion to jpg
> `mogrify -format jpg *.png`

## &nbsp;&nbsp; Append images
> veritcal: `convert -append in-*.jpg out.jpg`

> horizontal: `convert +append in-*.jpg out.jpg`

## &nbsp;&nbsp; Subtract A from B
> `convert B.jpg A.jpg -compose minus -composite  o.png`

## &nbsp;&nbsp; Rotate image by a$^\circ$
> `convert i.jpg -rotate a o.jpg`


# Youtube-dl

## &nbsp;&nbsp; Extract audio only
> `youtube-dl -x --audio-format mp3 link`


# Instaloader

## &nbsp;&nbsp; Download a post
> `instaloader -- -shcode`


# FFmpeg

## &nbsp;&nbsp; Convert to stable mp3
> `ffmpeg -i i.mp3 -vn -ar 44100 -ac 2 -b:a 192k o.mp3`

## &nbsp;&nbsp; Concat multiple audios
> `ffmpeg -f concat -i list.txt -c copy o.mp3`

> list.txt: 
> `
file 1.mp3
file 2.mp3 ....


## &nbsp;&nbsp; Trim audio
> `ffmpeg -ss start -i i.mp3 -t len e o.mp3`

> -ss : start from (in seconds or std frmt)

> -t : length (end = ss+t)

> -to : end time (in std frmt)

## &nbsp;&nbsp; Fade in/out  'n's of audio:

> `ffmpeg -i i.mp3 -af "afade=t=in/out:st=L-n:d=n" o.mp3`

> L = length of audio

## &nbsp;&nbsp; Increase or decrease volume by 'x' times
> `ffmpeg -i i.wav -filter:a "volume=x" o.wav`

> or by ±ndb:

> `ffmpeg -i i.wav -filter:a "volume=ndb" o.wav`

## &nbsp;&nbsp; scale/resize a video/img
> _resize_ :`ffmpeg -i in -vf scale=w:h out`

> _scale_ : `ffmpeg -i in -vf scale=w:-1 out`

> half the size: `ffmpeg -i in -vf scale=w=iw/2:h=ih/2 out`

## &nbsp;&nbsp; Rotate video
> `ffmpeg -i i.mp4 -vf "transpose=1" o.mp4`

> where

> 0 = 90CounterCLockwise and Vertical Flip (default)

> 1 = 90Clockwise

> 2 = 90CounterClockwise

> 3 = 90Clockwise and Vertical Flip

## &nbsp;&nbsp; Speed up/down video by 'x' times
> `ffmpeg -i i.mp4 -filter:v "setpts=PTS/x" o.mp4`

## &nbsp;&nbsp; Stabilize video
> `ffmpeg -i i.mp4 -vf vidstabdetect -f null -`

> `ffmpeg -i i.mp4 -vf vidstabtransform o.mp4`

## &nbsp;&nbsp; Export single frame
> `ffmpeg -i i.mp4 -ss ... -frames:v 1 o.jpg`

## &nbsp;&nbsp; Adjust quality of video
> `ffmpeg -i .. -c:v libx265 -crf ... o.mp4`

## &nbsp;&nbsp; Add metadata
> `ffmpeg -i . -metadata k=v -c copy ..`

> **MP4**: title, author, album, artist, composer, year, track, comment, genre, copyright, description, synopsis, show, episode_id, lyrics

> **MP3**: album, composer, genre, copyright, title, language, artist, album_artist, disc, publisher, track, encoder, lyrics, date, creation_time, performer

## &nbsp;&nbsp; Add cover art
> `ffmpeg -i i.mp3 -i cov.png -c copy -map 0 -map 1 o.mp3`

## Extract subtitles
> `ffmpeg -i i.mkv -c copy -map 0:s:0 s0.srt -c copy -map 0:s:1 s1.srt ...`


# Pandoc

## &nbsp;&nbsp; Markdown to PDF
> `pandoc i.txt --pdf-engine=xelatex -o o.pdf`

## &nbsp;&nbsp; Set margin to output pdf
> `pandoc i.txt -V geometry:margin=1in --pdf-engine=xelatex -o o.pdf`


# Miscellaneous functions

## &nbsp;&nbsp; Compress pdf:
> `pdfcompress i.pdf o.pdf level`

> level:

> - screen: 72dpi

> - ebook: 150dpi

> - prepress: 300dpi


# SoX

## &nbsp;&nbsp; Create silent audio of 't's
> `sox -n -r 16000 -c 1 o.mp3 trim 0.0 t`


# GhostScript

## &nbsp;&nbsp; Combine pdfs
> `gs -dBATCH -dNOPAUSE -q -sDEVICE=pdfwrite -sOutputFile=out.pdf ins.pdf`

* * *
\newpage


# Multiline scripts
## Average of images
```bash
i=0
for file in img*jpg; do
    echo -n "$file.. "
    if [ $i -eq 0 ]; then
        cp $file avg.jpg
    else
        convert $file avg.jpg -fx "(u+$i*v)/$[$i+1]" avg.jpg
    fi
    i=$[$i+1]
done
```

* * *
\newpage

# Troubleshooting
## Git
### RPC failed; result=22, HTTP code = 408
> `git config http.lowSpeedTime 600`
> `git config http.postBuffer 524288000`
