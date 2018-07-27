#rampcode

#### rampcode is still beta, so this is a really quick guide. You'll probably get suplementary help by checking the example.txt file.

*rampcode* it's an structure for livecoding in pd that I made inspired in the harcore one-line expressions from bytebeat style. Its core is very simple: EVERYTHING is a function of TIME. You need to open the **rampcode** patch in pd and a text file in your favorite editor. The patch will receive msgs via qlist object and there are two receivers: **ramp** and **code**.

**ramp** will set the speed (cycles per second) and **code** will send the audio to the mixer.

Write this on the text file and save it:

*ramp 0.8;*

Let's say your file is called **file.txt**. Now you need to click **open** at the right of the mixer and select **file.txt**. To run the code click the **read** button on the left. If everything is ok, the ramp value should be updated on the ramp box.

Now make some noise. Add this on the text file, save it and run the code:

*code 1 sin($v1) 4 2, end;*

If your dsp is on and you raise the channel 1 on the mixer, you should hear a very annoying beep playing with a boring rythm. Ok, ¿what the hell does this code mean? The syntax will always be:

*code (channel) (synthesis) (subdivission) (envelope curve), end;*

You can put all channels togheter like this:

*code*
*(channel) (synthesis) (subdivission) (envelope curve),*
*(channel) (synthesis) (subdivission) (envelope curve),*
*(channel) (synthesis) (subdivission) (envelope curve),*
*(channel) (synthesis) (subdivission) (envelope curve),*
*end;*

even

*code*
*
*(channel)*
*(synthesis)*
*(subdivission)*
*(envelope curve)*
*,*
*(channel)*
*(synthesis)*
*(subdivission)*
*(envelope curve)*
*,*
*end;*

and that kind of thing. Believe me, you'll probably need it further...

##The magic begins
Since pd deals with real time dsp, the good part is that you can play with all kind of crazy synthesis expressions. The bad part is the same thing, no default parameters, presets, instruments, whatever. This is experimental hardcore shit. Let's go!

##Expressions
All the structure is based on the **expr~** object. This means that you can use all logic and math operations such as **% / * - + > < | & () sin pow min max** etc etc. Two of the four code arguments can be expressions: **synthesis** and **subdivission**.

###synthesis expression
Remember that everything is a function of time. Time is a ramp, an increasing value that goes to 44100 in every cycle (to be precise goes from 0 to 4409999 in 100 cycles). So, like in bytecode, you just mess with that. Remember that *$v1* is the actual value of the ramp.

**$v1%100/100 This is a sawtooth wave**

**$v1%100<50 This is an square wave**

**sin($v1/16) This is a sine wave**

**sin($v1/16)*sin($v1/2000) This is AM**

**sin($v1/48+sin($v1/32)*10) This is a 2 op FM**

**sin($v1/($v1/(t/8)%8)) This is an arpeggio**

*t means 44100 but shorter :) use it in any expression*

Okey. That's all. If you got this, you can do all kind of weird synths.

##subdivissions and envelopes
Subdivissions are simple. 1 means each cycle, 2 means twice, etc. Keep in mind that you can use decimals too. So 1.5 means once each one and a half cycle, 0.25 means once each four cycles. And have fun changing the subds with expressions like:

**4+($v1/t%4)**
**8+($v1%(t*2)<t)*8**

Envelopes are even simpler, they are powers so a bigger number just exagerate the curve. So far, the envelope curve is fixed with a quick attack and long release (take a look inside *ramp* object).

##BONUS TRACK
Plus **t** you can use **$v2** in your expressions. $v2 is a random number in 0-1 range that is triggered according to the subdivission. Check this improve on the arpeggio example:

**sin($v1/int($v2*8+1)) 8+($v1%(t*2)<t)*8 2**

