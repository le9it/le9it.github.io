---
layout: post
title: "Displaying Current PC Stats on a 128x64 OLED display using ESP32"
date: 2026-02-24
categories: blog
tags: [hardware, tinkering]
---

WIP, please wait an indefinite amout of time before i finish this, i don't have much free time:(


I will write this post when i have time, but for the curious, it looks like this
![me being stupid](/assets/images/oled.jpg)

hey, never said it looks pretty



# Intro or something.. god i suck at titles

The ESP32 is an amazing device, you can do so much with it, like blinking an led :D. I was able to obtain a 128*64 OLED module that uses SPI for less than a dollar, which means i can do something actually useful with it for once, so i thought about using it for displaying current PC stats (not that a 0.9inch secondary display for monitoring resource usage is very useful..)

I'll skip the whole part where im like oH i TesTeD thE olED wItH thiS cOde anD thAt coDE, no need, it works, and we'll get straight to the juicy stuff

# To display the resource, one must obtain them

I wanted to display usage% and temperature (in Kelvin, obviously) for both CPU and GPU, while also getting the amount of RAM and VRAM being used, and i mainly use linux, so im unable to go the easy route by using [open hardware monitor](https://openhardwaremonitor.org/) like you can on windows, therefore i must find a way get the data myself from the system

Thankfully, CPU and RAM data are obtainable very easily using [psutil](https://pypi.org/project/psutil/) (yes im using the filthy python library while a C one exists, deal with it, it's what im most familiar with), the same, however, cannot be said about GPU data.

After days of digging through the system files, the internet, and reading a bit of the code of [amdgpu_top](https://github.com/Umio-Yasuno/amdgpu_top), i was finally able to find that the file at __"/sys/class/drm/card1/device/gpu_metrics"__ describes, as per the name, the metrics of the GPU, including temperature, horray, problem solve- WRONG, sure we've found where the data we need is, but it's just a bunch of bytes in a file, how am i supposed to know which bytes describes which metric? well, i got lucky and found [this](https://gist.github.com/leuc/e45f4dc64dc1db870e4bad1c436228bb) absolute gem, literally exactly what i needed

![amdgpu_top result](/assets/images/amdgpu_top.jpg)


after running amdgpu_top and finding out that my GPU uses the __v1.3__ map, i made a small python code that gets all the needed data and sends it (very inefficiently, might i add) to the ESP32, then i uploaded a code to the ESP32 that expects the exact format i sent and displays it on the OLED, voila, it's done


# Conclusion

Building stuff is fun, it's even more fun (for me at least) when its something that i actively use, sure the display is pretty small so i have to keep it very close to my main monitor, but it does give me a rough idea about where my specs usage is. Stay tuned for the next blog post where i make a QLED display that shows the quantum tunneling happening an 500km radius around me :D

As for codes, i will update the post as soon as i clean them up and ready them for publishing, please stay tuned (no like actually do, i have no idea when will i go around doing it lol)



