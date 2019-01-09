# Encoder

An Arduino Rotary Encoder Library that actually works with zero glitches

# Motivation

A lot of discussion can be found on the internet about the best approach to handle an Arduino Rotary Encoder. There are essentially two approaches and every single bit of code that I found is just a variation of the same.

- The simpler approach consists on trying to catch the encoder ticks on the main loop. Of course this doesn't work because as the program gets longer and takes more to complete a loop, encoder ticks start to be missed.

- The second approach is seting up one or more input interrupts that trigger as the Encoder moves. This doesn't work either because there no way to avoid contact bounces on the encoder, thus leading to inacurate or erratic response.

- I also found attempts to filter or eliminate the bounces by hardware implementations such as adding capacitators and other tricks. This may improve results on the second approach mentioned abobe, but it's difficult to find the right balance between bounce removal and quick encoder response.

So, in summary, nothing that I found so far actually works as well as it could be.

# Features

So what I did is to think fresh and used a totally different approach that works. Instead of relying on input interrupts I rely on time interrupts. This enables a robust, yet simple, software implementation that solves all the problems at once and by once. The Encoder library reacts precisely to very fast encoder moves without a miss of a tick and without any bounces, regardless of the lenght or complexity of your other code. It just works! 

Oh, and there's no limit on the number of encoders in use because you can conect them to any available regular inputs and all of them will seamlesly work indepently.

# How it works

You just create an Encoder object initialized to the pins the encoder is connected. Any input pins work, no need to be interrupt pins or anything. Just plain digital inputs. On the setup funcion call the `EncoderInterrupt.begin` function and pass in your encoder object. On the loop function call the `encoder.delta` function to get the encoder variation since the last call. That's it. Here's the code:

```
// define the input pins
#define pinA 19
#define pinB 20
#define pinP 21

// create an encoder object initialized with their pins
Encoder encoder( pinA, pinB, pinP );

// setup code
void setup() 
{
  // start the encoder time interrupts
  EncoderInterrupt.begin( &encoder );
}

// loop code
void loop()
{
  // read the debounced value of the encoder button
  bool pb = encoder.button();

  // get the encoder variation since our last check, it can be positive or negative, or zero if the encoder didn't move
  // only call this once per loop cicle, or at any time you want to know any incremental change
  int delta = encoder.delta();

  // add the delta value to the variable we are controlling
  myEncoderControlledVariable += delta;

  // do stuff with the updated value

  // that's it
}
```

# More

A template code using several encoders is available as a comment in the Encoder.h header file.

