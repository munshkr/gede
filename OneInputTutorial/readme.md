# OI (One Input) 
# Tutorial para hacer livecoding con Pure Data

## Descripción

Hola! Armé este tutorial para __hacer livecoding en Pure Data usando muy pocos objetos__ pero puede servir para componer, aprender síntesis o incluso armar obras generativas. Básicamente necesitamos un *phasor~* y un *expr~* para procesar de distinta manera su señal, generando distintos tipos de síntesis y variaciones. Todo esto es producto de varios años de trabajo en casa y en vivo, de prueba y error, y espero que le ahorre tiempo a quien le interese para que pueda partir de una base y mejorarlo!

## ¿Por qué Pure Data?

Existen muchísimos lenguajes para hacer livecoding que son excelentes y no sería justo decir que en general ninguno es mejor que otro. Cada unx elije el que le gusta por algo muy específico. __En mi caso me interesa mucho la síntesis y la experimentación, por eso Pure Data es lo que más me satisface.__ 

## Los elementos básicos

Para arrancar necesitamos conectar un *phasor~* a un multiplicador. Vamos a poner un *phasor~ 0.01* a un * ~ 4408000. Esto va a generar una rampa de 0 a 4407999, o sea que el largo total del live-loop va a ser de *100 segundos*. Nada mal si consideramos que vamos a ir haciendo modificaciones mínimas sobre la marcha, que está muy cercano a una calidad óptima de 44100 samples por segundo y -sobre todo- __las modulaciones pueden generar entonces casi 2 minutos de variaciones sin repetición a partir de dos o tres ideas básicas__. A la salida del multiplicador conectamos un *expr~* que es donde la magia va a ocurrir, y lo demás es ir al *dac~*. 

__Es aconsejable poner un clip~ -1 1 a la salida del expr~ para no mandar señales de mucha amplitud por error.__

## Fórmulas everywhere

Ya que todo lo vamos a hacer con *expr~*, tengan en cuenta que de aquí en más vamos a generar distintos sonidos con fórmulas aplicadas a esa función lineal. Algunas cuestiones generales para tener en cuenta son: 

__0.1__ 1 pulso por segundo, si lo modificamos cambia el tiempo y el pitch.

__5510__ es una suerte de semicorchea (tendríamos diez compases de 2/4).

__$v1__ es el valor actual que devuelve la función *phasor~*.

__%__ el módulo lo es todo, lo vamos a usar un montón para subdividir la función.

__*__ la multiplicación va a separar distintos términos de las fórmulas, generalmente implica amplitud.

__sin()__ seno, como si fuera un *osc~*, importantísimo sobre todo para las modulaciones.

__<>|&=, etc__ los condicionales y booleanos son fundamentales.

Probemos qué pasa, por ejemplo, si __llenamos el expr~ con $v1%5510==0 y... listo! Tenemos un lindo pop en semicorcheas__. Si entendés por qué ya podés ser unx genix del glitch livecoding con Pd.

## Esquema general

Para resumir las cosas podemos pensar en un esquema general que sería __(forma de onda) * (gate) * (modulación) * amplitud__. Más adelante vamos a ver cómo generar un elvonvente más prolijo, por ahora esto nos basta.

## Saw : diente de sierra

Generar una onda dentada es facilísimo porque nuestra función *ya es* una dentada ascendente. Nada más tenemos que subdividirla y normalizarla:

__($v1% PITCH / PITCH)__ recomendado: PITCH = 50 100 150 200 250 300 etc 

Cuanto más pequeña es la subdivisión, más agudo será el pitch, ya que la frecuencia será más rápida. *$v1%200/200* produce un tono más o menos medio como referente.

## Square : onda cuadrada

Una cuadrada son ceros y unos, una condición y ya está:

__($v1% PITCH > WIDTH)__ *WIDTH < PITCH*

Podemos probar distintos anchos de pulso y escuchar cómo varía el resultado armónico. *$v1%200>100* produce una onda simétrica.

## Sinusoide

__sin($v1 / PITCH)__ recomendado PITCH = 8 16 32 64 128 etc

#### Para mantener la misma relación armónica entre sinusoides y dentadas/cuadradas podemos usar múltiplos de 8 para las primeras y de 50 para las segundas. $v1%200/200 (50 * 4) es la misma nota que sin($v1/64) o sin($v1/32) (8*8 y 8*4). 

## Gates

Antes de aprender cómo producir modulaciones es importante saber cómo hacer gates. Por cuestiones de simplicidad vamos a usar gates como si fueran envolventes. Probemos qué pasa si agregamos a alguna de nuestras ondas lo siguiente: __(onda) * ($v1%5510 < 2205)__ 

Dado que 5510 es la semicorchea, la mitad del tiempo la condición es verdadera y la otra mitad falsa. Hay muchísimas combinaciones posibles. Algo simple: podemos usar __$v1%(%5510 * 8) < ($v1%(5510 * 1))__ para que la condición sea verdadera durante la primera semicorchea del pulso de negras.

## Modulaciones

Ahora que tenemos un sonido y su "envolvente" podemos probar algunas modulaciones. Lo más fácil para empezar es modular la amplitud:

__(onda) * (gate) * (sin($v1/3000) * 0.5 + 0.5)__

Listo! Modulamos la amplitud con una sinusoide muy lenta, al estilo LFO. * 0.5 + 0.5 normalizan el resultado ya que la función *sin* devuelve entre 1 y -1.
#### El poder de la AM: Tan sólo con estos pocos elementos ya podemos generar un loop de casi dos minutos sin repetición ya que lo aconsejable es desfazar la modulación respecto del pulso, provocando una polyrythmical madness!!!

# to do:

## in STEREO

## Ring

## FM

## Ahora sí: Envolventes

## Generatividad

## Tips

-- secuencias de notas
-- acordes ruidosos
-- otras secuencias
-- pop kick (<)400
-- sdgfsdg43t443gdgfdf

## Quick refference

Saw
expr~ ($v1%   /   )

Square
expr~ ($v1%   >   )

Sine
expr~ sin($v1/  )

PWM
expr~ $v1%   >$v1/   %

FM 2 op
expr~ sin($v1/   +sin($v1/   )*   )

FM 3 op
expr~ sin($v1/   +sin($v1/   )* (sin($v1/    )*  ))

Gate
expr~ $v1%(5510* )<(5510* )

AM
expr~ sin($v1/    )*0.5+0.5

Ring
expr~ sin($v1*    )*0.5+0.5

Envelope
expr~ if($v1%5510<  , pow($v1%  /  , 2), pow(1-($v1%5510/5510), 4))
