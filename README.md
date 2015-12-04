# Drumduino

Drumdino is a drum kit implementation using an Arduino shield interfaced with Max/MSP for MIDI sound generation and processing.

![Alt text](https://github.com/nehirakdag/Drumduino/blob/master/Images/Kit.jpg)

##Overview
In essence, an Arduino sketch is used to receive and process any impact received on the six pads. This data is sent to Max/MSP and converted to MIDI notes that can be outputted via the AU DLS Synth or be further processed for many interesting applications! Drumduino can be played with just about any object, not necessarily drum sticks. Its tuning implementation makes it possible for the user play the pads with their hands as if they are percussion instruments, with drumsticks and brushes, or many other creative ways. Drumduino has adjustable sensitivity and delay settings that let the kit achieve optimal responsiveness on any surface, with any method of hitting its pads. 
There were both hardware and software parts to this project, with more emphasis on hardware in relation to producing an optimal sound and the authentic feeling of actually playing the drums. Sensitivity, rebound and types of material to be used in conjunction with Arduino's sensing pins were all factors that needed to be considered before any software implementation, or else the kit (even though not authentic in terms of size & orientation), would not "feel" real when you hit it with a drum stick.



##Background & Objectives
MUMT306 taught me many interesting applications and methods of audio computing throughout this term. A particular one of these tools, the Arduino, was something that I had previous experience dealing with as part of my engineering courses. However, I did not have any knowledge on how to use it in relation to digital audio generation and processing. We had learned how to get the Arduino talking to Max/MSP in class, and my general familiarity with synthesis using Max made me eager to explore more about these ideas. I basically wanted to achieve some possible interesting application of this model of generating sound via Arduino's sensor pins, convert them to MIDI notes in Max, possibly add digital effects or other creative twists along the way.

The first thing I had in my mind was to create a drum sequencer (of the size of the arduino board) that I can attach to an electric guitar and combine the sounds generated by the two, add digital effects to them so that I get something like a "super-instrument". 

| Some interesting things people have done... | ...using my initial idea of a small drum sequencer |
| ------------- | ------------- |
| ![Alt text](https://github.com/nehirakdag/Drumduino/blob/master/Images/example1.jpg)  | ![Alt text](https://github.com/nehirakdag/Drumduino/blob/master/Images/example2.jpg)  |
| [Source] (https://www.youtube.com/watch?v=xkrGyFWPb5o) | [Source] (https://www.youtube.com/watch?v=PnbuOmGOJxA) |



Force-sensing seemed to be a reasonable starting point to focus on. However, being unfamiliar with the myriad parts that can be used with Arduino, I found myself being inclined to use the Piezo Element to achieve the "knock sensing" functionality after my initial research (instead of arcade buttons, for example, in order to have multiple force sensors in a smaller surface area). Once I got a hold of some Piezo elements, the first thing that came to my mind as someone who plays the drums was to actually build an electronic drum kit out of them. Reasons for this include:
  - Acoustic drums are very loud! I wanted to be able to create a series of pads that I can adjust the volume and other aspects with, making it safe to play them at home however I like
  - As a drummer, I always took the whatever the sound that the drums I hit produced for granted (after proper tuning, of course). Now that I know digital processing of sounds, I wanted to rather tweak these sounds and play around with them, producing many possibilities and expanding my horizons in terms of the sound that a drum kit element can produce (cymbals, drums, or even notes)
  - Building a drum kit was a reasonable implementation choice given that I decided to use force sensing resistors as part of this project to create interesting applications with the Arduino
  - The code I would have that would sense "knocks" via the Arduino and send proper messages to Max/MSP for sound generation and processing would be applicable to more than just an electronic drum kit project. So once I finished my drum kit and properly integrated its software, I could easily use the same software (with minor adjustments) to implement my initial project idea of building a small drum sequencer attachable to an electric guitar using arcade buttons as kit elements.

| Piezo Element  | Arcade Button |
| ------------- | ------------- |
| ![Alt text](https://github.com/nehirakdag/Drumduino/blob/master/Images/piezo_element.jpg)  | ![Alt text](https://github.com/nehirakdag/Drumduino/blob/master/Images/arcade_button.jpg)  |

#Design & Implementation
The core foundation of the Drumduino is the Piezo Element, which is basically a circuit element that can be integrated with Arduino, and allows one to build a force sensitive surface. It consists of two oppositely polarized ends soldered to two surfaces, which are connected together. They allow the detection of changes of pressure and converts these changes to electrical charges which can be processed. Continuously reading an analog pin connected to a piezo element results in voltage readings which are proportional to the force applied on the surface containing the element. Note that the piezo element must be parallel connected to a 1M Ohm resistor. Here is the diagram of such a circuit on an Arduino board.

![Alt text](https://github.com/nehirakdag/Drumduino/blob/master/Images/knock_piezo.jpg)

Reading analog pin 0 for this circuit in the main loop function will continually yield voltage values. When there is no pressure applied on the piezo element, the voltage reading will result in readings of 0 (assuming there is no noise). Any force applied will yield non-zero readings, depending on the pressure on the piezo. The great thing about these elements is that they are not necessarily used on their own. Attaching them to a metal surface allows one to expand the surface area that is sensitive to impact. This will be the theoretical approach behind the Drumduino's implementation in terms of hardware.

One must first decide on the approximate size of the surface of each pad will be, so that material can be bought accordingly. Although there are many ways to create the drum pads, it makes sense to build each pad in three layers. The top layer will receive the drumstrokes so must constitute of a mousepad-like surface for authenticity. The middle layer is a metal that the piezo element will attach to. We want to protect this surface from direct contact of any impact applied, and hence are wrapping it around in two other layers. The bottom layer is simply there for stability, shock absorbtion and preventing any damage on the important middle layer; and thus can be any foamlike material, or mousepad again. The goal of creating an authentic pad could only be accomplished by using such material. 

##Procedure
Materials needed:
- x6 Piezo elements
- A somewhat thin metal sheet-like material (will be cut to form all pads - so large surface area)
- Mousepad-like thicker material (will also be cut - x2 surface area of that of previous)
- Electrical wires to extend the piezo elements' ends, complete the circuit
- Soldering Iron and solder (and thus, soldering knowledge)
- Hot glue or electrical tape to connect piezos to metal surfaces
- Heat shrink tubes or electrical tape to protect the soldered joints of piezo elements and wires
- x6 1MegaOhm resistors
- Craft glue or adhesive spray to connect each layer together
- Plywood (or basically any large, flat surface that the six pads and Arduino can reside on once everything is finished)
- Scissors (or knife?) to cut each layer of the pad
- Other complementary tools for general wiring (wire cutter, stripper, plier etc.)
- A print-out schematic of a perfect circle about the desired pad size is also recommended for cutting each layer accurately

After acquiring these materials, one can start the implementation by using the schematic to cut six circles of each layer (or however many number of pads desired, six is the number used for this example). Here is what the bottom and top layer would look like:

![Alt text](https://github.com/nehirakdag/Drumduino/blob/master/Images/Pad_material.jpg)

The middle layer will later be inserted on top of the bottom layer as such. However, we need to attach the piezo element to this surface first.

| What we have  | What we have | What we want |
| ------------- | ------------- | ------------- |
| ![Alt text](https://github.com/nehirakdag/Drumduino/blob/master/Images/piezo_initial.jpg)  | ![Alt text](https://github.com/nehirakdag/Drumduino/blob/master/Images/Aluminum_disc.jpg) | ![Alt text](https://github.com/nehirakdag/Drumduino/blob/master/Images/Piezo_on_disc.jpg) | 

We will want to make six of these. This is where hot glue comes to play, used to stick the piezo element to the metal disc we have. For the best results, the piezo should be stuck as flat as possible. The Piezo will be oriented on the disc such that the ends of its wires are not be aligned on the disc when flattened (unlike the picture shown on the left above). Notice that it is impossible to make a circuit of six of these elements without increasing the length of each wires. To do so, one needs to solder this end to another wire and extend it to the circuit that will be built (preferably on the Arduino). At this point, it should be decided how each pad are to be situated from the Arduino board so that the wires on the other end can be cut according to this plan in mind. If too short, soldering would be necessary again. If too long, cutting the other end would be recommended to reduce its length. Here is an image example of soldering two wires together:

| |
| ------------- |
| ![Alt text](https://github.com/nehirakdag/Drumduino/blob/master/Images/soldering.jpg) |
| [Source] (https://www.youtube.com/watch?v=Q9G9gaokqvM) |

The wiring job is not quite done yet. The soldered extension of the piezo element will not be very strong. Since the Piezo's wires are pretty thin, and we will be dealing with an impact based system, we should aim to strengthen this connection somehow. If not cautious, the joint can easily detach and would have to be soldered again. Using heat shrinking tubes (electrical tape is also a possible solution) makes sure this does not happen. Putting them on the joint and heating the tube via a lighter or a blowdryer will cause them to shrink until sturdy, but caution should be taken not to damage the wires. After five more repetitions of this process, we should have something like so:

![Alt text](https://github.com/nehirakdag/Drumduino/blob/master/Images/heat_shrink.jpg)

Almost there! Now we glue each of the three layers in the order discussed, on top of the base material to obtain the kit like so:

![Alt text](https://github.com/nehirakdag/Drumduino/blob/master/Images/draft2.jpg)

All that is left is to complete the circuit. With the circuitry for piezo elements in mind (see above for the circuit), we can recreate six circuits of each piezo element in parallel connection with a 1 MegaOhm resistor, with the positive input going in to analog pins of the Arduino:

![Alt text](https://github.com/nehirakdag/Drumduino/blob/master/Images/final_circuit2.jpg)

Parallel connecting each + and - end of the piezo element to the appropriate ends of each circuit concludes the hardware implemetation. The drum kit is now ready to be processed with its software to produce some results!

##Software
The fundamental idea behind the software is as follows. The Arduino can read the voltage readings on each of its six analog pins continually. Printing these values on the Serial during each iteration of the main loop function lets the user be able to send these values to an audio processing software to produce appropriate sounds for the pads. Each pad will read a value of zero (assuming properly designed hardware) or some value below an adjustable threshold when there is no impact applied on them. When a pad is hit, the voltage value corresponding to it will have a higher value depending on the amount of pressure applied. If these values are continually sent to another program (Max/MSP in this case), one can assign Note On/Note Off pairs for each instance when a pad reading goes from zero to a non-zero value.

//PICTURE of 0 0 0 0 0 0's and a few non zeros

However, it is most likely that not every pad will yield identical readings for the same amount of pressure applied. Two pads will most likely respond to the same hit with different readings, considering the fact that they were not built in a factory environment. They each will probably have some noise, such that they keep producing some lower yet non-zero value reading for a certain number of iterations after the one at which they receive a drumstroke. If the user hits a pad and then keeps their drumstick/hand/finger etc. on the pad, the pad will notice this and can lead to the production of roll-like sounds even though no impact is being applied currently. Furthermore, if a pad is too sensitive, hitting a pad may result in non-zero readings in another one, producing undesired sounds again. It would be unprefferable to get a snare sound as well when one hits the hi-hat pad. On the other hand, if each pad is adjusted to only recieve non-zero readings during heavy impacts, the kit would lose the ability to play ghost notes, softer hits and so on. The balance should be created such that none of these outcomes occur. Hitting the pads with drumsticks, hands or other objects will all produce different pressure values on them, and thus create a variety of voltage reading combinations from the six pins on the Arduino. Different players will have different stroke strengths, styles and even the surface on which the kit is placed is an important factor that affects these readings.


To get around these possible sources of error, the Drumduino implements the tuning functionality. In other words, the software is created such that it has the ability to adapt the pads' sensitivities completely depending on the user's desires. Since each pad should be considered individually, we need arrays of the size of the number of pads to store one particular characteristic for each of them. There will be four arrays, one corresponding to the cutoff voltage reading value per each pad, the other for the number of maximum allowed play times (in iterations of the loop() function) for each pad, another for the current play time for each pad, and one to signal if a pad has become activated (received a hit) recently.

//PICTURE OF ARAYS IN THE CODE*

The Serial Bandwidth is set to 115200Hz in this example, and this value is important as Max/MSP must also be set to this bandwidth to properly read the pads' voltages.

In the main loop() function of the Arduino, the implementation is as follows:
- For each pad (each analog pin), read the current voltage value at the iteration
- If this reading is greater than the desired sensitivity threshold reading value, check if the pad was alread actived before
- If it was not, set that pad to active, and set its current play time to zero to let the program know it started playing just now, and output the voltage reading to the serial
- If the pad was already activated from earlier, increment the play time for the pad, and write 0 to the serial (since no need for a non-zero value as the pad already output a non zero value from earlier)
- If the received a non-zero value lower than the threshold, but was already active from before, this reading is probably noise, so must only be processed by incrementing its play time, sending 0 to the serial so there is no undesired roll-like output and deactivating the pad if it has past its maximum play time
- In the final case, if a pad is not active and receives no high voltage value reading (i.e. was not hit recently, and still is not), simply send a 0 to the serial monitor as nothing of interest is currently present for that pad

Finally, there is a small delay at the end of each iteration to prevent overloading the serial.

Here is an image of the complete code. It can also be found in the website's directory as an Arduino sketch file.

//PICTURE OF MAIN LOOP IN THE CODE*

Next, our readings are ready to be processed with MAX/MSP to produce sounds! Firstly, we must read the Arduino's serial by looking at the proper port and serial bandwidth in a specific sampling rate, parse the six integer readings so that they can correspond to inputs of different pads, and send them to note out objects with six different instances that trigger when their corresponding pad is hit. Here is the main patch that achieves this functionality:

//PICTURE OF MAX MAIN PATCH*

Once the pads are parsed and each reading is sent to separate inlets of the subpatch, we simply determine what type of sound we want for each pad to play when hit. Looking up the corresponding MIDI mappings of each drum kit element will likely be useful at this point. Note that the MIDI channel 10 (or 9 if you start from 0) corresponds to percussions, and should be chosen if a drum kit is the desired type of sound. Next, the patch is set up so that each pad will send a bang message to the designated note values when triggered, which cause a note output by activating the noteout object and produce the sound we want. Here is the patch that deals with this issue:

//PICTUTE OF SUBPATCH

##Example Usage
(Please view the raw videos provided, GitHub truncates the actual files)


- [First Upload Testing] (https://github.com/nehirakdag/Drumduino/blob/master/Test%20Videos/First_Upload_Not_Tuned_Yet.m4v) (sorry for size)
- [A Simple Beat] (https://github.com/nehirakdag/Drumduino/blob/master/Test%20Videos/Simple_Beat.mp4)
- [A Short Jam] (https://github.com/nehirakdag/Drumduino/blob/master/Test%20Videos/Short_Jam.m4v) <- a good example demonstrating the drums' sensitivity and potential

##Challenging Issues
- Putting together the hardware was much more difficult than how I envisioned it. I had previously worked with integrating circuits with Arduino to build a variety of different electronics in my engineering classes. However, back then I was more inclined to work on the software portions of group assignments during the division of tasks with my groupmates. I let others worry about the hardware while I worked on software, which was more engaging from my perspective. This project taught me, for one thing, that one can not simply neglect the importance of hardware and the existance of its issues when bringing together a project like this from scratch. Software is not everything (even though I initially preferred it to be).
- Bringing together the separate parts of hardware to build the kit also proved to be somewhat tedious. A few times when I thought I had everything I needed, I realized that some small, yet very important item was missing (like hot glue, spray glue, heat shrink tubes etc. on separate occasions) and I had to get them in order to proceed with being able to do anything at all. This was also the first time that I was not provided with any equipment needed in the building of a project. I learned now that the initial research and accurate brainstorming about parts is extremely important for a time-efficient project design.
- Soldering was also a major issue, as I had never done it before. I did not even know how to properly tin a soldering iron. When I tried to do so after learning about twisting solder on its tip online, I ended up oxidizing the tip of the brand new iron instead, making it obsolete instantly. Only after buying a new one, doing more research on the how-to, and using up two shields and many resistors as practice was I able to learn(although not master) the seemingly simple, yet extremely detailed act of soldering. Even after the tip was tinned just fine, I ended up burning a drum-kit shield that implements circuits for each six pads, so I had to build the circuit manually instead. Thankfully, I will be able to solder anything without problem in the future should I need to thanks to this project.

##Rewarding Issues
- During the cutting and gluing pads on top of each other, I felt like I was not doing something quite right (due to lack of experience) and that the producing pad would end up not working, and my efforts might go to waste therefore. I thought I had to cut every single element in perfect circles, glue everything perfectly flat in a golden ratio that only people dealing with electronics for a long time can achieve in order to get desired functionality. Of course, such imperfections are invitations to noise and small, yet significant errors. However, I learned that perhaps one should not be such a perfectionist in terms of building hardware. If something works, and seems that it will be able to work for a long time, it is better than not building anything due to the worry that it will not be perfect.
- When I first glued all the layers of pads together and checked the readings in each sensor, I was certainly getting responses, but I could not correlate these responses with the impact that I apply to the pads. In other words, it seemed like I would not be able to process the responses to get the desired functionality, and that all the tedious efforts of building the kit would be for nothing. However, after some analysis and research, I was able to come up with the "tuning" aspect of my software in order to adjust the response of each pad independently and properly. Software might not be everything, but it is certainly able to cover up hardware imperfections to some extent.


##Conclusion & Future Goals
This was my first project that involved combining hardware and software to perform audio computing. Although the final product is not overly complicated, this was a project that I had the most fun building since it became something I can personally relate to as a drums player in the end. When trying to come up with a topic, I came across some extremely interesting musical applications of the concepts that we learned about in class (specifically, using the Arduino). This project helped me create a more solid connection in my mind about the topics of MUMT306 and how they can be applied to real life projects. Hopefully this project will be the first step I take towards a possible carreer in this area. My goal is to expand this drum kit by applying a variety of synthesis techniques such as recording and looping a sequence played so that one can play/hear/process multiple rhythmic patterns at the same time and build on each one, applying digital effects to a pad's output in real time.

##Resources
1- ["Drum-Kit Kit"] (http://www.spikenzielabs.com/SpikenzieLabs/DrumKitKit.html)
