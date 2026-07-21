**[< Home](README.md)**
# FFMPEG

Prendre les 10 premières secondes d'une video :
```batch
ffmpeg -i input.webm -t 10 -c copy output.webm
```

Enregistrer à partir d'une url
```batch
ffmpeg -i "https://example.com/video/playlist.m3u8" -c copy video.mp4
```

Ajouter une piste audio a une video : 
```batch
ffmpeg -i video.mp4 -i audio.mka -map 0 -map 1:a -c:v copy output.mkv
```
