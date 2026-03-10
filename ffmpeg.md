Enregistrer à partir d'une url
```
ffmpeg -i "https://example.com/video/playlist.m3u8" -c copy video.mp4
```
Ajouter une piste audio a une video : 
```
ffmpeg -i video.mp4 -i audio.mka -map 0 -map 1:a -c:v copy output.mkv
```
