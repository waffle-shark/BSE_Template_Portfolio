# Audio Visualizer
Replace this text with a brief description (2-3 sentences) of your project. This description should draw the reader in and make them interested in what you've built. You can include what the biggest challenges, takeaways, and triumphs from completing the project were. As you complete your portfolio, remember your audience is less familiar than you are with all that your project entails!

You should comment out all portions of your portfolio that you have not completed yet, as well as any instructions:
```HTML 
<!--- This is an HTML comment in Markdown -->
<!--- Anything between these symbols will not render on the published site -->
```

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Harini P | West High School | Software Engineering | Incoming Freshman

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](logo.svg)
  
# Final Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/XrU3acGw_BY" title="Harini P Milestone 3" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE



# Second Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/mKDgi6pT2Xo" title="Harini P Milestone 2" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Since my second milestone, I've figured out my modification. My modicfication is that the bars will be relfected over a line in the center of the display, and the louder portions will be red and the quieter parts will be blue. A challenge that I faced while working toward my modification, was that I was orginally planning to use a rainbow LED display. After some extra research, we figured out that the display was unable to be supported by my Arduino due to power issues. So, we had to find an entirely new way to display the Audio visualizer. So, we decided to use a TFT LCD display. It's liek a mini phone screen, which made it easier. I also had to code for a part I didn't have on hand, which was challenging at first, but ended up working out as I did not need to debug anything! A challenge I am facing right now, though, is that the rate at which the frames of the bars are changing is too slow. It often gets stuck at one place. So, before my final milestone, my goal is to fix the frames so they move somewhat smoothly.

# First Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/ZzvixJHYUk0" title="Harini P Milestone 1" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

My project is a DIY audio visualizer using an Arduino Nano, a 32×8 MAX7219 LED matrix, and a microphone module. The goal is to capture live audio, analyze it using the Fast Fourier Transform (FFT), and display the sound frequencies as moving bars on the LED matrix. The Arduino reads analog sound data from a microphone connected to pin A0 and uses the arduinoFFT library to convert that sound into its frequency components. These frequency values are then mapped to visual bar heights, which are displayed in real-time on the LED matrix using the MD_MAX72XX library. One of the challenges I faced was fine-tuning sensitivity for my environment. Thankfull, I found the optimal sensitivity (4.9). The other challenge that I am facing is that there seem to be 2 filled bars at the left hand side of my matrix. They are due to other noises that my mic is picking up. I would like to resolve this issue moving forward. I also plan to explore more visual effects and possibly expand to a color LED matrix for a cooler look. 

# Schematics 
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 

# Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

```c++
#include <arduinoFFT.h>
#include <MD_MAX72xx.h>
#include <SPI.h>
float sensitivity = 4.9; 
MD_MAX72XX disp = MD_MAX72XX(MD_MAX72XX::FC16_HW, 10, 4);
arduinoFFT FFT = arduinoFFT();
double realComponent[64];
double imagComponent[64];
int spectralHeight[] = {0b00000000,0b10000000,0b11000000,
                        0b11100000,0b11110000,0b11111000,
                        0b11111100,0b11111110,0b11111111};
int index, c, value;
void setup()
{
  disp.begin();
  Serial.begin(9600);
}
void loop()
{
  Serial.println (analogRead(A6));
  for(int i=0; i<64; i++)
  {
    realComponent[i] = analogRead(A0)/sensitivity;
    imagComponent[i] = 0;
  }
  FFT.Windowing(realComponent, 64, FFT_WIN_TYP_HAMMING, FFT_FORWARD);
  FFT.Compute(realComponent, imagComponent, 64, FFT_FORWARD);
  FFT.ComplexToMagnitude(realComponent, imagComponent, 64);
  for(int i=0; i<32; i++)
  {
    realComponent[i] = constrain(realComponent[i],0,80);
    realComponent[i] = map(realComponent[i],0,80,0,8);
    index = realComponent[i];
    value = spectralHeight[index];
    c = 31 - i;
    disp.setColumn(c, value);
  }
}
```

# Bill of Materials
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Bewinner 1.8inch LCD Display Module, TFT Screen Module | Displaying the bars | $8.99 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/Bewinner-Resolution-Interface-Full-Color-Controller/dp/B083NYBN4Q?crid=12S2ZGBOVSY5H&dib=eyJ2IjoiMSJ9.fyDKEDaZ-SyYFjeeQWmIjKWSAgGv-rWQCzs6OfFR8Y7okFbQSezTTtBCWPs1wzE6a1V_QVDZ1E99UO7Tp9eEm1IuW8Ngh2twohn67HUODXx5IwdpR1JoKbwx3zTkfhnZvQzP4BeW9n0xwkcrFOm3cEV4tJC7GCgrMZ7uqLtYkB-NkikdItOMrXx0i7WK4QpT5xV1cRUsk_-5BveME4UV_09UUJHAzju6gYxvLkfTKrvBa7_bI6ddOxaA__-5kIZ_NXvnLdbZk170u6G9DcvXosexiuo-iM-fPlQr9ABYJq0.FNCMjCtO3RvX36toQqV2QU9O40AlQIUzNOcba9rDbGM&dib_tag=se&keywords=ST7735+TFT&qid=1752499772&s=electronics&sprefix=st7735+tft%2Celectronics%2C114&sr=1-3)"> Link </a> |
| ELEGOO UNO R3 Project Most Complete Starter Kit | Used the arduino, wires, breadboard | $59.99 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/EL-KIT-001-Project-Complete-Starter-Tutorial/dp/B01CZTLHGE/ref=sr_1_1_sspa?crid=2660U55Y4R0BQ&dib=eyJ2IjoiMSJ9.-TMWe7jTY1L2k9FBx9xn49qwFiVQDbHTh9labdWt--hedwkviIJguyezw59BW6r90ocCa4MEtGLi56YYbLjLLzufLJsiRGHx4fL574GMTqedpFL0DJiXW0naRr3GAqJJmM41oVgH0HZxcTAeCaD_2sXXTOliSzkPmxPHG2SKI-GSnG906aa5ey_ea7BF6XossbZJkRT0-NLuuJ5MKADgXfW8PJcmfs-fOio56KSPLOvjNhGF5LTXZyWIv13zuWcY3fmPN3MIp79sS0stz47wQDDnndnCJfLeMAujrGr_13U.OrzmQdKGXHvOifiPbKLaUI-t0BpSMIAR9yIRS2cvSu8&dib_tag=se&keywords=the%2Bmost%2Bcomplete%2Bstart%2Bkit%2Barduino%2Buno%2Belegoo&qid=1753460091&s=electronics&sprefix=the%2Bmost%2Bcomplete%2Bstart%2Bkit%2Barduino%2Buno%2Belegoo%2Celectronics%2C145&sr=1-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&th=1)"> Link </a> |
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |

# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
