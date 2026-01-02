RTSP Streaming Night Mode
==============

.. contents::
  :local:
  :depth: 2

Materials
---------

- `AMB82-mini <https://www.amebaiot.com/en/where-to-buy-link/#buy_amb82_mini>`_ x 1
- Realtek Pro2 to AMB82 MINI sensor adapter board x 1
- Camera sensor with a switchable IR cut filter x 1
- Realtek Amebapro LED board x 1

Example
---------
This example is built on the `Link "StreamRTSP" -> "VideoOnly" <https://ameba-doc-arduino-sdk.readthedocs-hosted.com/en/latest/ameba_pro2/amb82-mini/Example_Guides/Multimedia/RTSP%20Streaming.html>`_. Please refer to the "VideoOnly" example to learn how to set up an RTSP stream as this guide will focus on the IR-related functions.


In this example, we will use the AmebaPro2 board to stream video in night mode. This capability requires a camera that has an IR cut filter that can be toggled on and off along with an IR LED light source (or any IR light source).
The adapter board used in this example is to solely connect our camera sensor to the AmebaPro2 board. You may ignore the adapter board requirement if you have alternatives to connect your IR-cut-equipped camera to the AmebaPro2 board.

You can find this particular example under "Files" -> "Examples" -> "StreamRTSP" -> "NightMode" from the top left corner of the ArduinoIDE.

|image01| wheretofind

The adapter board has a power enable pin which we will be connecting with the GPIO Pin F2 on the AMB82-mini. The IR cut and LED will both be controlled by GPIO Pins F12 and F13 respectively. Pin F12 will connect to the pin TP1 and F13 will connect to pin TP2 on the adapter board.

|image02| pincon

If you are using the adapter board ensure that this is in the NightMode example before running it.

.. code:: c
    #define  PWR_EN 9

    pinMode(PWR_EN, OUTPUT);
    digitalWrite(PWR_EN, HIGH);

Now you just have to edit the network details for the AMB82-mini to open up the RTSP over.

|image03| networkssid

Compile the code and upload it to Ameba. After pressing the Reset button, wait for the AmebaPro2 board to connect to the WiFi network. The board's IP address and network port number for RTSP will be shown in the Serial Monitor.

|image04| vlcmediaplayer

|image05| streamexampleblack

|image06| streamexamplebright

Code Reference
---------
The Infrared class controls all the manual IR features of the AmebaPro2. You will need to the following lines before you can begin using any IR features.

.. code:: c
    #include <Infrared.h>

    Infrared ir;

First, the IR cut and/or LED has to be initialized before you can use them. After initializing, you can toggle the IR cut using "IR.setCut()" and control the IR LED brightness using "IR.setLedBrightness()".

.. code:: c
    ir.cutInit();               # Initializes GPIO Pin F12
    ir.ledInit();               # Initializes GPIO Pin F13
    ir.setCut(0);               # 0 to disable, 1 to enable
    ir.setLedBrightness(100);   # Brightness input can be from 0 to 100, [0,100]

It is also important to remember to set the camera to grayscale mode for better clarity and that the ISP auto-tuning has to be set to night mode. This can only be done after data stream has started by the following functions,

.. code:: c
    configCam.setGrayMode(1);       # 0 for RGB, 1 for Grayscale
    configCam.setDayNightMode(1);   # 0 for day mode ISP auto-tuning, 1 for night mode ISP auto-tuning