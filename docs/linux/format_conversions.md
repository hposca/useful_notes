# How to convert between formats

## m4a to mp3

    ffmpeg -i <file>.m4a -acodec libmp3lame -ab 128k <file>.mp3

### Automatically

    for file in *m4a; do echo ffmpeg -i $file -acodec libmp3lame -ab 128k ${file%.*}.mp3; done
