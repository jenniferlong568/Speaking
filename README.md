# Whose Words?

An interactive piece about the verb *augment*. The viewer answers one question with a single switch (the spacebar). A scripted "AI" offers to finish their words and sentences. Every letter remembers who wrote it: the learner's letters are solid black, and the AI's letters are hollow or grey.

## How to run it

- **Easiest:** double-click `index.html`. It opens in your browser and works offline (it falls back to a system font if Atkinson Hyperlegible can't load).
- **If the camera won't start** (some browsers block cameras on files opened directly): in Terminal, `cd` into this folder, run `python3 -m http.server`, then open http://localhost:8000.
- Chrome is the most reliable for the voice and the camera. For a presentation, press F11 (Windows) or Ctrl+Cmd+F (Mac) to go full screen.

## Controls

| Key | What it does |
|---|---|
| Space | The switch. On the intro: begin. While writing: open the highlighted row, then choose the highlighted key. On the end screen: try again (after 2.5 s). |
| ← ↓ / → ↑ | Move the Augmentation slider down / up (the mouse works too). |
| Mouse | The hollow/grey toggle, the camera button, and Try again / Export on the end screen. |

## The flow

Intro → write an answer → **SPEAK** (or stop 4 speaks for you) → end screen with the sentence, the authorship bar, time and selections → **Export** saves a PNG → **Try again** resets.

## CONFIG

All the tunable values are in one `CONFIG` object at the top of the `<script>` block in `index.html`. Save the file and reload the browser to see a change.

**Switch scanning**
- `scanSpeed`: how long (ms) the highlight stays on each row or key. Higher is slower and easier.
- `itemPasses`: how many times the highlight goes across an open row before giving up and going back to rows.

**Look**
- `colors.background`, `colors.lines`, `colors.uiText`: page, borders, UI text.
- `colors.learner`: the learner's letters. Keep it the only fully black thing.
- `colors.aiStroke`, `aiStrokeWidth`: the outline of hollow AI letters.
- `colors.aiGrey`: AI letters in grey mode.
- `colors.highlight`, `colors.highlightItem`, `colors.focusLine`: the scanning highlight and the dwell ring.
- `sentenceSize`: size of the sentence being written. It scales with the screen, and very long sentences shrink to fit.
- `aiLetters`: whether AI letters start `"hollow"` or `"grey"`.

**Words on screen**
- `question`, `introTitle`, `introLine`, `sentLine` ("Sent to teacher."), `closingLine`: change any text here and it updates everywhere, including the PNG export.

**Letter board**
- `boardLayout`: one list per row. `SPACE`, `DELETE` and `SPEAK` are commands; anything else is typed as itself. The prediction row is added above automatically.

**Augmentation slider**
- `levelNames`: labels for stops 0 to 4.
- `startLevel`: where the slider starts when the page opens and after Try again.
- `lettersAhead`: stop 1, how many next characters the single suggestion offers.
- `wordsAtStop2`: stop 2, how many word completions.
- `sentencesAtStop3`, `wordsAtStop3`: stops 3 and 4, how many sentences and words.

**Stop 4: "Speaks for you"**
- `takeoverAfterLetters`: how many letters the learner picks before the system takes over.
- `autoSentence`: the sentence the system writes and says for them.
- `takeoverDelay`: pause after the learner's last letter before the takeover.
- `takeoverTypeSpeed`: how fast (ms per character) the system types it.
- `endAfterSent`: how long "Sent to teacher." shows before the end screen.

**End screen**
- `endSpaceLock`: how long the spacebar is ignored, so a stray press can't wipe the result while people read it.

**Camera input** (`gaze`, only used after "Use camera instead of switch")
- `dwellTime`: how long to face a zone to select it.
- `noFaceAfter`: how long without a face before "No learner detected" and the switch board returns.
- `backAfter`: how long inside a row with no choice before going back to the top level.
- `afterSelectPause`: extra wait before the same zone can be chosen again.
- `yawThreshold`, `pitchThreshold`: how far to turn or tilt your head to change zone. Raise them if zones change too easily, lower them if you have to turn too far.
- `calibrationFrames`: how many frames count as "straight ahead" when the camera starts (about 1 second). Hold still then.
- `mediapipeUrl`, `mediapipeWasm`, `faceModelUrl`: where MediaPipe loads from (needs internet the first time).

**The "AI"**
- `vocabulary`: about 150 words used for word completions.
- `phrases`: about 30 full answers used for sentence completions.
- In both lists, earlier means "more likely". Reorder them to change what the system pushes first.

## Privacy

The camera only starts when you click the button. Face tracking runs inside the browser. No video, images or text are recorded or sent anywhere.
