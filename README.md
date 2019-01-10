####1. Split Mp4 to multiple small files. (each 10 secs.)####

ffmpeg -i HEATING_JACKETA.mp4 -map 0 -codec copy -f segment -segment_time 1:00 'output%03d.mp4'



https://superuser.com/questions/547296/resizing-videos-with-ffmpeg-avconv-to-fit-into-static-sized-player/1136305#1136305

ffmpeg -i input -vf "scale='min(1280,iw)':min'(720,ih)':force_original_aspect_ratio=decrease,pad=1280:720:(ow-iw)/2:(oh-ih)/2" output

ffmpeg -i c.mp4 -vf "scale='min(375,iw)':min'(667,ih)':force_original_aspect_ratio=decrease,pad=375:667:(ow-iw)/2:(oh-ih)/2" d.mp4

====
ffmpeg -i b.mp4 -vf "scale=750:1334:force_original_aspect_ratio=decrease,pad=750:1334:(ow-iw)/2:(oh-ih)/2" d.mp4

ffmpeg -i e.mp4 -vf "scale=750:1334:force_original_aspect_ratio=decrease,pad=750:1334:(ow-iw)/2:(oh-ih)/2" f.mp4


file f.mp4
file d.mp4

ffmpeg -f concat -i combine.txt -c copy output.mp4

ffmpeg -i b.mp4

ffmpeg -i intro.mp4 -i main.mp4 -i outro.mp4 -i logo.png 
-filter_complex "[0:v]scale=1280:720,setsar=sar=1[scaled]; 
[1][3]overlay=5:5[main]; [scaled][0:a][main][1:a][2:v][2:a]concat=n=3:v=1:a=1 [v] [a]" 
-map "[v]" -map "[a]" -c:v libx264 -b:v 1500k -c:a aac output.mp4

ffmpeg -i d.mp4 -c copy -bsf:v h264_mp4toannexb -f mpegts Ad.ts
ffmpeg -i f.mp4 -c copy -bsf:v h264_mp4toannexb -f mpegts Af.ts
ffmpeg -i "concat:Af.ts|Ad.ts" -c copy -bsf:a aac_adtstoasc Aoutput.mp4

convert format
ffmpeg -i d.mp4 -c:v libx264 -r 60 -c:a aac -ar 48000 -b:a 160k -strict experimental -f mpegts Bd.ts
ffmpeg -i f.mp4 -c:v libx264 -r 60 -c:a aac -ar 48000 -b:a 160k -strict experimental -f mpegts Bf.ts


----------------
mono to stereo: -af "pan=stereo|c0=c0|c1=c0"
stereo to mono: -af "pan=mono|c0=.5*c0+.5*c1"
======
ffmpeg -i d.mp4 -c:v libx264 -r 60 -c:a aac -ar 48000 -b:a 160k -af "pan=mono|c0=.5*c0+.5*c1" -strict experimental -f mpegts Bd.ts

ffmpeg -i d.mp4 -c:v libx264 -r 60 -c:a aac -ar 44100 -b:a 160k -af "pan=stereo|c0=c0|c1=c0" -strict experimental -f mpegts Bd.ts

ffmpeg -i f.mp4 -c:v libx264 -r 60 -c:a aac -ar 44100 -b:a 160k -af "pan=stereo|c0=c0|c1=c0" -strict experimental -f mpegts Bf.ts
----------------

ffmpeg -i "concat:Bf.ts|Bd.ts" -c copy -bsf:a aac_adtstoasc Boutput.mp4

ffmpeg -i f.mp4 -acodec libvo_aacenc -vcodec libx264 -s 1920x1080 -r 60 -strict experimental Bf.mp4

===========================
ffmpeg -i d.mp4 -c:v libx264 -r 60 -c:a aac -ar 44100 -b:a 160k -af "pan=stereo|c0=c0|c1=c0" -strict experimental -f mpegts Cd.ts
ffmpeg -i f.mp4 -c:v libx264 -r 60 -c:a aac -ar 44100 -b:a 160k -af "pan=stereo|c0=c0|c1=c0"  -strict experimental -f mpegts Cf.ts
ffmpeg -i "concat:Cf.ts|Cd.ts" -c copy -bsf:a aac_adtstoasc Coutput.mp4

original format change:
ffmpeg -i d.mp4 -c:v libx264 -r 60 -c:a aac -ar 44100 -b:a 160k -af "pan=stereo|c0=c0|c1=c0" -strict experimental -f mpegts Cd.ts

1. size change

ffmpeg -i b.mp4 -vf "scale=750:1334:force_original_aspect_ratio=decrease,pad=750:1334:(ow-iw)/2:(oh-ih)/2" d.mp4
ffmpeg -i e.mp4 -vf "scale=750:1334:force_original_aspect_ratio=decrease,pad=750:1334:(ow-iw)/2:(oh-ih)/2" f.mp4

2.  format change

ffmpeg -i d.mp4 -c:v libx264 -r 60 -c:a aac -ar 44100 -b:a 160k -af "pan=stereo|c0=c0|c1=c0" -strict experimental -f mpegts Cd.ts
ffmpeg -i f.mp4 -c:v libx264 -r 60 -c:a aac -ar 44100 -b:a 160k -af "pan=stereo|c0=c0|c1=c0"  -strict experimental -f mpegts Cf.ts

3. combine

ffmpeg -i "concat:Cf.ts|Cd.ts" -c copy -bsf:a aac_adtstoasc Coutput.mp4

