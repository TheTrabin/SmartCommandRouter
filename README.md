# 🧠 Smart Command Router
# 📝 [TheTrabin](https://trabin.space)
## ✅ 0.1.2

built in Streamer.bot v0.2.8
have not tested on alpha or beta for version 1.0.0 and up
If you do, please let me know how it goes at
[Cobalt Rogues Discord](https://www.discord.gg/EkUmf9SJVk)

[requirements](#requirements-req)
[JSON Loader](#json-loader-jsonloader)
[Folder Loader](#folder-loader)
[features](#features-features)
[commands](#commands-commands)
[json](#json)
[More from Trabin](#more)

## File Contents

- This readme file
- Assets 7-Zip File - drop into streamer.bot data folder.7z

### 7-Zip file - The Assets Brick

- Import folder
    This folder requires you to either import via Streamer.bot or via OBS
        - 1 txt file with the long string import code for Streamer.bot
        - 1 `smartcommandrouter.sb` that you should be able to drag and drop into the import box in Streamer.bot
        - 1 OBS json Source Copy compatible file [importing for OBS](#optional)

- Data folder
    This folder houses all the stuff the command router uses

- Data/Attribution
    The contents of this folder is for where the content comes from,
    and where to find more of their content. 
    They made some of the stuff in there, so.

- Data/Commands

    The contents of this folder is where the JSON commands are dropped into and where the
    JSON Loader will load commands that correlate to those that are present in Streamer.bot.
    The commands don't have any security on them and are active regardless of level of connection.
    Viewer or mod or anyone can use these.

- Data/images

    Houses more than just images
    This is what the inline loader relies on for a lot of content,
    such as having a folder in images that contains:

    > - pictures,
    > - sounds,
    > - or even a text file named lines.txt to allow messages to show up on screen

- Data/restrictions
    If you want to restrict commands based on if a game or tag is present

- Data/sounds
    Contains sounds for the sound actions.
    Put more sounds in them to make them play randomly
    when the command associated is triggered

## Requirements {#req}

You need [streamer.bot](streamer.bot) in order for the main meat and potatoes to work
you'll need to Import the contents of `commandrouter.txt` in order
to get it to work.

`commandrouter.txt is located in the Import folder`

### Optional

#### Import the OBS Scene

This scene has the

```json
sceneName,
mediaSource,
textSource,
audioSource
```

and then all you'd have to change is the targetOBS in the Folder Loader Action,
else see [OBS Scene variables](#obs-scene-and-variables-obsvariables)

For OBS, you'll need in order to import/load in the obs scene:
[Source Copy by Exeldro](https://obsproject.com/forum/resources/source-copy.1261/)
which has the function to import and export scenes.

Just point the load option to streamerbotscene.json in the import folder, and load in
It should import the proper types and a few extras
It's a Bundled Scene for Streamer.bot

## JSON Loader {#JSONLoader}

The JSON Loader just needs commands folder with command-format JSON.
For now, it only reads the category, what the command is, and what action it needs to trigger.

`Contents of Fun.json in /data/commands`

```json
{
    "Fun": {
      "!boop": "BoopAction",
      "!wiggle": "WiggleDance"
    }
  }
```

In this example, it places the command in the `Fun` category of commands
It then creates the association of the command with the action
`BoopAction` as an action in Streamer.bot will trigger with `!boop` in your chat
For the record, these actions don't exist in the import
so this gives you a chance to figure out how you want it to look
or do and let your creativity go wild

There are ways of extending it, and I'd love to see some of what y'all create with these basic blocks.

## Folder Loader

The inline C# script loads things up dynamically in a few ways
If you do have a folder in `data/images` that has the exact name
it should trigger off, so if you have `data/images/cat`
you should be able to `!cat` in chat
and it should pick something at random.

Otherwise you'll have to go into the code and add things in manually as a register to a function[^1]

## OBS Scene and variables {#obsVariables}

| variable name | What it is |
|---------------| ----------- |
|targetOBS | The OBS Connection you want to use under Stream Apps in Streamer.bot|
|sceneName | The scene in your OBS Connection that has the source |
|mediaSource | The Media Source located in the scene you want things to pop-up on|
|audioSource | The Sound source you want to trigger a sound from, must be an audio source |
|textSource | The text(GDI+) source you want to direct messages to via lines.txt located in the folder|

The `Folder Loader` needs these targets added into the

`Arguments to pass down` folder

located in the Streamer.bot Sub-Actions in order for the router
to post images, sound, and text located in folders
to the appropriate sources

## Other plugin compatibility

It won't be able to use previously created actions in streamer.bot
nor will it read commands created in streamer.bot
so you'll have to use a different system to dynamically get
a list of commands and post those along with these
but the Idea was that it would be something quick to set up
for someone just starting with streamer.bot and get an idea
of just how simple to complex using streamer.bot
and just how extensible it can be.

Eventually I'll find a way to collect and post commands from streamer.bot
so it can be a list that can be translated both by:

- Category : The Category of Command
- Command : The text input command associated with the Command
- Action : The Action triggered by the Command

At which point any command will become compatible with
this system regardless of how it was added

Until then, this system will more so focus on
creating an add-on infrastructure to allow
streamers to quickly get a semi-customized experience

## Features {#Features}

### What you need to Customize {#custom}

| Command | Action | What to fix |
|!jump,!js | jump scare | You'll need to provide your own video and set your media source to that |
|!clink, !cheers, !celebrate, !hype | celebrate | These need filters put on those sources that you target[^2] |
|!discord | Discord | Put your own discord and blurb about your own discord, unless you want them coming to me :joy: |
|!rules | rules| Fix them up with your own spin, but they're good on their own [^3] |
|!social | social | Put your own socials in there |
|!specs | specs | Put your own tools of the trade or don't, I'm not your boss |
|!welcome | welcome | Make your own welcome that fits you and says something short about what you do |

There is some simple stuff you have going for you here, but let's explain some of the basics

# Commands {#commands}

### !Command

Lists all registered commands via JSON Loader and Folder Loader
Currently JSON Loader only accepts one format for registering commands, but may expand in the future.

An acceptable JSON is represented in [JSON Loader](#json-loader-jsonloader)

In the future, it may also contain additional information that includes:

- target resource
    such as:
        - mediaSource
        - audioSource
        - textSource
- Command Aliases
    such as:
        - different prefix catching
        - additional suffix handling

Suggestions welcome

### Information

#### !rules

This contains a set of rules that seem to be the
average of the normal Twitch Etiquette,
a shorter less emote filled version is in the
comments of the action.

#### !social

Meant to present your socials and how to use a variable.
If all your socials are the same, then you can generally
just copy and paste the `%broadcastUser%` and the contents of it
and just put it where your username is

The `%broadcastUser%` will take your Twitch Username
and put it everywhere that variable is.
Also, pressing enter to make a new line
makes a brand new message in chat
and each message you CAN have up to 500 characters
so keep that in mind when you're making your messages

#### !discord

This is definitely something you want to fully customize to include
your own discord link. If you don't have a discord home,
we've got one for you in there.
We could always use some new friends, too

#### !welcome

This is a great option for folks when you get a raid in
one of your mods or yourself can toss down a `!welcome` command
and it quickly tells your new friends a little more about you
without having to completely shut down stream and
maybe get into a better spot to then address the new community
into your space. This can also include links

#### !specs

Think of this not as like this is how I'm going to show off my tech
even though that's how it normally is.
But what about your other tools?
Some people put their Cameras on there
or how about if you crochet or knit?
What are those tools worth to you?
Are those worth maybe showing off to your fans?
How about your favorite brushes if you're a painter?
There's a mountain of options that might fit in this same space.

### Support

Things like financial support of the stream of willing individuals

#### !tip

Replace with your donation information

#### !prime/ !freesub

Just presents a Use-Your-Prime-On-Me and subscribe link

### Tracking

People like doing things that track what they're doing

#### !Follow / !followage

Checks how long a user has been following the channel

#### !lurk

Means you're "Still Watching" but not at the chat window. This gives a cute message.
Edit it to fit your theme

#### !unlurk/!back

Means you're now active at the chat window. This gives a cute message.
Edit it to fit your theme

### Celebration

The Hurrays

#### !so, !shoutout, !shout

Followed by a username, should shout them out

#### !fireworks

Requires pointing to a scene or object in a scene that has a fireworks filter [^2]
Turns the filters on for a few seconds and then turns the filters off

#### !cake

shows a picture of a cake

#### !rainbow

Requires pointing to a scene or object in a scene that has a rainbow filter [^2]
turns the filter on for a few seconds and then turns the filter off

#### !balloon

shows a picture of a balloon

### Fun

cute fun for chat to play around with

#### !pp

Who doesn't like to measure their RNG featured E-Peen against each other?
type `!pp` and get a number between 1 and 10 and make fun of each other
Who has the longest number? Who has the shortest? come find out!

#### !love

Find out how compatible you are with Someone else in chat

#### !spank

Be Naughty, Spank someone

#### !fmk

The ol' Funk Marry Krill where it takes 3 folk randomly and assigns them in spots to give you a crazy combination

### Sounds

#### !fart

blames you for cutting the cheese in 16 flavors

#### !tweet

plays bird sounds and blames you for summoning a flock

#### !bark, !arf

dog noises

#### !mew, !meow

cat noises

#### !roar

random roar from an animal, I think

#### !growl

random growl from an animal, I think

#### !ching

cash register sound

#### !ding / !levelup / !lvlup

level up sound

#### !duck / quack

duck noise

#### !cow/!bovine/!moo

cow sound

### Videos

#### !jump/!js

Jumpscare, you supply the video

#### !clink / !cheers / !celebrate / !hype

shows a picture of a gathering and celebration

#### !shook / !gasp / !surprised / !surprising

shows a gasping face on screen

### Animations

#### !Weather

does one of the 4 random weather actions

#### !rain

Rain

#### !snow

Snows

#### !blizzard

It's like snow but more of it

#### !storm

It's not the X-man, but it's Thunder and Lightning - Very Very Frightening

#### !bounce / ball

A bouncing soccer ball

### Folder Magic

#### !tree

Shows a random picture of a tree, with a noise, and a welcoming growth message

#### !balloons

Shows a random balloon and an uplifting message and the sound of a balloon inflating

#### !thankyou/ !ty

A Thank You Celebration!

#### !popcorn

a picture of a popcorn maker, some corn, popcorn - completely random
popcorn sounds
and a spontaneous message or buttered drama? Find out!

#### !hearts / !heart

Heart will show one image, but Hearts adds a message and a lovely sound

#### !roses

Shows a random rose

#### !flowers

Shows random flowers

#### !Candy

Shows random candy with a Crunch!

#### !media_

media commands with an underscore followed by the foldername in `data/images/` will display random media in those folders
Commands premade: `!media_tree`, `!media_balloons`, `!media_cake`, `!heart`, `!hotairballoon`

## FootNotes

[^1]: I'll be working on a better solution where it reads from a list or a text file in later versions
[^2]: I use [obs-shadefilters by Exeldro](https://obsproject.com/forum/resources/obs-shaderfilter.1736/)
[^3]: There's messages that describe shorter versions that might be better. Or rewrite them entirely

## {#json}

You should just make a brand new one if you're adding in your own commands

### Template

```json
{
    "Category": {
        "!command" : "action",
        "!command1" : "action1",
        "!command2" : "action2",
        "!commandalias" : "action1"
    },
    "Category1" : {
        "!anothercommand" : "action17",
        "!adifferentcommand1" : "action18",
        "!anotherdifferentcommand2" : "action22",
        "!somecommandalias" : "action17"
    }
}
```

Whatever command you attach to an action
the action must be exactly what the title
of the action in Streamer.bot
otherwise it will not trigger

## {#more}

### [Auto Scene Switch](https://trabin.space/products/auto-scene-switch)

Allows you to put your process on a list so that when it's on
it'll automatically change to that scene
as well as be able to set a default scene
Streamer.bot v0.2.8 and up

### [Auto Game Checker](https://trabin.space/products/autogamecheckersb)

Allows you to put your process on a list so that when it's on
it'll automatically change your title and game category
on twitch without you needing to input
great for those of you who stream multiple things in one stream
Streamer.bot v0.2.8 and up

### [A Little Trouble in Tech City](https://trabin.space/products/a-little-trouble-in-tech-city)

It's a troubleshooting guide for Computers, goes over a bunch of things ranging from CPUs to GPUs
has some Windows 11 centric quick fixes and tweaks to get better performance
and what sort of tools out there to have better control of the user's pc
Check it out
