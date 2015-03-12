# Demoprogram

Raspbian kommer med en rad demoprogram som du bara kan kompilera och köra. De sträcker sig från enkla *hallå världen* med textutmatning, till full uppspelning av 1080p HD-video, 3D spinnande tekannor och realtidsanimerade fraktala mönster. Dessa är ett bra sätt att få en känsla för vad Pi kan göra, och för att få viss erfarenhet av att navigera runt i systemet och köra program på kommandoraden.

## Å nej! Ett kommandoradsgränssnitt!

Starta upp din Raspberry Pi och du kommer att finna dig vid prompten nedan. Om du har konfigurerat din Pi att automatiskt gå in i skrivbordsgränssnittet, använd `start` knappen för att logga ut från skrivbordet.

`pi@raspberrypi ~ $ _`

Texten ovan är kommandoprompten. Försök att inte vara rädd för den! Ett CLI (command line interface) är faktiskt ett mycket snabbt och effektivt sätt att använda en dator.

För att starta, gå till `hello_pi`-mappen där alla demos lagras. Ange kommandot nedan för att göra detta. **TIP**: Du kan använda `TAB`-tangenten för automatisk komplettering när du skriver in kommandon.

`cd /opt/vc/src/hello_pi`

The command prompt should now look like this. The blue part shows where you are in the file system of the Pi.

`pi@raspberrypi /opt/vc/src/hello_pi $ _`

If you enter `ls` and press `enter`, you’ll see a list of folders. There is one for each demo. Before you can run them though, they must be compiled. Don’t worry if you don’t understand why you need to do this; just go along with it for now, and we'll learn more about it later on.

There is a small shell script supplied in the `hello_pi` folder called `rebuild.sh`, which will do the compile for you. Enter the following command to run it. Ignore the gobbledygook for now!

Kommandoprompten bör nu se ut så här. Den blå delen visar var du är i filsystemet på Pi:n.

`pi@raspberrypi /opt/vc/src/hello_pi $ _`

Om du skriver `ls` och trycker `enter` ser du en lista med mappar. Det finns en för varje demo. Innan du kan köra dem, måste de dock kompileras. Oroa dig inte om du inte förstår varför du behöver göra detta; bara gå med på det för tillfället, och vi kommer att lära mer om det senare.

Det finns ett litet skalskript som levereras i `hello_pi`-mappen som heter `rebuild.sh`, vilket kommer att göra kompileringen åt dig. Ange följande kommando för att köra den. Ignorera rappakaljan för tillfället!

`./rebuild.sh`

En massa text kommer rulla upp skärmen nu, men för denna övning kan du ignorera det. Det är bara utmatningen från kompilatorn när den jobbar sig igenom demokoden. Vänta till kommandoprompten återvänder innan du fortsätter.

Nu är vi redo att köra några demos.

## Hello world

First, let's do a quick test that will ensure the previous compilation step worked correctly. This rather boring program will only display the text `Hello world!`, but if it works correctly then we know all the other demos should work too.

Enter the following commands to go inside the `hello_world` folder and list the files.

```
cd hello_world
ls
```

You’ll notice the `.bin` file is shown in green. This is because it is an executable file. This means this is the file we run to launch the program.

Use the following command to run the demo. You need the `./` to specify the current directory, otherwise the Linux system folders will be searched for the filename you type.

`./hello_world.bin`

## Hello video

This will play a 15 second long, full HD 1080p video clip with no sound. The intention here is to demonstrate video decode and playback capability. You’ll see that it's very smooth!

![image](./images/bbb.jpg "Big Buck Bunny")

Enter the following commands to navigate to the `hello_video` folder and list the files.

```
cd ..
cd hello_video
ls
```

You’ll notice the `.bin` file again. This demo needs to be told what video clip to play when we run it though, so this must be the `test.h264` file (h264 is a type of video codec).

You'll need the `./` to specify the current directory again.

`./hello_video.bin test.h264`

## Hello triangle

This displays a spinning cube with different images on each side. This is intended to demonstrate Open GL ES rendering (an open-source programming library for doing 3D graphics).

Enter the following commands to navigate to the `hello_triangle` folder and list its contents.

```
cd ..
cd hello_triangle
ls
```

You’ll again see that one of the files is green. This is the executable file. This demo doesn’t need any video input files like the previous one, so you can just go ahead and run the `.bin` file.

`./hello_triangle.bin`

The demo will run forever until you decide to quit. To exit the demo press `Ctrl – C`.

## Hello triangle 2

This one displays two superimposed fractals, one on top of the other. You can move the mouse to change the shape of the fractal in real time. This is also intended to demonstrate Open GL ES rendering. Some of you may recognise the Mandelbrot fractal.

![image](./images/mandelbrot.jpg "Mandelbrot")

```
cd ..
cd hello_triangle2
ls
```

Notice the green `.bin` file? Okay, run it!

`./hello_triangle2.bin`

Now move the mouse around, and you’ll see the fractal changing. See if you can get it to form a perfect circle. It’s a little tricky, but it can be done. To exit the demo press `Ctrl – C`.

## Hello teapot

This displays a spinning teapot with the video clip from `hello_video` texture-mapped onto its surface. Impressive! You may recognise the teapot model if you’re familiar with a piece of software called Blender. This demonstrates Open GL ES rendering and video decode/playback at the same time.

![image](./images/teapot.jpg "Tea Pot")

```
cd ..
cd hello_teapot
ls
```

Notice the green `.bin` file? Okay, run it!

`./hello_teapot.bin`

You may receive the following error when you try to run this demo:

```
Note: ensure you have sufficient gpu_mem configured
eglCreateImageKHR:  failed to create image for buffer 0x1 target 12465 error 0x300c
eglCreateImageKHR failed.
```

Don’t worry though: if you see this error, you just need to alter one configuration setting to make it work.

The error means the GPU (graphics processing unit) does not have enough memory to run the demo. It’s the GPU that does all the heavy lifting when drawing 3D graphics to the screen (a bit like the graphics card in a gaming PC). The Raspberry Pi shares its memory/RAM between the CPU and GPU, and by default is configured to only give 64 MB of RAM to the GPU. If we increase this to 128, that should fix the problem.

To do that, you'll need to enter the following command:

`sudo raspi-config`

This will open up a menu on a blue background. Perform the following actions:

- Go to Advanced Options.
- Go to Memory Split.
- Delete `64` and enter `128` instead. Press `enter`.
- Go down to Finish.
- Click Yes to reboot.

After you have logged back in, enter the following command to get back to the `hello_teapot` demo:

`cd /opt/vc/src/hello_pi/hello_teapot`

Now try and run it again, and you should find it will work.

`./hello_teapot.bin`

The demo will run forever until you quit. To exit the demo press `Ctrl – C`.

## Hello audio

This demo just demonstrates audio output. It plays a sine wave, which makes a kind of WOO WOO WOO sound.

```
cd ..
cd hello_audio
ls
```

Notice the green `.bin` file? Run it. Getting the hang of this now?

`./hello_audio.bin`

This will play the sound over the headphone jack on the Pi. If you’re using a HDMI monitor, you can make it output over HDMI by adding a `1` to the command.

`./hello_audio.bin 1`

The demo will run forever until you quit. To exit the demo press `Ctrl – C`.

## What next?

- By now you should be getting the hang of navigating up into the parent `hello_pi` folder (using `cd ..`) and then down into one of the demo folders (using `cd hello_something`).  
- Try some of the other demos on your own. The `hello_videocube` one is a good place to start.
