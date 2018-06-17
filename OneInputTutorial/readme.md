<a href="http://www.youtube.com/watch?feature=player_embedded&v=xEAfMJ9KS9Q" target="_blank"><img src="http://img.youtube.com/vi/xEAfMJ9KS9Q/0.jpg" 
alt="IMAGE ALT TEXT HERE" width="240" height="180" border="10" /></a>

[![IMAGE ALT TEXT HERE](http://img.youtube.com/vi/xEAfMJ9KS9Q/0.jpg)](http://www.youtube.com/watch?v=xEAfMJ9KS9Q)

# Hola!
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/01.png)
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/02.png)
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/03.png)
### pop
($v1%5510<10)
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

