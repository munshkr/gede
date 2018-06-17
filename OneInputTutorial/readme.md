# Hola, bienvenido al OneInput Tutorial de livecoding con expr~
###### Hi, if you don't get spanish you can follow the images, see the vid and download the demo patches (all that is in eng).
Si todavía no viste el video de demostración que está en YouTube y estás ansiosx por saber qué se puede hacer con lo que voy a explicar acá, chequeá:
[![IMAGE ALT TEXT HERE](http://img.youtube.com/vi/xEAfMJ9KS9Q/0.jpg)](http://www.youtube.com/watch?v=xEAfMJ9KS9Q)
Recién me gusta cómo suena a eso de los 10:30 pero vale la pena. Sobre todo para darse una idea del proceso y cómo, en una improvisación, se trabaja para darle forma a lo que va surgiendo. Ahora siguen una serie de imágenes bastante claras de cómo es la cosa. En esta carpeta podés encontrar un archivo __copypaste.txt__ que contiene las principales fórmulas que aparecen acá para que no tengas que tipear o recordar todo a la perfección. Si te interesa le vas a ir agarrando la onda y entendiendo mejor.
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/01.png)
### phasor~
Esta rampa va a ser nuestra One(and only)Input para *expr~*. Podés cambiar los valores alterando la __velocidad, tono y duración del loop__. En este caso, el loop dura 100 segundos. Eecordá que __TODO depende de esta función__, de manera que al volver a empezar se resetean todas las modulaciones también y por eso conviene que sea largo y produzca muchas variaciones antes de recomenzar). 
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/02.png)
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/03.png)
### pop
>($v1%5510<10)
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/04.png)
## Formas de onda
### saw
($v1%400/400)
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/05.png)
### square
($v1%400<200)
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/06.png)
### sine
sin($v1/64)
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/07.png)
### GATE
($v1%(5510 * 4)<4000)
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/08.png)
## Modulando
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/09.png)
### AM
(sin($v1/1000) * 0.5+0.5)
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/10.png)
### Stereo
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/11.png)
### PWM
($v1 %400>$v1 /100 %200 +50)
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/12.png)
### Ring
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/13.png)
### Glitching loops
sin($v1%320000 * 9999)
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/14.png)
### FM 2 operadores
sin($v1/64+sin($v1/128) * 10)
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/15.png)
### FM 3 operadores
sin($v1/64+sin($v1/32) * sin($v1/2000) * 20)
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/16.png)
### FM 4 operadores
sin($v1/64+sin($v1/32) * sin($v1/2000) * sin($v1/200000) * 50)
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/17.png)
### FM 5 operadores
sin($v1/64+sin($v1/32) * sin($v1/2000+sin($v1/10000)) * sin($v1/200000) * 50) * 0.1
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/18.png)
## Instant Insane IDM (Interludio)
sin($v1/32+sin($v1/32) * sin($v1/2100+sin($v1/10000)) * sin($v1/201000) * 50) * (sin($v1/1000) * 0.5+0.5) * ($v1%5510<1500) * 0.1;
sin($v1/32+sin($v1/32) * sin($v1/2000+sin($v1/15000)) * sin($v1/200000) * 50) * (sin($v1/2500) * 0.5+0.5) * ($v1%5510<900) * 0.1
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/19.png)
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/20.png)
## Envolventes
if($v1%5510<5, pow($v1%5/5, 2), pow(1-($v1%5510/5510), 3))
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/21.png)
## Tips
+ ($v1 /5510 %8 * 8)
+ sin($v1%150)
+ sin(8 * $v1%10)
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/22.png)
## Generatividad
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/23.png)
## Bye :)
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/24.png)

