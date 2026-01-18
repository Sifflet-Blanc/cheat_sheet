Ajouter une piste audio a une video : 
```
ffmpeg -i video.mp4 -i audio.mka -map 0 -map 1:a -c:v copy output.mkv
```
