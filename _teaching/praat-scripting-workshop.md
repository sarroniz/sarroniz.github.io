---
layout: page
title: Introduction to Praat Scripting
description: Linguistics Club Workshop (Fall 2026)
category: workshops
importance: 2
permalink: /teaching/praat_scripting/
img: /assets/img/praat-workshop.png
---

A hands-on introduction to scripting in Praat, offered for the Linguistics Club at the University of
Nevada, Reno. The workshop is designed for a mixed audience. We start with how Praat's windows work, move on to the building blocks of almost every script (commands, variables, loops and TextGrid queries), and finish by measuring the duration, F1 and F2 of 16 vowels in a single click. Every exercise has a **Core** task for everyone and a **Level up** task for those who finish early.

## Workshop Materials
- **Slides:** [PDF]({{ '/assets/pdf/praat-scripting-workshop.pdf' | relative_url }})
- **Exercise files:** [praat-workshop.zip]({{ '/assets/files/praat-workshop.zip' | relative_url }})
- **Praat:** free download at [praat.org](https://www.praat.org) (any recent version works)

The zip file contains three folders:

| Folder | Contents |
|---|---|
| `audio/` | 16 short recordings of hVd words (*heed, hid, head, had, hod, hood, who'd, hud*) from two synthetic voices (F1 = female, M1 = male), each with a TextGrid. Tier 1 = `word`, tier 2 = `phone`. The voices were made with Praat's own speech synthesizer. |
| `scripts/` | The exercise files. |
| `solutions/` | Complete versions of every exercise, including the Level up tasks. |

### Before the workshop
1. Install Praat.
2. Unzip `praat-workshop.zip` somewhere easy to find (Desktop or Documents).
3. In the Praat Objects window, choose **Praat > Open Praat script…**, open `scripts/01_hello.praat` and run
it with **Cmd+R** (Mac) or **Ctrl+R** (Windows).

> **Tip:** open the scripts from Praat instead of pasting their code into a new window. The scripts use
> relative paths such as `../audio/`, which Praat reads from the folder the script is saved in.

## Schedule

| Part | Topic |
|---|---|
| 1 | Why script? |
| 2 | Praat in five minutes: objects, editors and *Paste history* |
| 3 | Your first script: commands, arguments and Exercise 1 |
| 4 | Variables, loops and TextGrids: Exercise 2 and two quick quizzes |
| 5 | Batch processing: measure a whole folder (Exercise 3) |
| 6 | Wrap-up: common errors and where to go next |

---

## 1. Why script?
Measuring F1 and F2 in our 16 vowels by hand takes roughly a hundred clicks: open each file, find the vowel,
place the cursor at its midpoint, read the values and type them into a spreadsheet. A script does the same
work in one click and about a second, and the same script works for 1,600 files.

- **Faster:** seconds instead of hours, and it scales from 16 files to 16,000.
- **Consistent:** the same settings and the same measurement point in every file, with no copy-paste typos.
- **Reproducible:** share the script and anyone can rerun (and check) your analysis.

## 2. Praat in five minutes
There are two windows you need to know:

- **Objects window.** Everything you open or create becomes an object in this list. Select an object and the
commands for it appear on the right. Every analysis adds a new object: *To Formant* gives you a Formant
object, *To Pitch* a Pitch object, and so on.
- **Editor window** (*View & Edit*). Waveform, spectrogram and TextGrid tiers: this is where you look at
sounds and annotate them.

![Waveform, spectrogram with formant tracks, and TextGrid of the word heed]({{ '/assets/img/praat-workshop.png' | relative_url }})

*The word heed (F1_heed.wav) in Praat's editor view: waveform, spectrogram with formant tracks, and the word
and phone tiers of its TextGrid.*

A script talks to the Objects window: it selects objects and presses their buttons, just like you do.

### Every click is a command
The most useful trick in Praat scripting is **Paste history**:

1. Open a script window with **Praat > New Praat script** and choose **Edit > Clear history**.
2. In the Objects window, select the Sound and choose **Analyse spectrum > To Formant (burg)… > OK**.
3. Back in the script window, choose **Edit > Paste history**.

Praat writes the commands you just clicked, with every setting you used:

```praat
selectObject: "Sound F1_heed"
To Formant (burg): 0, 5, 5500, 0.025, 50
```

You never have to memorize command names: do it once by hand, then paste the history.

## 3. Your first script

### Hello, Praat!
Open `scripts/01_hello.praat` and run it (**Cmd+R** or **Ctrl+R**):

```praat
# Lines that start with # are comments
writeInfoLine: "Hello, Linguistics Club!"
appendInfoLine: "You are running Praat ", praatVersion$
appendInfoLine: "2 + 2 = ", 2 + 2
```

The result appears in the Info window (your version number will differ):

```
Hello, Linguistics Club!
You are running Praat 7.0.03
2 + 2 = 4
```

`writeInfoLine:` clears the Info window and writes a line; `appendInfoLine:` adds a line underneath. Change the
message and run it again: that edit-and-run loop is how you will work all the time.

### Anatomy of a command
This is the command *Paste history* gave us:

```praat
formant = To Formant (burg): 0, 5, 5500, 0.025, 50
```

- **`formant =`** is a variable that remembers the new object, so you can select it later.
- **`To Formant (burg)`** is the command name, exactly as it appears on the button, followed by a colon.
- **`0, 5, 5500, 0.025, 50`** are the arguments, in the same order as the fields in the command's dialog,
separated by commas:

| # | Dialog field | Value |
|---|---|---|
| 1 | Time step (s) | 0 (automatic) |
| 2 | Max. number of formants | 5 |
| 3 | Formant ceiling (Hz) | 5500 |
| 4 | Window length (s) | 0.025 |
| 5 | Pre-emphasis from (Hz) | 50 |

Text arguments and menu choices go in double quotes, for example `"hertz"` or `"yes"`.

> **What is "burg"?** It refers to Burg's algorithm, which Praat uses to fit a linear-prediction (LPC) model
> of the vocal tract to each stretch of sound. The formants are the resonances of that model, which is why
> the formant ceiling matters so much (see the sanity check in Part 5).

Older scripts online use a different style, with dots and spaces: `To Formant (burg)... 0 5 5500 0.025 50`.
It still works, but new scripts should use the colon style shown here.

### Exercise 1: Your first measurement (5 min)
**Core.** Open `scripts/02_first_measurement.praat`, replace the `___` and run it.

```praat
sound = Read from file: "../audio/F1_heed.wav"
duration = ___
writeInfoLine: "Duration: ", fixed$ (duration * 1000, 0), " ms"
```

You should see:

```
Duration: 400 ms
```

**Level up.**
- Measure `M1_heed.wav` instead.
- Also print the sampling frequency (*Query > Get sampling frequency*).

<details markdown="1">
<summary><strong>Show the solution</strong></summary>

```praat
# Exercise 1 - solution (core + level up)

sound = Read from file: "../audio/F1_heed.wav"
duration = Get total duration
writeInfoLine: "Duration: ", fixed$ (duration * 1000, 0), " ms"

# Level up (b): sampling frequency
fs = Get sampling frequency
appendInfoLine: "Sampling frequency: ", fs, " Hz"
```

Tip: if you don't know a command's name, do it once by hand from the *Query* button and use *Paste history*.

</details>

## 4. Variables, loops and TextGrids

### Variables: numbers and text
- **Numbers** hold durations, frequencies and counts: `duration = Get total duration`, `ceiling = 5500`
- **Text (strings)** holds labels and file names, and the variable name always ends in `$`:
`label$ = Get label of interval: 2, 3`, `name$ = "F1_heed"`

Three rules to remember:
- Variable names start with a lowercase letter: `duration` works, `Duration` gives an error.
- Text needs a `$`: `name$ = "heed"` works, `name = "heed"` gives an error.
- `fixed$` turns a number into text with a fixed number of decimals: `fixed$ (0.2381, 3)` gives `"0.238"`.

### Loops and decisions
**Repeat** with `for … endfor`:

```praat
for i from 1 to 3
    appendInfoLine: "Round ", i
endfor
```

**Decide** with `if … endif`:

```praat
if label$ = ""
    appendInfoLine: "silence"
else
    appendInfoLine: label$
endif
```

`<>` means "is not", and you can combine tests with `and` / `or`. Indenting is optional but makes scripts
readable; `endfor` and `endif` are not optional.

### Quick check: what does this print?

```praat
x = 3
for i from 1 to 3
    x = x + i
endfor
writeInfoLine: x
```

<details markdown="1">
<summary><strong>Show the answer</strong></summary>

**9.** `x` starts at 3, and the loop adds 1, 2 and 3: 3 + 1 + 2 + 3 = 9. (The most common wrong answer is 6,
which forgets the starting value.)

</details>

### TextGrids: where your annotations live
A TextGrid is a set of numbered **tiers**, and each tier is a sequence of numbered **intervals**. In our
files, tier 1 is the word and tier 2 the phones. Silences count as intervals too, so in `F1_heed.TextGrid`
the vowel is interval 3 of tier 2:

| Interval (tier 2) | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|
| Label | *(silence)* | h | iː | d | *(silence)* |

Scripts don't "look" at a TextGrid; they ask it questions:

| Query (with the TextGrid selected) | Answer for F1_heed |
|---|---|
| `Get number of intervals: 2` | 5 |
| `Get label of interval: 2, 3` | "iː" |
| `Get start time of interval: 2, 3` | 0.065 |
| `Get end time of interval: 2, 3` | 0.303 |

### Exercise 2: Walk through a TextGrid (8 min)
**Core.** Open `scripts/03_textgrid_loop.praat`, fill in the two `___` and run it.

```praat
textgrid = Read from file: "../audio/F1_heed.TextGrid"
nIntervals = Get number of intervals: 2
writeInfoLine: "label", tab$, "duration_ms"
for i from 1 to ___
    label$ = Get label of interval: 2, i
    start = Get start time of interval: 2, i
    end = Get end time of interval: 2, i
    dur = (end - start) * 1000
    appendInfoLine: label$, tab$, fixed$ (dur, 0)
___
```

You should see:

```
label   duration_ms
        11
h       54
iː      238
d       80
        17
```

**Level up.**
- Skip the silences (hint: `if label$ <> "" … endif`).
- For the vowel, also print F1 and F2 at its midpoint (hint: you need a Formant object).

<details markdown="1">
<summary><strong>Show the solution</strong></summary>

Core: the loop runs `for i from 1 to nIntervals` and is closed with `endfor`.

```praat
# Exercise 2 - solution (core)

textgrid = Read from file: "../audio/F1_heed.TextGrid"
nIntervals = Get number of intervals: 2

writeInfoLine: "label", tab$, "duration_ms"

for i from 1 to nIntervals
    label$ = Get label of interval: 2, i
    start = Get start time of interval: 2, i
    end = Get end time of interval: 2, i
    dur = (end - start) * 1000
    appendInfoLine: label$, tab$, fixed$ (dur, 0)
endfor
```

Level up:

```praat
# Exercise 2 - level up: skip silences, add F1/F2 at the vowel midpoint

sound = Read from file: "../audio/F1_heed.wav"
formant = To Formant (burg): 0, 5, 5500, 0.025, 50
textgrid = Read from file: "../audio/F1_heed.TextGrid"
nIntervals = Get number of intervals: 2

writeInfoLine: "label", tab$, "duration_ms", tab$, "F1", tab$, "F2"

for i from 1 to nIntervals
    selectObject: textgrid
    label$ = Get label of interval: 2, i
    if label$ <> ""
        start = Get start time of interval: 2, i
        end = Get end time of interval: 2, i
        dur$ = fixed$ ((end - start) * 1000, 0)
        if label$ = "h" or label$ = "d"
            appendInfoLine: label$, tab$, dur$
        else
            mid = (start + end) / 2
            selectObject: formant
            f1 = Get value at time: 1, mid, "hertz", "linear"
            f2 = Get value at time: 2, mid, "hertz", "linear"
            appendInfoLine: label$, tab$, dur$, tab$, fixed$ (f1, 0), tab$, fixed$ (f2, 0)
        endif
    endif
endfor
```

</details>

### Quick check: spot the bug
Praat stops with an error when you run this script. Which line would you fix?

```praat
sound = Read from file: "F1_heed.wav"
name = selected$ ("Sound")
duration = Get total duration
writeInfoLine: name$, ": ", duration
```

<details markdown="1">
<summary><strong>Show the answer</strong></summary>

**Line 2.** `name` holds text, so it must be called `name$`. Praat's message is *Found a string expression
instead of a numeric expression.* Line 4 is a tempting answer: `name$` is used there, but it was never
created, because line 2 created `name` instead. Fixing line 2 fixes both.

</details>

## 5. Batch processing

### The batch recipe
Almost every batch script follows the same four steps:

1. **List the files** in a folder.
2. **Loop** over the list.
3. **Open and measure** each file.
4. **Write a row** of results and clean up before the next file.

```praat
fileList = Create Strings as file list: "files", folder$ + "*.wav"
nFiles = Get number of strings
for f from 1 to nFiles
    selectObject: fileList
    fileName$ = Get string: f
    sound = Read from file: folder$ + fileName$
    textgrid = Read from file: folder$ + (fileName$ - ".wav") + ".TextGrid"
    # ... find the vowel, measure F1 and F2 at its midpoint ...
    appendFileLine: outFile$, fileName$, ",", f1, ",", f2
    removeObject: sound, textgrid, formant
endfor
```

`Create Strings as file list` makes an object with one file name per line, and `Get string: f` returns the
f-th name. The minus sign removes text from a string: `fileName$ - ".wav"` turns `F1_heed.wav` into
`F1_heed`, which we use to find the matching TextGrid. `removeObject` keeps the Objects window from filling up
with hundreds of objects.

### Exercise 3: 16 vowels, one click (10 min)
**Core.** Open `scripts/04_batch_vowels.praat` and run it. Then open `vowels.csv` (it appears next to the
script) in Excel, Numbers or R. The first rows look like this:

| file | vowel | duration_ms | F1 | F2 |
|---|---|---:|---:|---:|
| F1_had | æ | 279 | 758 | 1680 |
| F1_head | ɛ | 253 | 670 | 1809 |
| F1_heed | iː | 238 | 355 | 2423 |
| F1_hid | ɪ | 212 | 486 | 1973 |

…and 12 more rows.

<details markdown="1">
<summary><strong>Show the full script</strong></summary>

```praat
# ------------------------------------------------------------
# 04_batch_vowels.praat  -  Exercise 3 (the payoff)
# For EVERY .wav file in ../audio/ (with a matching .TextGrid):
#   find the vowel on the "phone" tier and measure
#   its duration, and F1 and F2 at the vowel midpoint.
# Results are saved to vowels.csv next to this script.
# ------------------------------------------------------------

folder$ = "../audio/"
outFile$ = "vowels.csv"

# Start a fresh CSV file with a header row
# Our labels are IPA, so make sure the CSV is saved as UTF-8
# (otherwise Praat silently switches to UTF-16, which confuses Excel and R)
Text writing preferences: "UTF-8"
writeFileLine: outFile$, "file,speaker,vowel,duration_ms,F1,F2"

# Make a list of all the .wav files in the folder
fileList = Create Strings as file list: "files", folder$ + "*.wav"
nFiles = Get number of strings

for f from 1 to nFiles
    selectObject: fileList
    fileName$ = Get string: f
    name$ = fileName$ - ".wav"
    speaker$ = left$ (name$, 2)

    sound = Read from file: folder$ + fileName$
    textgrid = Read from file: folder$ + name$ + ".TextGrid"

    # Formant analysis for the whole file
    selectObject: sound
    formant = To Formant (burg): 0, 5, 5500, 0.025, 50

    # Walk through the phone tier and keep only the vowel
    selectObject: textgrid
    nIntervals = Get number of intervals: 2
    for i from 1 to nIntervals
        selectObject: textgrid
        label$ = Get label of interval: 2, i
        if label$ <> "" and label$ <> "h" and label$ <> "d"
            start = Get start time of interval: 2, i
            end = Get end time of interval: 2, i
            mid = (start + end) / 2
            selectObject: formant
            f1 = Get value at time: 1, mid, "hertz", "linear"
            f2 = Get value at time: 2, mid, "hertz", "linear"
            appendFileLine: outFile$, name$, ",", speaker$, ",", label$, ",",
            ... fixed$ ((end - start) * 1000, 0), ",", fixed$ (f1, 0), ",", fixed$ (f2, 0)
        endif
    endfor

    # Clean up before the next file
    removeObject: sound, textgrid, formant
endfor

removeObject: fileList
writeInfoLine: "Done! Measured ", nFiles, " files. Open ", outFile$, " in Excel or R."

# LEVEL UP
# a) Use a lower formant ceiling (5000 Hz) for the male speaker (M1).
#    Hint: if speaker$ = "M1" ... else ... endif
# b) Add a column with the mean F0 of the vowel.
#    Hint: To Pitch (filtered autocorrelation)... then Get mean: start, end, "Hertz"
# c) Add a form at the top so the user can choose the folder.
```

</details>

**Level up.**
1. Use a 5000 Hz formant ceiling for the male voice (M1).
2. Add a column with the mean F0 of each vowel.
3. Add a form so users can choose the folder.

<details markdown="1">
<summary><strong>Show the solution</strong></summary>

```praat
# Exercise 3 - level up: a form, speaker-specific formant ceiling, mean F0

form Measure vowels in a folder
    sentence Folder ../audio/
    word Output_file vowels_levelup.csv
endform

# Our labels are IPA, so make sure the CSV is saved as UTF-8
# (otherwise Praat silently switches to UTF-16, which confuses Excel and R)
Text writing preferences: "UTF-8"
writeFileLine: output_file$, "file,speaker,vowel,duration_ms,F1,F2,meanF0"

fileList = Create Strings as file list: "files", folder$ + "*.wav"
nFiles = Get number of strings

for f from 1 to nFiles
    selectObject: fileList
    fileName$ = Get string: f
    name$ = fileName$ - ".wav"
    speaker$ = left$ (name$, 2)

    sound = Read from file: folder$ + fileName$
    textgrid = Read from file: folder$ + name$ + ".TextGrid"

    # (a) lower formant ceiling for the male voice
    if speaker$ = "M1"
        ceiling = 5000
    else
        ceiling = 5500
    endif
    selectObject: sound
    formant = To Formant (burg): 0, 5, ceiling, 0.025, 50

    # (b) pitch: filtered autocorrelation is the preferred method in recent Praat;
    #     older versions fall back to the classic command
    selectObject: sound
    if praatVersion >= 6444
        pitch = To Pitch (filtered autocorrelation): 0, 50, 800, 15, "no", 0.03, 0.09, 0.5, 0.055, 0.35, 0.14
    else
        pitch = To Pitch: 0, 75, 600
    endif

    selectObject: textgrid
    nIntervals = Get number of intervals: 2
    for i from 1 to nIntervals
        selectObject: textgrid
        label$ = Get label of interval: 2, i
        if label$ <> "" and label$ <> "h" and label$ <> "d"
            start = Get start time of interval: 2, i
            end = Get end time of interval: 2, i
            mid = (start + end) / 2
            selectObject: formant
            f1 = Get value at time: 1, mid, "hertz", "linear"
            f2 = Get value at time: 2, mid, "hertz", "linear"
            selectObject: pitch
            f0 = Get mean: start, end, "Hertz"
            appendFileLine: output_file$, name$, ",", speaker$, ",", label$, ",",
            ... fixed$ ((end - start) * 1000, 0), ",", fixed$ (f1, 0), ",", fixed$ (f2, 0), ",", fixed$ (f0, 1)
        endif
    endfor

    removeObject: sound, textgrid, formant, pitch
endfor

removeObject: fileList
writeInfoLine: "Done! Measured ", nFiles, " files. Results in ", output_file$
```

</details>

### Sanity check: something is off
These are the male voice's results from the Core script. One of these rows can't be right. Which one, and why?

| file | vowel | F1 | F2 |
|---|---|---:|---:|
| M1_had | æ | 629 | 1552 |
| M1_head | ɛ | 557 | 1669 |
| M1_heed | iː | 274 | 2223 |
| M1_hid | ɪ | 395 | 1855 |
| M1_hod | ɑː | 713 | 1156 |
| M1_hood | ʊ | 490 | 1014 |
| M1_hud | ʌ | 592 | 1266 |
| M1_whod | uː | 1297 | 2167 |

<details markdown="1">
<summary><strong>Show the answer</strong></summary>

**M1_whod.** An F1 of 1297 Hz is impossible for [uː], a high back vowel that should have a low F1 (compare
M1_heed: 274 Hz). The default formant ceiling of 5500 Hz suits an average adult female voice; for an average
adult male voice the Praat manual suggests about 5000 Hz. With a ceiling that's too high, Praat can miss or
merge the low formants of [u]. Rerun with 5000 Hz (Level up task 1) and you get F1 = 335 Hz, F2 = 1318 Hz.

Scripts are fast, not infallible: a script makes the same mistake a thousand times very quickly. Always look
at your numbers, ideally in a plot.

</details>

## 6. Wrap-up

### When Praat complains

| Praat says… | It usually means… | Try this |
|---|---|---|
| `Command "To Formant (burg):" not available for current selection` | The wrong object is selected | Add `selectObject: sound` before the command |
| `Found a string expression instead of a numeric expression` | Text stored in a variable without `$` | `name$ = …` |
| `Cannot open file "…/F1_hee.wav"` | A typo in the path, or the script isn't saved | Check the name; save the script in `scripts/` |
| `Unknown variable` | A typo or wrong capitalization (`f1` is not `F1`) | Check the spelling and where the variable is created |
| `Command "Duration = 3" not recognized` | Variable names must start with a lowercase letter | `duration = 3` |

### Key takeaway
> Every click is a command. Write it down once, and Praat will repeat it for 16 files or 16,000. Then look at
> your numbers before you trust them.

## Resources
- [Praat scripting tutorial](https://www.fon.hum.uva.nl/praat/manual/Scripting.html): the official
step-by-step guide in the Praat manual.
- [Using Praat for Linguistic Research](https://wstyler.ucsd.edu/praat/) (Will Styler): a free guide to Praat
by hand, with a long scripting chapter.
- [Sound: To Formant (burg)…](https://www.fon.hum.uva.nl/praat/manual/Sound__To_Formant__burg____.html):
what each formant setting means, including the 5500 vs. 5000 Hz ceiling.
- Jadoul, Y., Thompson, B., & de Boer, B. (2018). Introducing Parselmouth: A Python interface to Praat.
*Journal of Phonetics, 71*, 1–15. [https://doi.org/10.1016/j.wocn.2018.07.001](https://doi.org/10.1016/j.wocn.2018.07.001)
- Boersma, P., & Weenink, D. (2026). *Praat: Doing phonetics by computer* (Version 7.0.03) [Computer program].
[https://www.praat.org](https://www.praat.org)
