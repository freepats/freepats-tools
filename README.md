# SF-Tools

Tools to manage, create and convert sound fonts, collections of sampled
musical instruments and sound banks. Originally created for the FreePats
project: http://freepats.zenvoid.org/


## Dependencies

Requires Python 3 with dateutil, soundfile and numpy modules. This will
install the required dependencencies on Debian and derived distributions:

    apt-get install python3 python3-dateutil python3-soundfile python3-numpy

Debian jessie does not have python3-soundfile. It can be installed manually
or, if you prefer, by packages from this custom repository:
http://zenvoid.org/debian/


## Usage

There are two programs included:

* createSFZ.py: Takes audio files as input and writes to stdout a SFZ template
for them.

* convertSoundBank.py: Process a sound bank and writes another file, possibly
converted to a different format.


createSFZ.py is useful to create a new sound bank in SFZ format. It accepts a
collection of samples as a list of arguments and writes a template to the
standard output.

The SFZ format is composed of text that can be modified with any text editor
to complete the missing parts. Later, it can be converted to other formats
with the convertSoundBank.py program.

createSFZ.py will try to guess the pitch of each sample from its file name.
For this reason, each sample must be named with a suffix indicating its note.
It accepts either standard MIDI numbers (where 60 is middle C), or English
alphabetic notation plus an octave number (where C4 is middle C).

Examples:

    createSFZ.py piano_C4.wav piano_C5.wav piano_F#4.wav piano_F#5.wav
    createSFZ.py samples/*.wav > soundBank.sfz


Generated output will look like this:

    //+ Name: Unnamed sound bank
    //+ Date: 2016-12-19
    
    <global>
     //+ Instrument: Unnamed instrument
     ampeg_release=0.5
    
    <group>
     loop_mode=no_loop
    <region>
     hikey=62
     pitch_keycenter=60
     sample=piano_C4.wav
    <region>
     lokey=63
     hikey=68
     pitch_keycenter=66
     sample=piano_F#4.wav
    <region>
     lokey=69
     hikey=74
     pitch_keycenter=72
     sample=piano_C5.wav
    <region>
     lokey=75
     pitch_keycenter=78
     sample=piano_F#5.wav


Lines starting with // are comments. Lines starting with //+ contain hints
that will be processed by these tools to provide additional information and
aid conversion between different formats. They will be ignored by SFZ players.

There are a few things that should be edited. In particular:

* Name of the sound bank and instrument. For compatibility with the SF2 format,
the instrument name should be no longer than 19 characters.

* If samples contain loops, `loop_mode` instruction should be modified and each
sample should have `loop_start` and `loop_end` information added.


convertSoundBank.py can be used to validate and convert the SFZ file. When
converted to SF2, global options included within `<global>` will be converted
to global instrument options that can be overriden in subsequent groups or
regions.

This example will create a SF2 sound font.

    convertSoundBank.py grandPiano.sfz grandPiano.sf2

The resulting file grandPiano.sf2 should be ready to be used with fluidsynth,
qsynth, or any other program that handle SF2 files. It can also be open with a
sound font editor (swami, polyphone...) and inspect or continue editing its
contents.


## Limitations

This software is in development and currently supports a small set of
features.

* Only SFZ to SF2 conversion is available. The opposite conversion from SF2 to
SFZ is not done yet. Other formats are missing.

* Supports a minimal subset of SFZ opcodes.

* Stereo samples within SF2 sound banks are not supported yet.


## License

This program is free software: you can redistribute it and/or modify it under
the terms of the GNU General Public License as published by the Free Software
Foundation, either version 3 of the License, or (at your option) any later
version.

This program is distributed in the hope that it will be useful, but WITHOUT
ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS
FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.
