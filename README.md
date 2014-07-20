# Ableton Clip Colorizer

This tool colorizes your Ableton clips based on their name and a match/color configuration.

## Installation

Requirements:

[LiveOSC]('http://livecontrol.q3f.org/ableton-liveapi/liveosc/')
Note: be sure to select LiveOSC as a control surface in Live's preferences

Ruby and the following ruby gems:

```bash
# Command-line
gem install yaml
gem install osc-ruby
gem install eventmachine
```

## Usage

Run colorizer.rb after configuring colorizer.yml to your needs:

```bash
ruby colorizer.rb
```

## Configuration

Colorizer automatically retrieves clip information from Live and updates your clip colors accordingly. Since Live fires events when you make / rename clips, it will continue to colorize in real-time if you leave it running. A cool side-benefit!

The only real configuration is in colorizer.yml, where you specify a list of colors by name ('colors' section) and nested clip names to match against ('names').

For each clip, colorizer attempts to match the track name and clip name down the list of matchers. Example:

```yaml
names:
  Synths:
    default: blue_1
    double: blue_2
    triple: blue_3
  Basses:
    default: gray_1

colors:
  blue_1: 8bc5ff
  blue_2: 10a4ee
  blue_3: 0303ff
```

Any clip in a track named "Synths" will match this group, as will a clip anywhere named "Synths". Once "Synths" has been matched, colorizer continues down the nested list, trying to find the first next successful match. If your clip in the "Synths" track is called "Normal", it won't match anything further, so it will use the "default" color. If your clip in the "Synths" track is called "triple", it will match the triple setting and return blue_3.

The named colors (like blue_3) are referenced in the 'colors' section. These are sent to Live as-is, which finds the closest allowed clip color and uses that.

## Notes

- Tracks in folded groups are not be processed (a limitation of LiveOSC? Not sure.)
- Matches are done in order, so for example if a clip matches both "Synths" and "Basses" in the above configuration, it will dig into the "Synths" section for matches only.