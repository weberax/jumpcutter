# Jumpcutter
Based on carykh's [jumpcutter](https://github.com/carykh/jumpcutter). It speeds up or slows down silent and louder parts of a video at different rates.
This version reduces the work done by him as there were a few locations to optimize:
1) He exported every frame to the tempfolder to manually drop or copy frames. This was removed and replaced by an automatically generated ffmpeg filter. More exactly: the `setpts` with a binary tree for the local speedup and offset of the audio chunks (thus running in O(n log n) and therefore may still be able to be improved by more ffmpeg magic).
2) During the time change of the audio he called `np.concatinate` on the wav output, which lead to array copies and thus performed poorly with longer videos. 
3) During the audio time conversion he also created temporary files containing the current audio chunk. This was replaced by the `ArrayReader` and `ArrayWriter` to remove unnecessary file access.

There also were my own changes such as:
1) Adding progressbars via the `tqdm` libary.
2) Removing the youtube download function as in my opinion it did not fit in and could be substituted by something like `youtube-dl`.
3) The ability to pass multiple input files and / or a directory with files to be converted.
4) The ability to use the python file as a module.

## Installing dependencies
To install the python libaries this script depends on, simply run `pip install requirements.txt`.

As it also relies heavily on ffmpeg, please make sure that it is installed and accesible over the commandline. 
See [FFmpeg Website](ffmpeg.org) for information on how to install it.

## Running it
To run it call `python jumpcutter.py -h` to get a help for the settings that you can pass to it.

To simply use default settings use `python jumpcutter.py -i INPUT_FILE`.
