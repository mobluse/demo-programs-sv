[PDF för utskrift](https://gitprint.com/mobluse/demo-programs-sv/blob/master/worksheet.md)
# Demoprogram

Raspbian kommer med en rad demoprogram som du bara kan kompilera och köra. De sträcker sig från enkel "hallå världen" textutmatning, till uppspelning av 1080p/Full-HD-video, snurrande 3D-tekannor, och realtidsanimerade fraktala mönster.
Dessa är bra för att få en känsla för vad Pi:n kan göra, och för att få lite erfarenhet av att navigera runt i systemet och köra program från kommandoraden.

## Åh nej! Ett kommandoradsgränssnitt!

Starta upp din Raspberry Pi och du kommer att finna dig vid prompten nedan. Om du har konfigurerat din Pi att automatiskt gå in i skrivbordsgränssnittet, använd `start`-knappen för att logga ut från skrivbordet.

`pi@raspberrypi ~ $ _`

Texten ovan är kommandoprompten. Försök att inte vara rädd för den! Ett CLI (Command Line Interface) är faktiskt ett mycket snabbt och effektivt sätt att använda en dator på.

För att starta, gå till `hello_pi`-mappen där alla demona lagras. Ange kommandot nedan för att göra detta. **TIPS**: Du kan använda `TAB`-tangenten för automatisk komplettering när du skriver in kommandon.

`cd /opt/vc/src/hello_pi`

Kommandoprompten bör nu se ut så här. Den blå delen visar var du är i filsystemet på Pi:n.

`pi@raspberrypi /opt/vc/src/hello_pi $ _`

Om du skriver `ls` och trycker `enter` ser du en lista med mappar. Det finns en för varje demo. Innan du kan köra dem måste de dock kompileras. Oroa dig inte om du inte förstår varför du behöver göra detta, bara gå med på det för tillfället, så kommer vi att lära oss mer om det senare.

Det finns ett litet skalskript som levereras i `hello_pi`-mappen som heter `rebuild.sh`, vilket kommer att göra kompileringen åt dig. Ange följande kommando för att köra det. Ignorera rappakaljan för tillfället!

`./rebuild.sh`

En massa text kommer rulla upp på skärmen nu, men för denna övning kan du ignorera den. Det är bara utmatningen från kompilatorn när den jobbar sig igenom demokoden. Vänta till kommandoprompten återvänder innan du fortsätter.

Nu är vi redo att köra några demon.

## Hallå världen

Först, låt oss göra ett snabbt test som kommer att säkerställa att det tidigare kompileringssteget fungerat korrekt. Detta ganska tråkiga program kommer bara att visa texten `Hello world!`, men om det fungerar korrekt så vet vi att alla andra demon också bör fungera.

Ange följande kommandon för att gå in i `hello_world`-mappen och lista filerna.

```
cd hello_world
ls
```

Du kommer att märka att `.bin`-filen visas i grönt. Detta beror på att det är en körbar fil. Det innebär att detta är den fil som vi kör för att starta programmet.

Använd följande kommando för att köra demot. Du behöver `./` för att ange den aktuella katalogen, annars kommer Linux' mappar för körbara filer att sökas igenom efter det filnamn du skriver, men inte den nuvarande katalogen (av säkerhetsskäl).

`./hello_world.bin`

## Hallå video

Detta kommer att spela ett 15 sekunder långt, Full-HD/1080p-videoklipp utan ljud. Avsikten här är att visa videoavkodning och -uppspelning. Du kommer se att det går väldigt flytande (d.v.s. utan att lagga)!

![image](images/bbb.jpg "Big Buck Bunny")

Ange följande kommandon för att navigera till `hello_video`-mappen och lista filerna.

```
cd ..
cd hello_video
ls
```

Du kommer att märka `.bin`-filen igen. Detta demo måste dock få veta vilket videoklipp som ska spelas när vi kör det, så det måste vara `test.h264`-filen (h264 är en typ av video-codec).

Texten `./` behövs för att använda den nuvarande katalogen, annars letar Linux i systemmapparna efter filnamnet du skrivit.

`./hello_video.bin test.h264`

## Hallå triangel

Detta visar en snurrande kub med olika bilder på varje sida. Syftet är att visa Open GL ES-rendering (ett programmeringsbibliotek med öppen-källkod för att göra 3D-grafik).

Ange följande kommandon för att navigera till `hello_triangle`-mappen och lista dess innehåll.

```
cd ..
cd hello_triangle
ls
```

Du kan återigen se att en av filerna är grön. Detta är den körbara filen. Detta demo behöver inte några videoindatafiler som det tidigare, så du kan bara gå vidare och köra `.bin`-filen.

`./hello_triangle.bin`

Demot kommer att köras för evigt eller tills du bestämmer dig för att sluta. För att lämna demot, tryck `Ctrl+C`.

## Hallå fraktal

Detta visar två överlagrade fraktaler, en ovanpå den andra. Du kan flytta musen för att ändra formen på fraktalen i realtid. Detta är också avsett att demonstrera Open GL ES-rendering. Några av er kanske känner igen Mandelbrot-fraktalen.

![image](images/mandelbrot.jpg "Mandelbrot")

```
cd ..
cd hello_triangle2
ls
```

Ser du den gröna `.bin`-filen? Okej, kör den!

`./hello_triangle2.bin`

Kör nu omkring med musen, och du ser hur fraktalen förändras. Se om du kan få den att bilda en perfekt cirkel. Det är lite knepigt, men det är möjligt. För att lämna demot, tryck `Ctrl+C`.

## Hallå tekanna

Detta visar en snurrande tekanna med videoklipp från `hello_video` textur-mappat på dess yta. Imponerande! Du kanske känner igen tekannemodellen om du är bekant med en programvara som heter Blender. Detta visar Open GL ES-rendering och videoavkodning och -uppspelning samtidigt.

![image](images/teapot.jpg "Tea Pot")

```
cd ..
cd hello_teapot
ls
```

Ser du den gröna `.bin`-filen? Okej, kör den!

`./hello_teapot.bin`

Du får kanske följande felmeddelande när du försöker köra detta demo:

```
Note: ensure you have sufficient gpu_mem configured
eglCreateImageKHR:  failed to create image for buffer 0x1 target 12465 error 0x300c
eglCreateImageKHR failed.
```

Men oroa dig inte: Om du ser detta fel behöver du bara ändra en konfigurationsinställning för att få det att fungera.

Felet innebär att GPU:n (Graphics Processing Unit) inte har tillräckligt med minne för att köra demot. Det är GPU:n som gör allt tyngdlyftande vid ritande av 3D-grafik till skärmen (lite som grafikkortet i en spel-PC). Raspberry Pi:n delar sitt minne/RAM mellan CPU:n (Central Processing Unit) och GPU:n, och som standard är den konfigurerad att bara ge 64 MB RAM till GPU:n. Om vi ​​ökar detta till 128, bör det lösa problemet.

För att göra det, måste du ange följande kommando:

`sudo raspi-config`

Detta kommer att öppna upp en meny på en blå bakgrund. Utför följande åtgärder:

- Gå till Advanced Options.
- Gå till Memory Split.
- Ta bort `64` och ange `128` i stället. Tryck `Enter`.
- Gå ner till Finish.
- Klicka på Yes för att starta om.

Efter att du har loggat in igen, skriv följande kommando för att komma tillbaka till `hello_teapot`-demot:

`cd /opt/vc/src/hello_pi/hello_teapot`

Försök nu att köra det igen, och du kommer upptäcka att det fungerar.

`./hello_teapot.bin`

Demot kommer att köras för evigt eller tills du avslutar. För att lämna demot, tryck `Ctrl+C`.

## Hallå audio

Detta demo visar bara ljuduppspelning. Det spelar en sinusvåg, vilken gör ett slags WOO WOO WOO-ljud.

```
cd ..
cd hello_audio
ls
```

Ser du den gröna `.bin`-filen? Kör den. Börjar du få kläm på detta nu?

`./hello_audio.bin`

Detta kommer att spela upp ljudet via hörlursuttaget på Pi:n. Om du använder en HDMI-monitor, kan du få den att spela upp via HDMI genom att lägga till en `1`:a till kommandot.

`./hello_audio.bin 1`

Demot kommer att köras för evigt eller tills du avslutar. För att lämna demot, tryck `Ctrl+C`.

## Vad vill du göra nu?

- Vid det här laget bör du börja få kläm på att navigera upp till förälder-mappen `hello_pi` (med `cd ..`) och sedan ner i en av demomapparna (med `cd hello_something`).
- Prova några av de andra demona på egen hand. `hello_videocube` är ett bra ställe att börja.

[PDF för utskrift](https://gitprint.com/mobluse/demo-programs-sv/blob/master/worksheet.md)
