# How to convert between formats

## m4a to mp3

    ffmpeg -i <file>.m4a -acodec libmp3lame -ab 128k <file>.mp3

### Automatically

    for file in *m4a; do echo ffmpeg -i $file -acodec libmp3lame -ab 128k ${file%.*}.mp3; done

# How to remove accent characters

With the help of: http://www.linuxquestions.org/questions/linux-newbie-8/how-to-remove-accent-characters-4175431373/

    find . -type f -iname "*.py" | while read file; do
        iconv -f UTF-8 -t ASCII//TRANSLIT $file > $file-tmp
        mv $file-tmp $file
    done
