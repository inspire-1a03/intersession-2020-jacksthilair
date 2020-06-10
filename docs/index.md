# Jack St Hilaire title page
<!--
Welcome to your project page for Electronics for the Rest of Us. You'll use this page to describe and showcase your work throughout the module. 
A place for each deliverable has been created below for you in this markdown document. 
Note that comments (such as this) will not appear in the final markdown document (which you can view with the "Preview" button).
-->


## Day 1: Reflection
<!--
In this section, provide a ~250 word reflection on your first day of the module, and discuss why you're interested in this module and what you hope to take away from it.

You're also asked to insert a photo that represents your accomplishments on your first day. 
- Take a photo of you working or one of your circuits and upload it to the /docs/images/ folder of this repository. 
- Then, insert your photo into your document by modifying the markdown example that has been inserted below.
-->I have learned many things about programming on only the first day! All of this is super new to me and I have no prior experience with the content on the course. I find it super cool that on my first day I was able to create a website on GitHub. The website for GitHub its kind of confusing and daunting for someone who knows nothing about it. The way it was explained was very clear and made it possible to learn how to navigate the site. I am sure after working with it more I will learn how to use it better and implement more advanced ideas. I chose this course because I am not very experienced with computers and technology and I would love to learn new skills. So far from what I have done I can tell that I will expand my knowledge and be able to make things I never thought were possible before. Some things I would like to take away from this module is to have better skills with technology. Both my brother and my dad are very versed in computers, as my brother has a computer science minor, and my dad worked in computer security. The reason I took this course is because it was more hands on and I would be creating something interesting. I enjoy doing things that are more interactive and being able to see the finished product. The first day was fun yet a little bit frustrating because of my lack of knowledge in computers, but I look forward to the rest of the course!

<!--
Inserting an image takes the form: 
![image alt text](url/to/photo "Logo Title Text")
See the following webpage for more information: https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet#images
Replace the elements below to insert your picture.
--> 
![Markdown logo](images/markdown.png "This is the Markdown logo!")

## Day 2: Results
<!--
-->
```
/*
 * Inputs ADC Value from Thermistor and outputs Temperature in Celsius
 *  requires: include <math.h>
 * Utilizes the Steinhart-Hart Thermistor Equation:
 *    Temperature in Kelvin = 1 / {A + B[ln(R)] + C[ln(R)]3}
 *    where A = 0.001129148, B = 0.000234125 and C = 8.76741E-08
 *
 * These coefficients seem to work fairly universally, which is a bit of a 
 * surprise. 
 *
 * Schematic:
 *   [Ground] -- [10k-pad-resistor] -- | -- [thermistor] --[Vcc (5 or 3.3v)]
 *                                               |
 *                                          Analog Pin 0
 *
 * In case it isn't obvious (as it wasn't to me until I thought about it), the analog ports
 * measure the voltage between 0v -> Vcc which for an Arduino is a nominal 5v, but for (say) 
 * a JeeNode, is a nominal 3.3v.
 *
 * The resistance calculation uses the ratio of the two resistors, so the voltage
 * specified above is really only required for the debugging that is commented out below
 *
 * Resistance = PadResistor * (1024/ADC -1)  
 *
 * I have used this successfully with some CH Pipe Sensors (https://www.atcsemitec.co.uk/pdfdocs/ch.pdf)
 * which be obtained from https://www.rapidonline.co.uk.
 *
 */

#include <math.h>

#define ThermistorPIN 0                 // Analog Pin 0

float vcc = 4.91;                       // only used for display purposes, if used
                                        // set to the measured Vcc.
float pad = 9850;                       // balance/pad resistor value, set this to
                                        // the measured resistance of your pad resistor
float thermr = 10000;                   // thermistor nominal resistance

float Thermistor(int RawADC) {
  long Resistance;  
  float Temp;  // Dual-Purpose variable to save space.

  Resistance=pad*((1024.0 / RawADC) - 1); 
  Temp = log(Resistance); // Saving the Log(resistance) so not to calculate  it 4 times later
  Temp = 1 / (0.001129148 + (0.000234125 * Temp) + (0.0000000876741 * Temp * Temp * Temp));
  Temp = Temp - 273.15;  // Convert Kelvin to Celsius                      

  // BEGIN- Remove these lines for the function not to display anything
  //Serial.print("ADC: "); 
  //Serial.print(RawADC); 
  //Serial.print("/1024");                           // Print out RAW ADC Number
  //Serial.print(", vcc: ");
  //Serial.print(vcc,2);
  //Serial.print(", pad: ");
  //Serial.print(pad/1000,3);
  //Serial.print(" Kohms, Volts: "); 
  //Serial.print(((RawADC*vcc)/1024.0),3);   
  //Serial.print(", Resistance: "); 
  //Serial.print(Resistance);
  //Serial.print(" ohms, ");
  // END- Remove these lines for the function not to display anything

  // Uncomment this line for the function to return Fahrenheit instead.
  //temp = (Temp * 9.0)/ 5.0 + 32.0;                  // Convert to Fahrenheit
  return Temp;                                      // Return the Temperature
}


int redPin = 11;
int greenPin = 10;
int bluePin = 9;

//uncomment this line if using a Common Anode LED
//#define COMMON_ANODE

void setup()
{
  Serial.begin(115200);
  pinMode(redPin, OUTPUT);
  pinMode(greenPin, OUTPUT);
  pinMode(bluePin, OUTPUT);  
}

void loop()
{
    float temp;
  temp=Thermistor(analogRead(ThermistorPIN));       // read ADC and  convert it to Celsius
  Serial.print("Celsius: "); 
  Serial.print(temp,1);                             // display Celsius
  //temp = (temp * 9.0)/ 5.0 + 32.0;                  // converts to  Fahrenheit
  //Serial.print(", Fahrenheit: "); 
  //Serial.print(temp,1);                             // display  Fahrenheit
  Serial.println("");                                   
  delay(1000);                                      // Delay a bit... 

  if(temp > 30){
   setColor(255, 0, 0);  // red
  }
 else if(temp > 20 & temp < 30){ 
  setColor(0, 255, 0);  // green
 }
  else {
  setColor(0, 0, 255);  // blue
  }
}

void setColor(int red, int green, int blue)
{
  #ifdef COMMON_ANODE
    red = 255 - red;
    green = 255 - green;
    blue = 255 - blue;
  #endif
  analogWrite(redPin, red);
  analogWrite(greenPin, green);
  analogWrite(bluePin, blue);  
}
```
<!--
Upload your fully-commented Arduino sketch from your final Day 2 build task--a thermometer connected to an RDB LED--into your GitHub repository.
Provide a short (~150 words) summary of your work on this circuit:
- How does your device work?
- What was challenging? 
- What worked? What didn't? 
- Be sure to link to your code (in your GitHub repository) in the text of your response.
--> 

This circuit I made serves a purpose to show a certain colour which represents the temperature. For example, I set it to display red if the temperature is greater than 30 degrees Celsius and to display green if the temperature is greater than 20 and less than 30. I found it easy to build the thermometer, but I made some crucial mistakes by using the potentiometer instead of the photoresistor. Other than this, Jay made it simple to follow along and was great at showing a tutorial on how to do it. It all worked but not after I had some slight modifications. I had to do different steps because I did not have a common anode. This made some of my steps different from where I had to plug in certain jumpers to different areas. For example, I had to plug it into ground for one of my jumpers. Making the circuit was a lot of fun and I never thought I would have been able to make something like this before this course. 

## Arduino build-off results
<!--Upload your fully-commented Arduino sketch from the final product of your Arduino build-off into the top-level of your module GitHub repository.
In ~300 words, provide a final device description and product pitch: 
- What does it do? Use a table (created in markdown) to list and describe the features. You can use the template provided below. 
- Describe briefly how it works.
- How could it be used in everyday life (or maybe just in rare cases)? 
- Be sure to link to your code (in your GitHub repository) in the text of your response.
- Include a snippet of code using the ``` ``` characters to display the code properly. 
Finally, record a short (30 second) video of a 'product pitch' for your device. 
- Upload the video to Youtube, and use the sample code below to embed your video.
https://youtu.be/FIm8gJ06mCQ 
-->


The Arduino circuit I built is my colour coordinated weather station. There are many circuits working in motion to create this weather station. The way it works is that it shows certain colours for a specific temperature. The RGB will light up green when is in between the temperature 25-30 degrees. It will light up blue if it below 25 degrees. And if it above 30 degrees Celsius it will light up red and a buzzer will sound until it drops below this temperature. The way it is able to read the temperature is with a photoresistor. This weather station that I have coined the Colourful Weather Station 3000 can be used in some unique circumstances. It can be used if you have to have a room kept at a certain temperature for example if the room can not be over 30 degrees, you can place the Weather Station 3000 in the room and the buzzer will sound if I gets above that temperature. This can also be very handy for having it outside and be able to tell the temperature. If the Weather Station 3000 is red that means it is way too hot outside and you should stay inside. If it displays green that means you are good to go outside because it is a comfortable temperature and you should enjoy the outdoors. The best way to use this device is to have it in an area that should not be above 30 degrees Celsius to the buzzer will go off. I did not know it was possible to have all these separate components working together in harmony to make the Weather Station 3000. I am very impressed that I was able to build something that is so complicated with no prior knowledge for anything like this. 


<!--
Below is a general markdown table template. 
You can find more information at these links: 
- https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet#tables

-->

| Feature | Description | Other Notes |
|---------|-------------|-------------|
|   photresistor      |     this calculates the temperature and is able to work with the RGB LED to change its colours       |             |
|   RGB LED changes       |   changes to 3 diffrent colours          |             |
|      Buzzer  |   Buzzer sounds when it is above 30 degrees and displays a red colour          |             |
|         |             |             |


<!--
Below is an example of embedding a YouTube video in a markdown document for use in GitHub pages. 
Note that this video won't show when previewing the document in GitHub--it only works on the GitHub pages webpage. 
- Once your YouTube video is uploaded, right click and select ```<> Copy embed code```. 
- You can paste this code directly into your markdown document. 
- Note that you may want to adjust the width and height parameters to make it fit well in your webpage
-->

<iframe width="789" height="444" src="https://www.youtube.com/embed/FIm8gJ06mCQ " frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


## Final reflection & summary
<!--
In ~300 words:
- Summarize your experience in this module. What you learned, what you liked, what you found challenging.
- Reflect upon your learning and its relevance in your life.
-->
For the Inspire course I had three unique modules, and this has been my favourite one. Going into this module it was definitely the most daunting as I have never done anything with electronics or coding before. Working with the Arduino was exciting as it was satisfying to be able to see something that you have built come to life. I enjoy doing things hands on rather than writing tests and exams, so this module was refreshing. The feeling you got after you hooked something up to your bread board and it worked was something I did not expect to happen with this course. One aspect I found challenging was how I had to submit my work. I am not the most tech savvy and I was not sure if I was submitting it the right way. I did all the work and made sure I put it where I thought it should go but it was frustrating to think that I might have not have submitted it properly. I do understand that this was a core element of the course and I am here to learn new things. What made this possible was the very helpful way Jay made it to follow along as well as going above and beyond by dropping off a new Arduino at my house when mine was faulty. With everything being online it made the course harder to teach, but he was helpful with troubleshooting and helping me to figure out how to work my devices. This module has opened my eyes to something that I never thought I would be interested in. My favourite part was building with the Arduinos and use a code to produce a certain function. For example, my weather station was awesome as I had three functions working in unison to create my Weather Station 3000. I can use this in my everyday life with having some skills to create a website through GitHub. It also made me think about how electronics are part of so many things, from using a remote to turn on the TV to playing video games. This module was educational, hands on and fun and I would recommend this course to anyone.
