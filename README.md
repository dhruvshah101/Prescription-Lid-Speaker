# Prescription Lid Speaker 💊🔊

**An improved prescription lid equipped with a speaker to read medication information aloud at the press of a button, making it more accessible to older adults and visually impaired users who struggle to read standard prescription labels.**

> 🎓 PLTW Engineering Design & Development project  
> **GitHub:** [Dhruv Shah](https://github.com/dhruvshah101) · **Team:** Dhruv, Krish, Nick, Omer

---

## The Problem

Prescription bottles place important information—medication names, dosages, expiration dates, and warnings—on small, densely printed labels. For people with declining vision or difficulty understanding the language on a label, accessing that information can be challenging.

Our team explored this problem through consumer interviews, surveys, research, and an analysis of existing solutions. Recurring concerns included:

- Small text and difficulty reading labels in poor lighting.
- Confusion between similar-looking medications and bottles.
- Language barriers and reliance on memory or assistance.
- The cost, accessibility, and technical familiarity required by some existing solutions.

## The Solution

A replacement bottle lid with a single button and a built-in speaker. Pressing the button plays a prerecorded audio clip stored on a microSD card, allowing medication information to be heard aloud.

The prototype uses a **Seeed Studio XIAO RP2040**, a **DFPlayer Mini**, and an **RGB status LED** inside a custom enclosure. Playback runs locally, without an app or an internet connection. Audio must be prepared and loaded onto the card in advance.

## How It Works

```text
[ Button ] → [ XIAO RP2040 ] → [ DFPlayer Mini ] → [ Speaker ]
                    │                  ↑
                    ↓             [ microSD ]
           [ RGB Status LED ]
```

1. Prerecorded audio clips are stored on the microSD card.
2. A short button press tells the RP2040 to play the next track.
3. The DFPlayer Mini reads the audio file and drives the speaker.
4. The RGB LED provides visual feedback during startup, playback, and errors.
5. Holding the button for two seconds stops playback.

| Button action | Result |
| --- | --- |
| Short press and release | Play the next track; wrap back to the first after the last |
| Long press (2 seconds) | Stop playback |


## Materials Utilized

| Component | Purpose |
| --- | --- |
| Seeed Studio XIAO RP2040 | Microcontroller; handles button input and playback commands |
| DFPlayer Mini MP3 module | Plays audio stored on the microSD card |
| Speaker | Audio output |
| Momentary push button | Single-button user input |
| RGB status LED | Visual feedback; the firmware uses the board's LED pin definitions |
| microSD card | Stores prerecorded audio clips |
| 3D-printed enclosure | Houses the electronics in the bottle-lid assembly |
| Wiring and solder | Connects the circuit components |

## Firmware

The firmware is written in **C++ for the Arduino framework** and uses the **DFRobotDFPlayerMini** library.

It includes:

- **Button debouncing:** 50 ms to filter switch noise.
- **Short-press control:** advances through the available tracks on release.
- **Long-press control:** stops playback after a two-second hold.
- **Playback feedback:** alternates blue and green every 500 ms while playing.
- **Event handling:** clears the playback state when a track finishes and indicates reported errors.
- **Startup diagnostics:** reports DFPlayer initialization status and the detected track count over USB serial.

| Setting | Value in the supplied firmware |
| --- | --- |
| Button pin | `D1`, using `INPUT_PULLUP` |
| Audio module connection | `Serial1` at 9600 baud |
| USB serial diagnostics | 115200 baud |
| LED pins | `PIN_LED_R`, `PIN_LED_G`, `PIN_LED_B` |
| LED polarity | Active-low |
| Playback volume | 25 on a 0–30 scale |
| Equalizer | Normal |
| Audio source | microSD card |

The supplied code plays existing recordings; it does not scan labels, generate speech, or record audio. The startup green flashes indicate that the initialization sequence completed, rather than confirming that every audio file is playable.

## Repository Structure

Suggested layout for the project files:

```text
Prescription-Lid-Speaker/
├── README.md
├── code/
│   └── Prescription-Lid-Speaker/
│       └── arduino_code.ino
└── docs/
│   └── Research_Documentation.pdf
    └── Prototype Development Documentation.pdf


```

## Engineering Process

Our team developed the prototype through the PLTW engineering design process:

- **Problem identification:** investigated prescription-label readability through research, interviews, and surveys.
- **Existing-solution analysis:** reviewed pillboxes, caregiver assistance, accessible labels, RFID systems, and related patents.
- **Design criteria:** considered accessibility, cost, usability, efficiency, and development time.
- **Concept selection:** evaluated 20 potential solutions using a decision matrix and narrowed the options to a final concept.
- **CAD and circuit planning:** developed enclosure designs in Onshape and explored the circuit in Tinkercad.
- **Prototype construction:** assembled the electronics, soldered connections, and integrated the circuit into a 3D-printed housing.
- **Iteration:** revised wiring, replaced the audio module, and added speaker openings to improve the prototype.

### My Contribution

As part of the team, I conducted academic and market research to identify and justify the problem's needs. For prototype development, I researched viable components, designed, and built a circuit utilizing the parts mentioned above. 

## Lessons Learned

- **Check voltage compatibility early.** The team documented a mismatch between the intended 5 V connections and the RP2040's 3.3 V logic requirements, which required changes to the wiring.
- **Verify component variants before integration.** We replaced the initial DFPlayer Mini module after identifying that it was the incorrect version for our build.
- **Design the enclosure around audio output.** Small openings above the speaker helped sound travel out of the housing.
- **Allow time for soldering and rework.** Broken connections and soldering mistakes took time to diagnose and repair.
- **Keep the interface simple.** A single button supports both track selection and stopping playback without requiring a screen or app.


## Engineering Portfolio

Further documentation, pictures, information and coursework from my engineering courses can be accessed [here](https://sites.google.com/view/dhruvshah/capstone?authuser=0)


## Acknowledgments

Thanks to **Mr. Hamrani** for materials and 3D-printing support, and to **Ms. Layachi** for circuitry support and the peers who helped the team with circuit connections and prototype construction. Thank you to the interview participants and survey respondents who helped shape our understanding of the problem.

---

*This project is an academic engineering prototype. It does not replace professional medical guidance or the original prescription label.*
