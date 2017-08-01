
# Table of Contents
- [Introduction](#introduction)
- [&&](#&&)
- [For loop:](#for-loop)
- [Intermediate: multiple command in your for loop:](#intermediate-multiple-command-in-your-for-loop)
- [Intermediate: Pipe to files:](#intermediate-pipe-to-files)
- [Recursive loop (search through all subfolders)](#recursive-loop-search-through-all-subfolders)
- [Advanced - combining for and find for piping purposes](#advanced-combining-for-and-find-for-piping-purposes)


## Introduction
As per our workshop, here’s three methods that allow you to increase the scale of your command line use by incorporating batch processing (`&&`, `for` loop, `find` loop). 
WARNING - with great power comes great reponsibility! Always perform tests in an isolated environment, perhaps on a test folder on an external drive. Be incredibly careful when launching any batch process on archival material.

## &&
`mediainfo /users/kieran/Downloads/youghal_clock_tower_animation.mov && mediainfo /users/kieran/Downloads/hallowed_fire.mov`  

This is one of the most simple methods to string commands together. You simply write out a list of commands that are linked with `&&`

NOTE:
`&&` means ‘run the next command if the previous command did not return an error.
`&` A single `&` means : run the next command regardless of any errors.

## For loop:
This will loop through files in your current directory.
`for i in /users/kieran/Downloads/*.mov ; do mediainfo "$i"; done`
Breakdown:
`for i in *.mov` - For all files that  end in .mov
`;` indicates the end of the first command 
`do` - do something for each file that matches the filename pattern
`mediainfo "$i"` - runs mediainfo on one of the files that was discovered in the initial for command. Bash will replace “$i” with a real filename.
`;` indicates the end of the command 
`done` - When all files that match the filename pattern have been processed, end the script.

## Intermediate: multiple command in your for loop:
You can string together multiple commands in your loop as seen in this example:
`for i in /users/kieran/Downloads/*.mov ; do mediainfo "$i"; exiftool "$i"; md5deep "$i"; done`
Which launches mediainfo, exiftool and md5deep.

## Intermediate: Pipe to files:
The for loop examples so far will all just print information to your screen. If you wanted to redirect the information to a sidecar file, you would use piping.
`for i in /users/kieran/Downloads/*.mov ; do mediainfo "$i" > “$i”_mediainfo.txt; done`
So ` > “$i”_mediainfo.txt` means:
Instead of printing information to screen, redirect the information to “$i”_mediainfo.txt`
This results in the creation of a sidecar file with `_mediainfo.txt` appended to the filename. 

## Recursive loop (search through all subfolders)
`find /Users/kieran/Downloads/objects -name "*.md5" -exec validate.py {} \;`

Breakdown:
`find` - Call the bash program called `find`.
`/Users/kieran/Downloads/objects` - Absolute path that you want the search to start from.
`-name "*.md5"` tells `find` to try to find all files that end with `*.md5`.
`-exec validate.py` When a file is found that matches the `-name` pattern, execute validate.py
`{}` - bash will replace this value with the filenames that match the `-name` pattern
`\;` ends the command

You can replace `validate.py` with whatever command you’d like to execute, for example, to run mediainfo on a files that match a `*.mov` pattern, then you could run:
`find /Users/kieran/Downloads/objects -name "*.mov" -exec mediainfo {} \;`


## Advanced - combining for and find for piping purposes

It seems to be very difficult to pipe to a file when using find, but you can do the following 
for i in `find /users/kieran/Downloads  -name "*.mov"`; do mediainfo "$i" > "$i".txt; done

This seems to go quite haywire when files or folders have spaces so use with caution!
