# Hola, bienvenido al OneInput Tutorial
###### Hi, if you don't get spanish you can follow the images, see the vid and download the demo patches (all that is in eng).

Si todavía no viste el video de demostración que está en YouTube y estás ansiosx por saber qué y cómo se puede hacer:

[![IMAGE ALT TEXT HERE](http://img.youtube.com/vi/xEAfMJ9KS9Q/0.jpg)](http://www.youtube.com/watch?v=xEAfMJ9KS9Q)

Me gusta cómo suena a eso de los 10:30 pero vale la pena mirarlo para darse una idea del proceso y de cómo, en una improvisación, se trabaja para darle forma a lo que va surgiendo. Ahora siguen una serie de imágenes bastante claras de cómo es la cosa. 

## AL TRABAJAR CON EXPRESIONES MATEMÁTICAS Y LÓGICAS, PRÁCTICAMENTE TODO ES APLICABLE A TODO, así es que no tomes este tutorial como si fuera lo único que puede hacerse sino como una base a partir de la cual podés empezar a experimentar por tu cuenta.

__La idea es trabajar solamente con *expr~* a partir de una única señal de entrada,__ algo valioso sobre todo en un contexto de livecoding. En esta carpeta podés encontrar un archivo __copypaste.txt__ que contiene las principales fórmulas que aparecen acá para que no tengas que tipear o recordar todo a la perfección. Si te interesa le vas a ir agarrando la onda y entendiendo mejor.

![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/01.png)

### phasor~

Esta rampa va a ser nuestra One(and only)Input para *expr~*. Podés cambiar los valores alterando la __velocidad, tono y duración del loop__. En este caso, el loop dura 100 segundos. Eecordá que __TODO depende de esta función__, de manera que al volver a empezar se resetean todas las modulaciones también y por eso conviene que sea largo y produzca muchas variaciones antes de recomenzar). 

![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/02.png)
![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/03.png)

### pop

>($v1%5510<10)

Un sample rate standard es de 44100 samples por segundo, en este caso vamos a trabajar con 44080 que es bastante aceptable, el número lo elegí porque es divisible de manera que podamos tener una unidad de tiempo de números enteros. Tenemos, entonces, 4408000 enteros en 0.01Hz. Esto equivale a 100 segundos. Ya que no vamos a trabajar con secuencias ni "partes", la gracia está en poder producir una variación suficientemente interesante a partir de las mismas cosas y eso se logra con ritmos y modulaciones complejxs, por eso necesitamos un tiempo largo de desarrollo para que nuestras fórmulas rindan muchos resultados distintos. __5510 funciona como una semicorchea__, tenemos 800 en un ciclo, creo que es suficiente (aunque más no estarían mal). Al preguntar si el módulo de semicorcheas de la señal es menor que 10 tenemos un resultado así: 1x10 0x5500, algo tipo 

10000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000001000000000000000000000000000000000000000000000000001000000000000000000000000000000000000

![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/04.png)

##### Si entendiste esto vas a entender todo, no hay nada que pueda impedirte terminar esta tutorial. #####

## Formas de onda

Veamos cómo generar distintas formas de onda operando con la señal. 

### saw

>($v1%400/400)

Al dividirla en segmentos pequeños con un módulo conseguimos una frecuencia audible de cambios en la señal, acordate que es una señal que se va incrementando de 0 a 4407999, podemos hacer que esa misma señal incremente -por ejemplo- de 0 a 399 y listo pero __acordate de divir el módulo por sí mismo para normalizar la amplitud y no volar tus oídos o parlantes__ porque lo que vamos a obtener va a ser algo tipo:

012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901

cuando la amplitud máxima debería ser, como mucho, 1.

![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/05.png)

### square

>($v1%400<200)

Esta forma es menos "riesgosa" en términos de amplitud. Con una simple condición ya logramos una onda cuadrada (en este caso, si es menor que 200 va a darnos un 0 y sino un 1, como el módulo es de 400 tenemos un pulso simétrico), podés probar distintos anchos de pulso (la proporción entre su parte 0 y su parte 1) para lograr timbres distintos. Más adelante vamos a ver cómo modular eso. El ejemplo daría algo como:

001100110011001100110011001100110011001100110011001100110011001100110011001100110011001100110011001100110011001100110011001100110011001100110011001100110011001100110011001100110011001100110011001100110011001100110011001100110011001100110011001100110011

![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/06.png)

### sine

>sin($v1/64)

Aunque la onda sinusoidal tiene menos impacto armónico en principio, vamos a ver más adelante que resulta increíblemente valiosa. Sobre todo porque __podemos pasar cualquier cosa por seno y siempre nos va a devolver un resultado interesante entre -1 y 1__.

![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/07.png)

### GATE

>($v1%(5510 * 4)<4000)

Antes de ver cómo se puede resolver el problema de los envolventes tengamos en cuenta que en ciertos contextos podemos prescindir de ellos y que, aún si vamos a usarlos, las compuertas __sirven para controlar el cuándo y el cuánto de cada sonido__. En este caso el cuándo es cada cuatro semicorcheas.

![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/08.png)

__No es más que otra onda cuadrada más lenta (*a tempo*) por la que multiplicamos nuestra forma de onda.__
Podemos hacer lo mismo con una saw por ejemplo:

>($v1%5510/5510)

De hecho eso ya es casi un envolvente decente, creo que debería utilizarlo jaja.

## Modulando
Con unos pocos elementos ya estamos en condiciones de ir al punto de la cosa: las modulaciones.
Modular es todo en este contexto, el desafío es transformar esas ondas monótonas en hermosos desarrollos que nos seduzcan por sus progresiones y a la vez nos cautiven con una cierta inasible complejidad. Bueno, eso es para el libro... 

![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/09.png)

### AM

>(sin($v1/1000) * 0.5+0.5)

Dijimos que *sin()* retorna valores entre -1 y 1, si lo normalizamos a 0 y 1 podemos controlar la amplitud de las ondas y generar __trémolos, fades, polirritmia, etcétera__. Es, otra vez, un elemento muy sencillo pero que rinde un montón. Al principio parece una pavada pero si lo sabemos combinar con otras cosas se vuelve casi el alma, algo imprescindible.

![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/10.png)

De más está decir que podemos usarlo para controlar la amplitud sin disminuirla completamente y lograr un efecto como de __distintas intensidades o matices__. Por ejemplo:

>(sin($v1/2000) * 0.7+0.3)

### Stereo

Aprovechá el stereo (o la cuadrafonía si la tenés) al máximo. Podés __copiar los mismos sonidos con diferentes modulaciones y obtener resultados espaciales muy copados__.

![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/11.png)

### PWM

>($v1 %400>$v1 /100 %200 +50)

Modulando el ancho de pulso de una onda cuadrada conseguís una __barrida de armónicos, desde timbres de chiptune, vibratos, desafinadas e incluso patrones rítmicos notables!__

![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/12.png)

### Ring

Es como una AM pero a frecuencias audibles, sobre todo aquellas que guarden una particular relación con la modulada. También logra __vibratos, detunings, tremolos, etc__.

![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/13.png)

### Glitching loops

>sin($v1%320000 * 9999)

Creo que es mejor escucharlos que explicarlos, tampoco los entiendo muy bien, creo que están relacionados con nuestra especie de samplerate extraño.

![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/14.png)

### FM 2 operadores

¿Te acordás que te dije que no había que subestimar a las ondas sinusoidales? Bueno, cuando empezás a modularlas con otras ondas sinusoidales obtenés los resultados más ricos y complejos, siempre __siendo posible seguir modulando a niveles cada vez más locos__.

>sin($v1/64+sin($v1/128) * 10)

![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/15.png)

__Probá también modular, por ejemplo, la amplitud con una frecuencia a su vez modulada!__

### FM 3 operadores

>sin($v1/64+sin($v1/32) * sin($v1/2000) * 20)

![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/16.png)

### FM 4 operadores

>sin($v1/64+sin($v1/32) * sin($v1/2000) * sin($v1/200000) * 50)

![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/17.png)

### FM 5 operadores

Te advertí que no tiene fin...

>sin($v1/64+sin($v1/32) * sin($v1/2000+sin($v1/10000)) * sin($v1/200000) * 50) * 0.1

![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/18.png)

## Instant Insane IDM (Interludio)

*Un pequeño excurso musical...*

>sin($v1/32+sin($v1/32) * sin($v1/2100+sin($v1/10000)) * sin($v1/201000) * 50) * (sin($v1/1000) * 0.5+0.5) * ($v1%5510<1500) * 0.1; sin($v1/32+sin($v1/32) * sin($v1/2000+sin($v1/15000)) * sin($v1/200000) * 50) * (sin($v1/2500) * 0.5+0.5) * ($v1%5510<900) * 0.1

![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/19.png)

![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/20.png)

## Envolventes

En términos de amplitud es aconsejable trabajar con elevaciones *pow()*. La importancia de los envolventes en este caso particular es que __si no suavizamos un poco los ataques y relajamientos generamos un montón de pops, clicks y cosas que explotan de armónicos y daños colaterales__. Esta es una posible solución (para nada perdecta igual, es un tema que me preocupa un poco):

>if($v1%5510<5, pow($v1%5/5, 2), pow(1-($v1%5510/5510), 3))

![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/21.png)

## Tips

Algunos tips finales de arpeggios y glitches que te pueden interesar:

>($v1 /5510 %8 * 8)

>sin($v1%150)

>sin(8 * $v1%10)

![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/22.png)

## Generatividad

Aunque salgamos de la consigna inicial,incluso haciendo livecoding, nada nos impide __utilizar señales aleatorias para jugar un poco más__ (en este caso usamos *ruido* -1 1, lo normalizamos a 0 1 y lo sostenemos en sincro con el loop con *samphold~*):

![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/23.png)

## Bye :)

Espero que te haya copado el tutorial y, sobre todo, que haya resultado inspirador. __Te aliento a experimentar por tu cuenta y escribirme__. Hay otros videos relacionados en mi canal de YouTube, chusmealo. Chau :)

![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/24.png)

