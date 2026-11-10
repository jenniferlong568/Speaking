# Augmented Keyboard Interaction

An interactive piece about the verb *augment*. The viewer answers one question with a single switch (the spacebar). A scripted "AI" offers to finish their words and sentences. Every letter remembers who wrote it: the learner's letters are solid, and the AI's letters are hollow outlines. As augmentation rises, the screen moves from handwritten paper to a glowing machine, and the voice from robotic to human.

## How to run it

- **Easiest:** double-click `index.html`. It opens in your browser and works offline (it falls back to system fonts if the web fonts can't load).
- **Online:** https://jenniferlong568.github.io/Speaking/ (once GitHub Pages is switched on).
- **If the camera won't start** (some browsers block cameras on files opened directly): in Terminal, `cd` into this folder, run `python3 -m http.server`, then open http://localhost:8000.
- Chrome is the most reliable for the voice and the camera. For a presentation, press F11 (Windows) or Ctrl+Cmd+F (Mac) to go full screen.

## Controls

| Key | What it does |
|---|---|
| Space | The switch. On the intro: begin. While writing: open the highlighted row, then choose the highlighted key. On the end screen: try again (after 2.5 s). |
| ← ↓ / → ↑ | Move the Augmentation slider down / up (the mouse works too). |
| R | Reset: start over at the same level (same as the Reset button). |
| V | Voice check: shows which voice each level found on this computer, with a Test button for each. Press V again to close. |
| Mouse | Reset, the hollow/grey toggle, the camera button, and Try again / Export on the end screen. |

## The flow

Intro → write an answer → **SPEAK** (or level 4 speaks for you) → end screen with the sentence, the authorship bar, time and selections → **Export** saves a PNG → **Try again** or **Reset** starts over at the same level.

## The five levels

| Level | Look and font | What the AI offers | Voice |
|---|---|---|---|
| 0 Off | cream paper, Waiting for the Sunrise | nothing | flat robotic synth, slow, a pause after every word (classic AAC) |
| 1 Letters | white, Patrick Hand | 3 next letters | still synthetic, a little faster, shorter pauses |
| 2 Words | pale blue-grey, Atkinson Hyperlegible | 3 word completions | a clearer standard voice, normal pace, no pauses |
| 3 Sentences | light blue, Inter | after one letter the keyboard goes and 4 sentences that **begin with that letter** take its place | a natural, premium voice (Siri / "natural" voices), warm and polite |
| 4 Speaks for you | dark navy, Space Grotesk, glowing blue | no suggestions; after one letter, writes a formal sentence and speaks it without asking | the most human voice on the computer, slightly lower and self-assured (a different voice from level 3 when one exists) |

The learner's letters are always solid, and the AI's are always hollow outlines (or faded, in "grey" mode). The screen's colours follow the slider, but **every letter keeps the font of the level it was written at**, so a sentence shows how it was made. The authorship bar under the sentence shows the learner's share (solid) against the AI's (outlined), with no numbers.

## CONFIG

All the tunable values are in one `CONFIG` object at the top of the `<script>` block in `index.html`. Save the file and reload the browser to see a change.

**Switch scanning**
- `scanSpeed`: how long (ms) the highlight stays on each row or key. Higher is slower and easier.
- `itemPasses`: how many times the highlight goes across an open row before going back to rows (1 = one pass, so a wrong row costs little).

**Levels** (`levels`, one entry per level, 0 to 4)
- `name`: the label under the slider.
- `font`, `fontScale`: the font letters are written in at that level, and a size tweak so all the fonts look about the same size.
- `background`, `lines`, `uiText`: the page, borders and UI text at that level.
- `learner`: colour of the learner's solid letters (off-white at level 4 so they show on navy).
- `ai`, `glow`: colour of the AI's hollow letters, and an optional glow behind them (`"none"` for no glow).
- `highlight`, `highlightItem`, `focusLine`: the scanning highlight and the dwell ring.
- `voice.prefer`: voice names to look for, in order. The first one this computer has is used, matched loosely ("Ava" finds "Ava (Premium)").
- `voice.rate`, `voice.pitch`: speed and pitch (1 = normal). Low pitch and rate sound more robotic.
- `voice.wordPause`: silence (ms) after every word. 450 at level 0, 180 at level 1, 0 (fluent) from level 2 up.
- `voiceLang`: only voices with this accent are used (`en-US`, American). If the computer has none, any English voice is used.
- `voice.notSameAs`: a level whose voice to avoid, so level 4 doesn't sound the same as level 3.
- `showLevelNames`: show the level names under the Augmentation slider (`false` = no labels).
- `startLevel`: the level when the page first opens.

**Look**
- `aiStrokeWidth`: thickness of the hollow letters' outline.
- `sentenceSize`: size of the sentence being written. It scales with the screen, and very long sentences shrink to fit.
- `aiLetters`: whether AI letters start `"hollow"` or `"grey"`.
- `themeFade`: how long the colours take to change between levels.

**Words on screen**
- `question`, `introTitle`, `introLine`, `sentLine` ("Sent to teacher."): change any text here and it updates everywhere, including the PNG export.

**Letter board**
- `boardLayout`: one list per row. `SPACE`, `DELETE` and `SPEAK` are commands; anything else is typed as itself. The prediction row is added above automatically.

**What the AI offers**
- `lettersAtLevel1`, `wordsAtLevel2`, `sentencesAtLevel3`: how many suggestions each level shows.
- `autocorrectFromLevel`: from this level up (2), a misspelled word is corrected when the learner finishes it (SPACE, SPEAK, or when the AI takes over). Letters they got right stay solid; letters the AI changed or added turn hollow.
- `sentencesAfterLetter`, `level3Sentences`: at level 3, after the first letter the keyboard is replaced by this many sentences. They come from `level3Sentences`, plain everyday answers grouped by first letter, so every sentence starts with the learner's letter ("p" → "Play outside with my friends.", with the "p" solid).
- `letterFrequency`: the letter order level 1 falls back on when the phrases have no guess.

**Level 4: "Speaks for you"**
- `takeoverAfterLetters`: how many letters the learner picks before the system takes over (1).
- `autoSentences`: the formal sentences the system writes for them. If the learner's letter starts a word, the sentence beginning with that letter is used and continues from it ("p" becomes "perhaps I might…"). Otherwise `default` is added after what they wrote.
- `takeoverDelay`: pause after the learner's letter before the takeover.
- `takeoverTypeSpeed`: how fast (ms per character) the system types it.
- `endAfterSent`: how long "Sent to teacher." shows before the end screen.

**End screen**
- `endSpaceLock`: how long the spacebar is ignored, so a stray press can't wipe the result while people read it.

**Camera input** (`gaze`, only used after "Use camera instead of switch")
- `dwellTime`: how long to face a zone to select it (2.5 s).
- `noFaceAfter`: how long without a face before "No learner detected" and the switch board returns.
- `backAfter`: how long inside a row with no choice before going back to the top level (10 s).
- `afterSelectPause`: extra wait before the same zone can be chosen again.
- `yawThreshold`, `pitchThreshold`: how far to turn or tilt your head to change zone. Raise them if zones change too easily, lower them if you have to turn too far.
- `calibrationFrames`: how many frames count as "straight ahead" when the camera starts (about 1 second). Hold still then.
- `mediapipeUrl`, `mediapipeWasm`, `faceModelUrl`: where MediaPipe loads from (needs internet the first time).

**The "AI"**
- `vocabulary`: about 150 words used for word completions.
- `phrases`: about 30 full answers used for letter, word and sentence suggestions.
- In both lists, earlier means "more likely". Reorder them to change what the system pushes first.

## Voices

The voices come from the computer, so they sound different on each machine. Test on the laptop you'll present from. Chrome on a Mac (Samantha, Ava) or Windows 11 ("Natural" voices) gives the best range from robotic to human. Press **V** to see which voice each level is using and to hear each one. If a name in `voice.prefer` isn't found, the browser's default voice is used with that level's rate and pitch.

## Privacy

The camera only starts when you click the button. Face tracking runs inside the browser. No video, images or text are recorded or sent anywhere.
