# ffmpeg-passthru-finder-action
Automator quick action to passthru streams of selected media file to MKV container

## Why was this created?
My camera outputs recordings to a DAV file format.  Unfortunately this isn't a very compatible format, and the only way to get VLC to play it is to mess around with some deep settings that I'd rather not touch.  I happen to know that my camera is just recording a H.265 stream, which will playback perfectly if I do a passthrough to a MKV container.  After a quick proof of concept with ffmpeg (i.e. `ffmpeg -i input.dav -codec copy output.mp4`) I put together this automator action.

## Requirements
Assumes [ffmpeg](https://ffmpeg.org) is already installed at `/usr/local/bin/ffmpeg`

## What this action does
This "Quick Action" adds "Passthru to MKV" as an option under "Services" when you right-click a file.  A new file will be created in the same directory with a '.mkv' extension.
![Screenshot of Services menu](example.png "Screenshot of Services menu")

If you inspect the workflow you will see it simply amounts to the following bash script

```
#finder item passed in as arguemnt
full_input_path=$1

#paramater expansion to remove file extension
#https://stackoverflow.com/questions/12152626/how-can-i-remove-the-extension-of-a-filename-in-a-shell-script
full_input_path_no_ext=${full_input_path%.*}

#create output path
full_output_path=$full_input_path_no_ext
full_output_path+=".mkv"

#use ffmpeg to pass the video stream through to a new container
/usr/local/bin/ffmpeg -i "${full_input_path}" -codec copy "${full_output_path}"

```
