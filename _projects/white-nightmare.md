---
title: "White Nightmare"
repository: "white-nightmare"
header:
  video:
    id: ZzZ8mFEQnbo
    provider: youtube
  teaser: /assets/images/white-nightmare-th.png
excerpt: "When I was a child, I used to have a recurrent nightmare. Now I recreated using some math voodoo."

---

**Warning** <i class="fas fa-exclamation-triangle"></i> Project under construction.
{: .notice--warning}

## The story

### English

I do not remember my own dreams anymore. I only wake up to feel the last shreds of a dream tangling around my pillow, and then I try to munch out the aftertaste while I rouse myself, until it finally vanishes away. Whether that is bitter or sweet it is up to each dream, but they all end up fading out of my memory. Yet there are some recurrent childhood dreams from which I still keep a vivid image.

Almost all these recurrent dreams were nightmares, most of them ridiculously naïve and harmless when thinking retrospectively. Maybe some spooky monster here and there, or images from a horror movie trailer I should not have watched… The usual stuff kids experience at that age. One of them was, nevertheless, particularly haunting, and still nowadays it makes me admire the psyche of my inner pants-shitting ten-year-old. There are no monsters in that dream, nor blood or definite dangers. I cannot even say whether a single person I know shows up there. I do think I am present, even though that presence is intangible, incorporeal, almost indescribable.

Let me try to explain it: This nightmare looks like a fuzzy frame of white noise, this kind of snowy mess you can see on the screen of an old untuned TV. The main difference is the noise pattern, which stretches across fixed lines as a maze does. It actually IS a maze, and I am staring at it from above, like a bird’s eye view. Sound plays a role in this dream too, and it also resembles the crackle of an analog TV. These are all the sensorial details I can provide, as the terror of this white-noise nightmare lies much more on the sensations it conveys. Should I fix my attention on one of the lines, an intuition will rapidly come through to persuade me I am lost in it, buried so deep down that trench that I can barely see myself. And then the sound rises up to a deafening volume, while the image furiously convulses. The evidence of confinement grips me tighter and tighter in this abstract corner of the labyrinth. My family and friends may be hidden in another unreachable region of this black-and-white map. The experience is probably also unpleasant for them, but they are at least together, that is the only thing I know. I am, on the contrary, fully isolated dozens of pixels away. And I ignore whether they miss me, or if they are worried, or trying to find me by all means…

There is no further development of the dream. The sensations I have described just keep building up in a cruel crescendo until anxiety grows unbearable. On the edge of a child’s psychological pain threshold, I suddenly wake up. The deafening white noise starts to settle down, like venomous sea foam that retires from the skin without moistening it. It will still take one or two minutes to untie the knot in the stomach.

### Spanish

Hace mucho que no recuerdo mis propios sueños. Me despierto con sus últimos jirones enredados en la almohada, y mientras me desperezo paladeo el regusto que han dejado (dulce, amargo... según cada cual), hasta que se desvanece por completo. La cosa es que hay sueños recurrentes de mi infancia de los que aún conservo una viva imagen.

Casi todos son pesadillas, la mayoría enternecedoramente inofensivas. Un monstruo por aquí, un tráiler de una peli de miedo que no debí haber visto por allá... Nada que cualquier niño no conozca. Sin embargo, hay una que me causa especial inquietud, aunque también una cierta admiración por la mente febril de un niño cagón. En ese sueño no hay monstruos, ni sangre, ni peligros concretos. Ni tan siquiera sé decir si aparece alguien que conozca. Yo creo que sí estoy presente, aunque es una presencia intangible, incorpórea, casi inefable.

Voy a intentar explicarme: esa pesadilla es un plano de ruido blanco burbujeante, como el de un televisor analógico desintonizado. Se diferencia de éste en que el patron forma una serie de surcos fijos, creando una suerte de laberinto. De hecho ES un laberinto, lo estoy viendo desde arriba, en plano cenital. El sueño también tiene sonido, también el típico chisporroteo de televisor viejo. Sensorialmente no hay muchos más detalles que explicar; lo realmente terrorífico de esta pesadilla de ruido blanco son las sensaciones que transmite. Si me da por fijar la vista en alguno de los surcos, se abre paso en mí la intuición de que yo estoy perdido dentro de él, aunque no alcance a verme. El sonido aumenta entonces de volumen hasta un punto ensordecedor, y la imagen se empieza a agitar violentamente. La evidencia cada vez más cierta de estar aislado en este rincón abstracto del laberinto me atenaza. Estoy solo, atrapado y desvalido, y no encuentro la manera de escapar de esta trampa. Mi familia y mis amigos se encuentran en alguna otra región inaccesible de este mapa en blanco y negro. La experiencia seguro que no es agradable para ellos tampoco, pero al menos están juntos, eso sí que lo sé. Y en cambio yo estoy incomunicado a decenas de pixeles de distancia, y ni tan siquiera sé si me echan de menos, si están preocupados por mí, si están intentando encontrarme...

El sueño no tiene ningún desarrollo. Las sensaciones que describo simplemente van aumentando en un cruel crescendo hasta que la angustia se hace insoportable. Al borde del umbral del dolor psicológico de un niño de 10 años, me despierto al fin. El ruido blanco ensordecedor va reposando, como una espuma de mar venenosa que se aparta sin dejar la piel mojada. Aún tardaran uno o dos minutos en deshacerse el nudo del pecho.

## The math

The nature of the nightmare, based on this concept of a maze emerging out of white noise, lend itself to a
quite clean mathematical model. I just needed a couple of ingredients to bring it together nicely:

- A **maze generation** algorithm
- Some statistical tools to mimic the noisy effects, most of which are related with **autoregressive models**. 


### Maze generation



### Autoregressive models

$$X_t=\alpha X_{t-1}+\left(1-\alpha\right)\epsilon_t$$

TODO

### How white is your noise?

TODO