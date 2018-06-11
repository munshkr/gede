# No oscillators (bytebeat inspired) tutorial

Do you know *bytebeat?* If not, you should. It's a very cheap and minimalistic way of coding music in a simple way, working only with a function of time. The results of the formula go directly to the analog conversor, ussualy is writen in 8-bit, reading each ascii character (0-255) as a sample, an speaker possition. Check it out, there are many documentations and software (you can do it even from the console with python commands).

Well, since I do livecoding and I like to mess with synthesis, much general aspects of this technique are very helpfull and I did an example video with *Pure Data*, check https://youtu.be/eVdi0i-4WLk.  Here I'll explain the basics more or less in the same order of appearence than in the vid.

## Time ramp

First you should make the time ramp (the only input that is required). I do it with *phasor~*. In the vid I count to 64000 in 0.125 Hz. These values give you the speed and pitch of the loop. Although it is easier to work with small numbers, you should keep in mind that the greater the number is, the higher is the quality and pitches you can generate.

## Boolean pop

For the rest we will use only the *expr~* object to operate the input signal (*$v1*). It works like any C or similar expression, combining conditionals, booleans and math. So if you do this:

###### $v1%8000 == 0

you will hear a pop for every "pattern", since it returns 1 each time the modulus resets (8 times in one of our 64000 loop). If you got this, there should be no problem understanding the rythmic aspect of it.

## Square waves

The pop is a very rare square wave 'cause it has alsmost no pulse width. It is something like 10000000000000000000000000000100. But in order to hear something like a pitch you should make the cycle shorter and the width more symetrical. Something like 111000111. You do it with smaller numbers and conditionals:

###### $v1%100 < 50

This would make a perfect symetrical square wave at a low pitch in our 64000 in 0.125Hz loop (half of the times it is 1, and half 0). 

## Amplitude

In the video you'll notice that I ussually use the *(wave) * (envelope) * amplitude* formula.  You can give it some rythm by using modulus in subdivisions of the counter. 8000 would be like a pattern, 4000 could be the floor beat (commonly the kick drum), etc. So if you use:

###### ( $v1%100<50 ) * ( $v1%8000 < 4000 )

the amplitude of the square bass we made will be 1 the first half of the pattern and 0 the other half. But say you want it to sound at the beginning of the pattern but with a shorter duration. Ok, you just decrease the condition like $v1%8000<1000. There are lots of crazy combinations you can do, like $v1%16000%3000 (it will go off beat but always to the floor each 2 patterns).

And by adding a final multiplication you can controll the amplitude (normally from 0 to 1).

## PWM (Pulse width modulation)

In the example above, the width of the square is fixed in the half but you can modulate it to get a richer harmonic result. Check this:

###### $v1%100 < ( $v1/100 * 100 )

Now the condition changes over the time, making the width longer.  Try different divisors and proportions like 

###### $v1%100 < ( $v1/50 * 40 + 40 )

## Bytecode saw wave

Your input is an increasing number, so it is already a saw wave. *But carefull with the amplitude!* $v1%1000 should be normalized * 0.01 or less if you don't want to blow your speakers with a 1000 amplitude killer saw. It would be a great idea to put a *clip~ -1 1* before the *dac~.

## Sines!!!

sin($v1) makes a sine wave and you can change the pitch by multiplying (* 0.5, one octave lower, * 2 higher, etc). But what's juicy of sines is that you can use them to operate anything very fluidly, like LFOs and get some AM, FM or whatever:

###### ( $v1%1000 == 0 ) * sin( $v1 * 0.001 )                         The pop sound with some amplitude 

###### sin( $v1 * 0.2 + sin ($v1 * 0.6) )                             Two operator FM (1:3)

###### sin( $v1 * 0.2 + sin ($v1 * 0.6) * (sin ($v1 * 0.0001) * 20)   The same but plus one more operator index modulator

*Probably noticed in the vid that FM high modulation index goes very noisy so you can make some crazy hihats and snares:

###### sin($v1+sin($v1) * 999)

Well, this was just a quick guide to some stuff you can do. Now go and experiment, make some noise and share!


### If you are somewhere near Buenos Aires, see you in the next livecoding event ;)
