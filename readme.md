# Synth Distortion / Saturation Module Design

## Some background.

I’m designing a distortion/saturation modular synth module using different methods of distorting audio signals.

## Source of inspiration

### DIYRe’s Colour Modules

I got some ideas from DIYRe's Colour modules. DIYRe is a company that produces DIY kits for professional audio recording equipment. Some of their product offerings include recording studio rackmount effects.

[The Colour Format](https://www.diyrecordingequipment.com/pages/the-colour-format)

| Colour Module | What does it do? | What ideas did you get? | Link to page | Schematic |
| --- | --- | --- | --- | --- |
| CTX Transformer Colour | A transformer saturation module with a discrete opamp and a Cinemag audio transformer which has a 1:1 turns ratio. | Non-inverting amp to drive the transformer past what it can handle. Resistor before the transformer that isolates the opamp from the transformer. | https://www.diyrecordingequipment.com/products/ctx-cinemag-transformer-colour-mkii | https://cdn.shopify.com/s/files/1/0698/2265/files/CTX_mkII_Schematic.pdf?375 |
| Colourphone | A telephone sound emulator. It uses a bandpass filter and a 1:1 turn ratio audio transformer. | I have a similar transformer which is a 600:600 ohm transformer. Resistor before the transformer that isolates it from the opamp. | https://www.diyrecordingequipment.com/products/colourphone-telephone-distortion-colour | https://cdn.shopify.com/s/files/1/0698/2265/files/XQP_Colourphone_Schematic_1.3.pdf?8973696934571457842 |
| Colourupter | An opto distortion module that is vactrol based. | Using a vactrol and the circuit around it. IC1c and IC1d only. | https://www.diyrecordingequipment.com/products/colourupter | https://cdn.shopify.com/s/files/1/0698/2265/files/XQP_Colourupter_Schematic_1.7.pdf?13355729951948932441 |

### Other sources

- Eliot Sound Products [https://sound-au.com](https://sound-au.com/) - is a website full of audio articles that I’ve learned from
    - Designing With Op Amps
    - Audio Transformers
    - Soft Clipping
- Automatic gain amplifier
    - [Vactrol auto gain op amp](https://www.youtube.com/watch?v=Cg2cLocjgGQ)
- Vactrol wet/dry mix from music thing modular’s spring reverb module
    - [https://www.musicthing.co.uk/Spring-Reverb/](https://www.musicthing.co.uk/Spring-Reverb/)

# Circuit Design

### Stages

1. Input gain stage (buffer only, see design note below)
    
    ![Untitled](readme_images/Untitled.png)
    
    We must take into account the headroom of the op amp. The NE5532 can only output 2 volts less than the supply voltage. So for example, your supply voltage would be +/- 12V, your max output would be 10V based on the image below.
    
    ![Untitled](readme_images/Untitled%201.png)
    
    A buffer would suffice as the input stage. We wouldn’t be able to amplify the 10Vpp input to 12Vpp unless we use a +/- 15V supply. 
    
2. Transformer Saturation
    
    ![Untitled](readme_images/Untitled%202.png)
    
3. Voltage controlled Vactrol Wet/Dry mix
    
    I used my handmade Vactrol. I made it using a 5mm diffused red LED and a 5mm LDR enclosed with black heat shrink tubing. I crimped the ends so no light would leak in. 47k works for R13 with my Vactrol.
    
    ![Untitled](readme_images/Untitled%203.png)
    
4. Vactrol Opto Electronic Saturation (To Test)
    
    ![Untitled](readme_images/Untitled%204.png)
    
5. Soft Clipping Diode Saturation (To Test)
    
    ![Untitled](readme_images/Untitled%205.png)
    
6. Output gain stage (not yet tested)
    
    ![Untitled](readme_images/Untitled%206.png)
    

## Some circuit housekeeping todo

- Reverse polarity protection
- I/O protection
- Unused opamps
- Power filter caps
- Audio decoupling if necessary

Version Todo and Notes:

- Version 0.2
    - TODO
        - [x]  Replace R5 with trimmer
        - [x]  Test wet/dry mix on breadboard
        - [x]  Input buffer biasing resistor
    - Notes
        - The transformer saturation is very subtle on a mono synth
- Version 0.3
    - TODO
    - [ ]  Add gain trimmer to dry signal at U2B
    - [ ]  Scrap the single saturation and add diode clipping and the Colourupter, the transformer alone is too subtle
    - [ ]  Test output gain stage