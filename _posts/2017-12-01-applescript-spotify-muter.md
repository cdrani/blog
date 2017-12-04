---
layout: post
title: 'Applescript: Spotify Muter'
date: 2017-12-01 13:15:20 -0600
categories: applescript
---

Now that I have fully committed to using tmux in my workflow, I've done further
research into how to personalize it. Unfortunately, most plugins are written in
a shell script, which is understandable as a shell like **bash** is available
(or built-in) in most distros or OSs. I do want to learn how to write shell
scripts, not just for the purposes of tmux, but as well as the universality of
it. I don't want to be firmly stuck in javascript land and not peer over the
hedge at all, or even worse just wait until javascript implements what I want to
learn (** cough machine learning cough **).

> NOTE: Applescripts will only work on a mac ecosystem.

Now that that preamble is over, let's create a script that's **human-readable**
using applescript. Notabley, we could also write it using javascript using the
"applescript editor" app. If we were to not use the app and still want to write
the script using javascript, then we would need our script to be executed in a
node env: `#!/usr/bin/env node`, I suppose.

## IMPLEMENTATION: SETUP

As for most script files, the heading should be the environment that the file
should be run in:

> #!/usr/bin/env osascript

1. We want to **tell** the desktop Spotify to mute/unmute itself based on
   certain conditions. The conditions being executed are predicated on the
   belief that Spotify is running && that currently playing a song:

```bash
tell application "Spotify"
  if it is running the
    if the player state is playing then
```

2. Now we want to retrieve the current track name as well as the name of the
   artist on the track:

```bash
set track_name to name of current track
set artist_name to artist of current track
```

3. This is now the crux of the entire script: muting/unmuting the app based on
   the player state. If there's no artist, then mostly there's an advert on, and
   we want to mute the app. On the other hand if there's a song playing we want
   to unmute the app. Additionally we want to display the **_track_name_** and
   the **_artist_name_**. Additonally we could also retrieve other information,
   such as the **_album_name_**, or any other variables you captured earlier:

```
if artist_name > 0
  # track has artist set and mostly likely a song
  set volume without output muted
  "♫ " & track_name & " - " & artist_name
else
  # track does not have artist set and most likely an advert
  set volume with output muted
  "~ ADVERT"
end if
```

4. And that's about it - other than closing the earlier if statements and
   telling Spotify that's the end of the commands. Here's the completed script:

```bash
#!/usr/bin/env osascript
# Returns the current playing song in Spotify for OSX

tell application "Spotify"
  if it is running then
    if player state is playing then
      set track_name to name of current track
      set artist_name to artist of current track

      if artist_name > 0
        # track has artist set and most likely a song
        set volume without output muted
        "♫ " & track_name & " - " & artist_name
      else
        # track doesn't have an artist set and is most likely an advert
        set volume with output muted
        "~ ADVERT"
      end if
    end if
  end if
end tell
```

5. Now we want to make the script executable. I saved mine as **spotify** in a
   scripts directory (of which I am inside):

   > chmod +x spotify

6. The last step is to plug it in our tmux status line. I chose the right hand
   side:
   > set -g status-right '#(~/scripts/spotify)'

## Musings:

I actually enjoyed that. I created something that was both useful for myself
**_and_** actually quite easy to implement from ideation to full
conceptualization. Even if I stray from the applescript approach and learn how
to write bash scripts, I will continue to write some small scripts like this one
for my own enjoyment, regardless of their immediate practicality.
