# Download a video from youtube

To download a video from youtube:

    youtube-dl --list-formats $youtube_link

After seeing the list of options and formats

    youtube-dl -f $format $youtube_link

e.g. for download only the audio:

    youtube-dl -f 140 $youtube_link

# Convert the downloaded audio

When you download the audio it have an `m4a` extension, to convert to a `mp3` audio file:

    ffmpeg -i $m4a_file -acodec libmp3lame -ab 128k ${m4a_file%.*}.mp3

To to a batch conversion, and have safe names:

    for file in *m4a; do rename 's/ /_/g' $file; done && for file in *m4a; do ffmpeg -i $file -acodec libmp3lame -ab 128k ${file%.*}.mp3; done
