# Synth Distortion / Saturation Module Design

![Multi Saturation assembled](photos/front.jpg)

## Revision Summary

- Schematic Revision 0.5 - an easier to read schematic - please note that there are some errors in this. The Eurorack power connector pins are backwards. Use this as a reference for the circuit design.
- Schematic Revision 0.6 - multiboard layout (LATEST - working on circuit revisions)
- PCB Revision 0.1 - revising in progress (working on rev 0.2)

## Some background.

I’m designing a distortion/saturation modular synth module using different methods of distorting audio signals.

This module is a combination of transformer, opto, and clipping distortion/saturation.

## Source of inspiration

Local archived copies of the borrowed-source schematics live in [`references/`](references/) so this repo stays useful if the upstream links die. Originals from [diyrecordingequipment.com](https://www.diyrecordingequipment.com/pages/the-colour-format).

- **CTX Cinemag Transformer Colour mkII** — [local copy](references/DIYRE-CTX-Cinemag-mkII-schematic.pdf) · [upstream](https://cdn.shopify.com/s/files/1/0698/2265/files/CTX_mkII_Schematic.pdf?375)
- **Colourphone (v1.3)** — [local copy](references/DIYRE-Colourphone-schematic-v1.3.pdf) · [upstream](https://cdn.shopify.com/s/files/1/0698/2265/files/XQP_Colourphone_Schematic_1.3.pdf?8973696934571457842)
- **Colourupter (v1.7)** — [local copy](references/DIYRE-Colourupter-schematic-v1.7.pdf) · [upstream](https://cdn.shopify.com/s/files/1/0698/2265/files/XQP_Colourupter_Schematic_1.7.pdf?13355729951948932441)

### DIYRe’s Colour Modules

I got some ideas from DIYRe's Colour modules. DIYRe is a company that produces DIY kits for professional audio recording equipment. Some of their product offerings include recording studio rackmount effects.

[The Colour Format](https://www.diyrecordingequipment.com/pages/the-colour-format)

| Colour Module | What does it do? | What ideas did you get? | Link to page | Schematic |
| --- | --- | --- | --- | --- |
| CTX Transformer Colour | A transformer saturation module with a discrete opamp and a Cinemag audio transformer which has a 1:1 turns ratio. | Non-inverting amp to drive the transformer past what it can handle. Resistor before the transformer that isolates the opamp from the transformer. | https://www.diyrecordingequipment.com/products/ctx-cinemag-transformer-colour-mkii | https://cdn.shopify.com/s/files/1/0698/2265/files/CTX_mkII_Schematic.pdf?375 |
| Colourphone | A telephone sound emulator. It uses a bandpass filter and a 1:1 turn ratio audio transformer. | I have a similar transformer which is a 600:600 ohm transformer. Resistor before the transformer that isolates it from the opamp. | https://www.diyrecordingequipment.com/products/colourphone-telephone-distortion-colour | https://cdn.shopify.com/s/files/1/0698/2265/files/XQP_Colourphone_Schematic_1.3.pdf?8973696934571457842 |
| Colourupter | An opto distortion module that is vactrol based. | Using a vactrol and the circuit around it. IC1c and IC1d only. | https://www.diyrecordingequipment.com/products/colourupter | https://cdn.shopify.com/s/files/1/0698/2265/files/XQP_Colourupter_Schematic_1.7.pdf?13355729951948932441 |

### Other sources

- Vactrol wet/dry mix from music thing modular’s spring reverb module
    - [https://www.musicthing.co.uk/Spring-Reverb/](https://www.musicthing.co.uk/Spring-Reverb/)
- Eliot Sound Products [https://sound-au.com](https://sound-au.com/) - is a website full of audio articles that I’ve learned from. Go search for these articles, they’re great!
    - Designing With Op Amps
    - Audio Transformers
    - Soft Clipping

# Circuit Design

- I will update this section soon.

![Untitled](readme_images/Untitled.png)

### Stages

1. Input gain stage (buffer only, see design note below)
    
    ![Untitled](readme_images/Untitled%201.png)
    
    We must take into account the headroom of the op amp. The NE5532 can only output 2 volts less than the supply voltage. So for example, your supply voltage would be +/- 12V, your max output would be 10V based on the image below.
    
    ![Untitled](readme_images/Untitled%202.png)
    
    A buffer would suffice as the input stage. It would be nice to give the transformer more voltage but we wouldn’t be able to amplify the 10Vpp input to 12Vpp unless we use a +/- 15V supply. 
    
    I chose to use an NE5532 Op Amp because I believe it can drive outputs higher than TL07x.
    
2. Transformer Saturation
    
    The transformer distortion works by oversaturating the transformer’s core past what it can handle. The transformer I used would normally be used in line audio signals. A 600:600 ohm transformer. We’re using Eurorack voltage which is around 10vpp so that would oversaturate the transformer.
    
    ![Untitled](readme_images/Untitled%203.png)
    
3. Vactrol Opto Electronic Saturation
    
    The transformer’s output was low, I needed to add a non-inverting op amp with gain to bring the levels back to 10vpp for this stage.
    
    I got this circuit from DIYRe’s Colourupter colour module which uses a Vactrol to clip the signal.
    
    ![Untitled](readme_images/Untitled%204.png)
    
4. Soft Clipping Diode Saturation
    
    I chose to place these diodes after the wet amplifier. It would be softer if I placed it before the U4A. I chained LEDs to the 1N4148s to get a softer clipping effect. LEDs alone would make saw waves looking like square waves.
    
    ![Untitled](readme_images/Untitled%205.png)
    
5. Voltage controlled Vactrol Wet/Dry mix
    
    I used my handmade Vactrol. I made it using a 5mm diffused red LED and a 5mm LDR enclosed with black heat shrink tubing. I crimped the ends so no light would leak in. 47k works for R17 with my Vactrol.
    
    ![Untitled](readme_images/Untitled%206.png)
    

## Schematic Revisions:

- Revision 0.2
    - TODO
        - [x]  Replace R5 with trimmer
        - [x]  Test wet/dry mix on breadboard
        - [x]  Input buffer biasing resistor
    - Notes
        - The transformer saturation is very subtle on a mono synth
- Revision 0.3
    - TODO
    - [x]  Add gain trimmer to dry signal at U2B
    - [x]  Scrap the single saturation and add diode clipping and the Colourupter, the transformer alone is too subtle
    - [x]  Test output gain stage
- Revision 0.4
    - [x]  Scrap output auto gain
    - [x]  add opto saturation
    - [x]  add soft clipping diodes
- Revision 0.5
    - [x]  Multiboard layout
- Revision 0.6
    - [ ]  Fix amplitude difference on opto sat stage output.

# PCB Design

![Dec 27, 2023 - Almost satisfied  with the board design, rev0.1](readme_images/Untitled%207.png)

Dec 27, 2023 - Almost satisfied  with the board design, rev0.1

![Rev 0.1 in black](readme_images/Untitled%208.png)

Rev 0.1 in black

## PCB revisions:

- Todo for Revision 0.1
    - [x]  Ground plane vias
    - [x]  Front panel ground plane
    - [x]  Double check hole sizes
        - [x]  Trimmer
    - [x]  Double check via sizes
    - [x]  Round tracks
- Todo for Revision 0.2
    - Front panel
        - [x]  Add keep out zone for led window zone fills for FCU and BCU - I had to Dremel it out for the revision 0.1 front panel board
        - [x]  Enlarge switch holes for Dailywell switches
        - [x]  Enlarge trimmer holes
        - [x]  Change tilt knob silkscreen to “mix”
        - [ ]  Fix amplitude difference on opto sat output. The opto stage attenuates the signal from the transformer too much
- Revision 0.1 Board test notes
    - Voltage tests
        - Power rails after reverse protection diode I used a 1N4007 diode
            - V+ reads about 10V
            - V- reads about 11V
            - I might need to change the reverse protection diodes to Schottkys
    - Music thing modular’s cv wet dry circuit has an optimal input of 0-5V control voltages. If 10Vpp was used, the middle of the cv level pot is zero. Fully clockwise, the cv would be in phase. Fully counter clockwise, the cv is inverted. This is simulated in falstad, the file is included in the falstad folder.
    - Gain trim is finicky. Turning on the Opto would attenuate the signal. Maybe this is caused by the variety of the vactrol, I think not all vactrols are equal. Opto gain/amplitude trim might be required. It might be a simple resistor value change within the circuit.
    - LEDs are not as bright as I want them.

## Circuit walkthrough

Three saturation stages in series, each individually bypassable from the front panel, then a CV-controlled wet/dry mix:

```
Audio In ─► U1A buffer ─┬─► [Dry path → wet/dry mixer U4B]
                        │
                        └─► SW1 (Xformer bypass) ─► [TL016 transformer, 680Ω matching, HPF 15Hz]
                                                  ─► U1B buffer ─► transformer-saturated signal
                                                                     │
                                                                     ▼
                                                  SW2 (Opto bypass) ─► U2A buffer ─► C2 (1µF) ─► U2B inverting amp (gain −68)
                                                                                                    ├─► drives U3 vactrol LED (R10 limits)
                                                                                                    └─► AC-coupled out as "Wet"
                                                                                                                                ▼
                                              U4A amp ────────► clipping LEDs + 1N4148 (SW1 on Board B bypasses)
   Dry signal ──► U4B amp ───────────────────┘                                                              │
                                                                                                            ▼
                                                                                  U6 vactrol (CV wet/dry mixer, driven by Mix CV via U5A/U5B)
                                                                                                            ▼
                                                                                                       Mix Audio Out
```

### Three saturation flavors, switchable

**1. Transformer saturation (TL016-R, 600:600Ω).** 680Ω resistors match the transformer's characteristic impedance. Driving the primary hard pushes the magnetic core into non-linearity — classic warm "tape-like" harmonic content (mostly odd harmonics). The 15Hz HPF (C1 + R7) on the secondary removes any DC bias.

**2. Opto-electronic saturation (vactrol, U3).** U2B is a high-gain inverting amp (R8/R6 = 680/10 = **−68× gain**) driving the vactrol's LED. As the signal gets louder, the LED brightens, the LDR resistance drops, more signal shunts to ground — soft compression / saturation. Fast attack/release due to the vactrol's photo response.

**3. Diode soft clipping (Board B).** LEDs (D1, D2) in series with 1N4148s (D3, D4) — asymmetric soft clipping with the LEDs adding ~2V of softness before the 4148s contribute hard clip. Pleasant warmth without harsh square-wave look. SW1 on Board B bypasses.

### Wet/Dry mix (CV-controlled)

Borrowed from Music Thing Modular's Spring reverb topology. Second vactrol (U6) acts as a CV-controlled variable resistor in the mix path. Mix CV input → U5A → U5B → vactrol LED. The Mix Trim pot (RV4, 50K) shifts the CV center point by ±12V so the vactrol's response curve aligns with your CV source (the Music Thing original optimizes for 0–5V CV; this tuning lets it handle ±5V or other ranges).

### Calibration

Three trim pots accessible from the front panel:

**RV2 — Wet gain trim.** Sets U4A's amplification of the wet (saturated) signal. Goal: match the wet signal's amplitude to the dry signal so the mix knob crossfades cleanly.

**RV3 — Dry gain trim.** Sets U4B's amplification of the dry signal. Adjust together with RV2 until at the mix midpoint, the output amplitude doesn't jump as you switch saturation stages on/off.

**RV4 — Mix trim.** Shifts the Mix CV bias up or down ±12V. Adjust until the panel mix pot's full clockwise position gives 100% wet and full counter-clockwise gives 100% dry, with your CV source connected. With no CV connected (or 0V), midpoint should give 50/50.

Procedure:
1. Patch a steady audio signal (e.g., a sawtooth from a VCO) into Audio In.
2. Set all bypass switches to "off" (no saturation) — only the dry path is active.
3. Patch a scope on Mix Audio Out. Note the amplitude.
4. Switch in transformer saturation (SW1). Adjust RV2 (wet gain) so the amplitude is similar to dry.
5. Switch in opto saturation (SW2). If the amplitude drops dramatically here, see [issue #1](https://github.com/DIYSynthMNL/Eurorack-Multi-Saturation-Module/issues) for the known opto-sat fix.
6. With a 0–5V LFO into Mix CV, adjust RV4 (mix trim) so panel mix pot fully clockwise = wet, fully counter-clockwise = dry.

## Build status

What's ready for builders today, and what's still on the TODO list:

**Production assets** (what you need to actually fabricate and assemble a final unit)

- [x] Schematic — Rev 0.6 ([Single saturation module - Multiboard Schematic - rev0.6.pdf](Schematic%20PDFs/Single%20saturation%20module%20-%20Multiboard%20Schematic%20-%20rev0.6.pdf))
- [x] PCB layout — rev 0.1 separated for fab in `kicad/Separate boards/rev 0.1/`; rev 0.2 fixes pending (see PCB revisions in readme)
- [ ] Gerber files for fabrication — none yet
- [x] BOM — [ibom.html](BOMs/ibom.html)
- [x] Final front panel (SVG/PDF for fab) — [MultiSaturation_FPANEL_FOR_KICAD_IMPORT.svg](Front%20panel%20graphics/MultiSaturation_FPANEL_FOR_KICAD_IMPORT.svg)
- [x] License — [LICENSE](LICENSE)

**Prototype assets** (for breadboard / perfboard / 3D-printed-panel builds before final PCB)

- [ ] 3D-printed prototype panel STL — none yet

**Documentation**

- [x] Photos of the assembled module — see [photos/](photos/)
- [ ] Demo video — none yet
- [ ] Build / assembly instructions — none yet
- [ ] Calibration / tuning notes — none yet

Want to help fill a gap (build photos, gerbers, an assembly guide)? Open an issue or PR.
