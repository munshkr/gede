# OI (One Input) Tutorial

## Descripción

Hola! Armé este tutorial para __hacer livecoding en Pure Data usando muy pocos objetos pero puede servir para componer, aprender síntesis o incluso armar obras generativas__. Básicamente necesitamos un *phasor~* y un *expr~* para procesar de distinta manera su señal, generando distintos tipos de síntesis y variaciones. Todo esto es producto de varios años de trabajo en casa y en vivo, de prueba y error, y espero que le ahorre tiempo a quien le interese para que pueda partir de una base y mejorarlo!

## ¿Por qué Pure Data?

Existen muchísimos lenguajes para hacer livecoding que son excelentes y no sería justo decir que en general ninguno es mejor que otro. Cada unx elije el que le gusta por algo muy específico. __En mi caso me interesa mucho la síntesis y la experimentación, por eso Pure Data es lo que más me satisface.__ 

## Los elementos básicos

Para arrancar necesitamos conectar un *phasor~* a un multiplicador. Vamos a poner un *phasor~ 0.01* a un * ~ 4408000. Esto va a generar una rampa de 0 a 4407999, o sea que el largo total del live-loop va a ser de *100 segundos*. Nada mal si consideramos que vamos a ir haciendo modificaciones mínimas sobre la marcha, que está muy cercano a una calidad óptima de 44100 samples por segundo y -sobre todo- __las modulaciones pueden generar entonces casi 2 minutos de variaciones sin repetición a partir de dos o tres ideas básicas__. A la salida del multiplicador conectamos un *expr~* que es donde la magia va a ocurrir, y lo demás es ir al *dac~*. Es aconsejable poner un clip~ -1 1 a la salida del expr~ para no mandar señales de mucha amplitud por error.

![](https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/start.jpg)

## Fórmulas everywhere

Ya que todo lo vamos a hacer con *expr~*, tengan en cuenta que de aquí en más vamos a generar distintos sonidos con fórmulas aplicadas a esa función lineal. Algunas cuestiones generales para tener en cuenta son: 

__0.01__ 1 pulso por segundo, si lo modificamos cambia el tiempo y el pitch.

__5510__ es una suerte de semicorchea (tendríamos diez compases de 2/4).

__$v1__ es el valor actual que devuelve la función *phasor~*.

__%__ el módulo lo es todo, lo vamos a usar un montón para subdividir la función.

__*__ la multiplicación va a separar distintos términos de las fórmulas, generalmente implica amplitud.

__sin()__ seno, como si fuera un *osc~*, importantísimo sobre todo para las modulaciones.

__<>|&=, etc__ los condicionales y booleanos son fundamentales.

Probemos qué pasa, por ejemplo, 
> expr~ ($v1 %5510 == 0) * 0.1

__y... listo! Tenemos un lindo pop en semicorcheas__. Si entendés por qué ya podés ser unx genix del glitch livecoding con Pd.

## Esquema general

Para resumir las cosas podemos pensar en un esquema general que sería __(forma de onda) * (gate) * (modulación) * amplitud__. 
Más adelante vamos a ver cómo generar un elvonvente más prolijo, por ahora esto nos basta. Sigamos con las formas de onda cuidando la amplitud y después vamos a modulaciones...

## Saw : diente de sierra

Generar una onda dentada es facilísimo porque nuestra función *ya es* una dentada ascendente. Nada más tenemos que subdividirla y normalizarla:

> expr~ ( $v1 %200 / 200 ) * 0.1

Cuanto más pequeña es la subdivisión, más agudo será el pitch, ya que la frecuencia será más rápida. Probá modificando el 200 por otro valor, __no te olvides de siempre dividirlo por el mismo valor__.

## Square : onda cuadrada

Una cuadrada son ceros y unos, una condición y ya está:

> expr~ ( $v1 %200 > 100) * 0.1

Podemos probar distintos anchos de pulso que no sean simétricos y escuchemos cómo varía el resultado armónico. 
__Recordá que el ancho del pulso no puede ser mayor que el módulo ( % )__.

## Sinusoide

> expr~ sin( $v1 / pitch ) * 0.1 

Para __mantener la misma relación armónica entre sinusoides y dentadas/cuadradas podemos usar múltiplos de 8 para las primeras y de 50 para las segundas__. $v1 %200 / 200 (50 * 4) es la misma nota que sin( $v1 /64 ) o ( $v1 /32 ) (8 * 8 y 8 * 4). 

## Gates

Antes de aprender cómo producir modulaciones es importante saber cómo hacer gates. Por cuestiones de simplicidad vamos a usar gates como si fueran envolventes. Probemos qué pasa si agregamos a alguna de nuestras ondas lo siguiente: 

>expr~ (sine($v1)) * ($v1 %5510 < 2205 )

Dado que 5510 es la semicorchea, la mitad del tiempo la condición es verdadera y la otra mitad falsa. Hay muchísimas combinaciones posibles. Algo simple: podemos usar *$v1%(5510 * 8)<5510* para que la condición sea verdadera durante la primera semicorchea de un grupo de 8 (blanca).

## Modulaciones

Ahora que tenemos un sonido y su "envolvente" podemos probar algunas modulaciones. Lo más fácil para empezar es modular la amplitud:

>expr~ sin (8 * $v1 / 10 %10) * ($v1 %5510 < 2205 ) * (sin ($v1 /3000) * 0.5 + 0.5 ) * 0.1

Listo! Modulamos la amplitud con una sinusoide muy lenta, al estilo *LFO*. * 0.5 + 0.5 normalizan el resultado ya que la función *sin* devuelve entre 1 y -1.

__El poder de la AM: Tan sólo con estos pocos elementos ya podemos generar un loop de casi dos minutos sin repetición ya que lo aconsejable es desfazar la modulación respecto del pulso, provocando una polyrythmical madness!!!__

## in STEREO

Las expresiones pueden separarse con ;. Cada expresión se corresponde con un outlet del objeto. Un recurso valioso es aprovechar el stereo por ejemplo: copiemos la línea anterior pero modifiquemos la frecuencia de modulación y hagamos que cada outlet vaya a un canal distinto.

>expr~ sin (8 * $v1 / 10 %10) * ($v1 %5510 < 2205 ) * (sin ($v1 /3000) * 0.5 + 0.5 ) * 0.1;
sin (8 * $v1 / 10 %10) * ($v1 %5510 < 2205 ) * (sin ($v1 /4000) * 0.5 + 0.5 ) * 0.1

## Aquellos buenos tiempos : PWM

¿Alguien dijo chiptune?

>expr~ ($v1 %200 > $v1 /1000 %100) * 0.1

Esta técnica puede utilizarse también, por ejemplo, para agrandar o achicar las duraciones de los gates.

## Ring

Si modulamos la amplitud a frecuencias audibles tenemos *ring modulation* 
>expr~ ($v1 %100 /100) * (sin($v1 /65)) * 0.1

## FM

La síntesis de frecuencia modulada es explotable a muchísimos niveles. Dado que podemos usarla para producir formas de onda muy ricas en armónicos y modulaciones, siempre con la opción de volverla más compleja agregando operadores.

>expr~ (sin($v1 /64 + sin ($v1 /128) * 10) * 0.1

Ahí tenemos una FM de dos operadores de razón 1:2. Podemos cambiar de razón y amplitud para obtener distintos resultados tímbricos. Cuando modulamos con amplitud muy alta producimos ruido (muy útil para percusión):

>expr~ sin($v1 /64 + sin ($v1 /128) * 9999) * 0.1

También agregar otro operador que module, por ejemplo, a la amplitud:

>expr~ sin($v1 /64 + sin($v1 /128) * (sin($v1 /44080) * 20)) * 0.1

¿Qué tal cuatro operadores?

>expr~ (sin($v1 /64 + sin($v1 /128) * (sin($v1 /44080) * sin($v1 /440800) * 222))) * 0.1

## ONE-LINE IDM TRACK !

Agregando un gate y haciendo las debidas modificaciones ya tenemos un track de IDM en una fórmula:

>expr~ (sin($v1 /128 + sin($v1 /512) * (sin($v1 /3000) * sin($v1 /10000) * 1999))) * ($v1 %5510 < 700)

Démosle un poco de complejidad y estéreo:

>expr~ (sin($v1/64+sin($v1/512) * (sin($v1/3000) * sin($v1/10000) * 1999))) * ($v1%5510<700) * (sin($v1/6000) * 0.5+0.5);
(sin($v1/128+sin($v1/512) * (sin($v1/3000) * sin($v1/10000) * 1999))) * ($v1%5510<700)* (sin($v1/4000) * 0.5+0.5)

*;) para los amantes de Aphex*

## Ahora sí: Envolventes

Si sobrevivimos a la FM ya podemos pasar a los envolventes. Son importantes para evitar los pops que resultan de multiplicar la amplitud de 0 a 1 constantemente como hacemos con los gates. Necesitan ser manejados de manera particular, multiplicando la señal que queremos envolver por un if:

>if($v1 %figura < ataque, pow($v1 %ataque/ataque, exponente), pow(1-($v1 %figura/figura), exponente))
>if($v1 %5510 < 10, pow($v1 %10/10, 2), pow(1-($v1 %5510/5510), 5))

En el ejemplo hay un envolvente de ataque rápido en semicorcheas. Cuanto más grande sea el exponente, más pronunciada será la curva de amplificación. *No te asustes, según el resultado buscado y la premura de la ejecución en vivo, podemos prescindir totalmente de los envolventes y trabajar únicamente con los gates.

## Tips

### Kick pop
Si buscamos un resultado bailable es imprescindible un kick bien duro y marcado en negras. No se me ocurre una manera más simple y rápida que 
>expr~ ($v1 %(5510 * 4) < 200) * 0.1
o podemos desarmarlo un poco para que no queme tanto la cabeza:
>expr~ ($v1% (5510 * 16) %(5510 * 7) < 10) * 0.1
>expr~ ($v1% (5510 * 32) %(5510 * 6) < 10) * 0.1
>expr~ ($v1% (5510 * 16) %(5510 * 3) < 10) * 0.2
>expr~ ($v1% (5510 * 32) > (5510 * 24) && $v1% 5510 < 25) * 0.2

la duración de la condición nos va a dar distintos timbres y profundidades. Ya que *la operación es bastante violenta recomiendo meterlo y regularlo con cuidado*. 

### *Glitching bells*
Si usamos *sin* y en lugar de *dividir* la señal para obtener una frecuencia la *multiplicamos*, obtenemos un hermoso glitch:
>expr~ sin($v1 * 1024) * 0.1
Para loopear la parte que queremos agregamos un módulo:
>expr~ sin($v1 %200000 * 1024) * 0.1
En el siguiente tip vamos a conseguir un resultado parecido desde otro lugar pero también con *sin*.

### Arpegios, secuencias y *más glitches*
Al dividir la señal y pasarla por un módulo ($v1/...%...) podemos armar secuencias. Probemos:

>expr~ ($v1% ($v1 /5510 %7 * 100) < 50) * 0.05

>expr~ ($v1% ($v1 /5510 %4 * 50) < 25) * 0.05

>expr~ ($v1%($v1/5510 %10%6 * 50) /500) * 0.05

>expr~ ($v1%($v1/5510%8 * 50) / ($v1 /5510 %8 * 50)) * 0.05

>expr~ ($v1%($v1/2500%100) / ($v1/2500%100)) * 0.025

Y a velocidades absurdas con *sin*, prescindiendo de la división, el resultado te puede interesar...

>expr~ sin (8 * $v1 %10) * 0.05

>expr~ sin (20 * $v1 %50) * 0.05

>expr~ sin (4 * $v1 %30) * 0.05

>expr~ sin (32 * $v1 %600) * 0.05

>expr~ sin (32 * $v1 %110%32) * 0.05

### Generatividad

El live-loop de 100 segundos ya produce bastantes variaciones además de las que podemos ir introducionendo en vivo pero si queremos agregar elementos de generatividad podemos usar más de una señal. Por ejemplo usando varios *phasor~* conectados a nuestro *expr~* a través de un *samphold~* sincronizado al loop principal.

## Copipasteá!
Acá te dejo las fórmulas vacías para que puedas copipastearlas y combinarlas de manera más rápida:

Saw
>($v1%.../...)

Square
>($v1%...>...)

Sine
>(sin($v1/...))

PWM
>($v1%...>$v1/...%...)

FM 2 op
>(sin($v1/...+sin($v1/...) * ...))

FM 3 op
>(sin($v1/...+sin($v1/...) * (sin($v1/...) * ...)))

FM 4 op
>(sin($v1/...+sin($v1/...) * (sin($v1/...) * sin($v1/...) * ...)))

Gate
>($v1%(5510 * ...)<(5510 * ...))

AM
>(sin($v1/...)* 0.5 + 0.5)

Envelope semicorchea
>if($v1%5510<..., pow($v1%.../..., 2), pow(1-($v1%5510/5510), 4))

[1]: https://raw.githubusercontent.com/gabochi/gede/master/OneInputTutorial/readme.md##Descripción
