**required materials:**

&#x20;• a software defined radio and its software (sdr++ in this case)

&#x20;• a microcontroller and a Transmitter circuit

&#x20;• audio editing software



![Alt text](16MhzCarrierOnSDR.png)





**How to setup:**

&#x20;• draw your image using the program contained in the folder called "BinaryImageTotextFile"

&#x20;• use right click to draw pixels and left click to delete pixels

&#x20;• press space to write data to file(overwrites data to a pre existing file if it exists)

&#x20;• pressing space allows you to only write the image data once in order not to overwrite to a file when space is

&#x20;  pressed more than once

&#x20;• on the SDR software set the signal decoding to CW and set the tone frequency to a high value but half the audio sample rate due to sampling limitations, @48000hz, 1250hz tone





once everything is setup you will have the binary image as a text file in the folder directory "BinaryImageToTextFile\\output"

open the file and copy all of the data inside.

Next open the .ino file in the folder directory "BinaryOutputToMcu" and delete the elements of the Binary data array if there is existing data.

once the array is empty copy all of the data from the binary image text file to the array ONCE.

once done plug in your desired micro controller and upload the code then wire the digital write pin to the optocoupler anode(must use a current limiting resistor typically in the range of 220 ohms - 2k) and the micro controller ground hooked up to the optocoupler cathode.

when all of these steps are done turn on the transmitter and Micro controller to begin transmitting data over air and use your SDR to log the data

to a .wav file



**How to process the .wav file**:

&#x20;open the file in the desired audio editing program and align the left side crop to the exact start of the first preamble bit

&#x20;once done save the cropped audio file and include it in the OOKdecoder.py program to see your results (Adjust the threshold variable value to get the proper output)



![Alt text](binaryDataWaveform.png)

**Audio transmisson playback**
![Alt text](AudioTransmissonPlayback.mov)

**Decoded data && side by side output and input image comparsion**
![Alt text](outputAndInput.png)
*the left image is the decoded data sent over air*

*the right image is the original image used as the source for transmisson over air*



**extra information**:

&#x20;• the preamble and postamble contains 8 bits where all bits are on (used as the cropping reference)
• the transmitter has a bit width of 25ms meaning that it transmits at 40bits/second (1000ms/25ms= 40bits/second) and the time for a full transmisson is defined by this equation (imageBitSize+preambleSize+postambleSize)/ bits per second

• the bit width can be reduced for faster data transfer but due to circuit limitations it might not give the expected result due to voltage not ramping fast enough (best signal quality @ 25ms bit width)

