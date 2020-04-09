---
title: "White Nightmare"
repository: "white-nightmare"
header:
  video:
    id: ZzZ8mFEQnbo
    provider: youtube
  teaser: /assets/images/white-nightmare-th.png
excerpt: "When I was a child, I used to have a recurrent nightmare. Now I recreated it using some math voodoo."

---

**Warning** <i class="fas fa-exclamation-triangle"></i> Project under construction.
{: .notice--warning}

## The story

**Note** _This is a translation of the original story I wrote about my recurrent nightmare in Spanish. You can find the original text [here](../../pesadilla-blanca)._
{: .notice--info}

I do not remember my own dreams anymore.
I only wake up to feel the last shreds of a dream tangling around my pillow,
and then I try to munch out its aftertaste while I rouse myself, until it finally vanishes away
 Whether that taste is bitter or sweet, it is up to each dream, but they all end up fading out of my memory.
There are, however, some recurrent childhood dreams from which I still keep a vivid image.

Almost all these recurrent dreams were nightmares, most of them ridiculously naive and harmless when thinking retrospectively. Maybe some spooky monster here and there, or images from a horror movie trailer I should not have watched… The usual stuff kids experience at that age. One of them was, nevertheless, particularly haunting, and still nowadays it makes me admire the psyche of my inner pants-shitting ten-year-old. There are no monsters in that dream, nor blood or definite dangers. I cannot even say whether a single person I know shows up there. I do think I am present, even though that presence is intangible, incorporeal, almost indescribable.

Let me try to explain it: This nightmare looks like a fuzzy frame of white noise,
this kind of snowy mess you can see on the screen of an old untuned TV.
The main difference is the noise pattern, which stretches across fixed lines as a maze does.
It actually IS a maze, and I am staring at it from above, like a bird’s eye view.
Sound plays a role in this dream too, and it also resembles the crackle of an analog TV.
These are all the sensorial details I can provide,
as the terror of this white-noise nightmare lies much more on the sensations it conveys.
Should I fix my attention on one of the lines,
an intuition will rapidly come through to persuade me I am lost in it,
buried so deep down in that trench that I can barely see myself.
And then the sound rises up to a deafening volume,
while the image convulses furiously.
The evidence of confinement grips me tighter and tighter in this abstract corner of the labyrinth.
My family and friends may be hidden in another unreachable region of this black-and-white map.
The experience is probably also unpleasant for them, but they are at least together,
that is the only thing I know. Unlike them, I am fully isolated, dozens of pixels away from them.
And I ignore whether they miss me, or if they are worried, or trying to find me by all means…

There is no further development of the dream. The sensations I have described just keep building up in a cruel crescendo until anxiety grows unbearable. On the edge of a child’s psychological pain threshold, I suddenly wake up. The deafening white noise starts to settle down, like venomous sea foam that retires from the skin without moistening it. It will still take one or two minutes to untie the knot in the stomach.

## The math

The nature of the nightmare, based on this concept of a maze emerging out of white noise, lends itself to a
quite clean mathematical model. I just needed a couple of ingredients to bring it together nicely:

- A **maze generation** algorithm.
- Some statistical tools to mimic the noisy effects, most of which are related with **autoregressive models**. 

Both concepts sink their roots into **probability theory**. This means that they operate randomly according
to some probability distribution; every time you generate a maze, it will be a different than the preovious one.
Likewise, everytime I simulate my nightmare it will be similar but not the same one.
That is exactly the way it would also happen during my childhood, every four or five months. 

{% include figure image_path="https://media.githubusercontent.com/media/rodrihgh/white-nightmare/master/media/white_nightmare.gif" alt="White noise maze" caption='One of the many "nightmares" that would haunt me.' %}


### Maze generation

There is a bunch of algorithms to generate mazes out there, those based on
[graph theory](https://en.wikipedia.org/wiki/Graph_theory) being my absolute favorite.
Under this setup, the maze is modelled as a **graph** where nodes represent cells and edges represent walls,
and the task consists of finding a **spanning tree** for that model.

{% include figure image_path="assets/images/maze-graph.gif" alt="graph-theory-based maze generation" caption="Graph-theory based maze generation. Source: Wikipedia" %}

For this project I chose
[Wilson's algorithm](https://en.wikipedia.org/wiki/Maze_generation_algorithm#Wilson's_algorithm),
which generates very esthetically pleasant mazes in an unbiased way.
A big source of inspiration here were
[Mike Bostock's examples][mike-bostock].
Make sure to check them out to learn more about this algorithm.

{% include figure image_path="assets/images/wilsons-algorithm.png" alt="color-flooded maze" caption="Maze generated by Wilson's algorithm. The path can be traced through color gradient. Source: Mike Bostock" %}

### White noise

All Wilson's algorithm returns is an abstract graph of cells and walls.
In order to accommodate it in between the white-noise jungle, we need to render it,
and we better make it look like proper white noise.

Signal theory designates as **white noise** those random signals whose
frequency spectrum is flat. This means that they are composed of all frequencies
of the spectrum, in the same way that white light comprises all frequencies in
the visible spectrum (what we actually know as colors), hence the name. A consequence of this is that all signal values
are **independent** of each other. These values may follow a Gaussian distribution,
although this is not always the case.

### Autoregressive models

The grayscale levels of the cells and walls in the maze cannot be independent
random values, otherwise the structure would be lost. On the other hand,
if we naively assign black level to walls and white level to cells it
will stand out of the white noise and the visual impact will be equally lost.

The solution is to find a trade-off: the brightness of the maze pixels will be random,
but not entirely, so that the structure is preserved.
That is what autoregressive models are about.

**Autoregressive models** (AR models for short) are random processes obtained by weighting 
and averaging a random value
with some of the past values of the process. If the right parameters are chosen, this
has a smoothing effect: values are arbitrary, but they do not deviate wildly from
their neighboring values.

{% include figure image_path="assets/images/ar-finance.png" alt="Finance forecast" caption="Autoregressive models are at the core of financial forecasting. Source: Auquan" %}


The model I chose is a variation of an AR(1) process,
meaning that I only take into consideration a single past value.
It is pretty much as simple as it gets:

$$X_t=\alpha X_{t-1}+\left(1-\alpha\right)\epsilon_t$$

where $$\epsilon_t$$ represents an (independent) random value, $$X_{t-1}$$ and $$X_t$$
are the values of the AR model for positions $$t-1$$ and $$t$$ respectively,
and $$\alpha\in\left(0,1\right)$$ is a smooth factor: the closer $$\alpha$$ is
to 1, the more similar $$X_t$$ will be to $$X_{t-1}$$.

I apply this AR model along the path, in the same direction as the color gradient above.
The only difference I make between walls and cells is in $$\epsilon_t$$; in the case of
cells, I draw it from a Gaussian distribution centered around a bright value,
while for the walls the center of this distribution is dark.

But this is not all. Walls and cells have a certain width, so they cannot be
as granular as proper white noise. In order to overcome this, I apply fine-grained white
noise on top of the AR maze, making it indeed look noisier.

You may have noticed that the maze pattern does not stay still.
but it rather keeps shaking around timidly. Can you guess how I modeled this random shift?
You are right: Autoregression.

In that case I calculate two different AR processes: one for the column shift and
another one for the row shift. Then I apply this $$\alpha$$-smoothing along the corresponding
direction, so that two consecutive rows/columns are not abruptly shifted from each other.
On top of that, I also smooth it down along the time direction to avoid extremely fast changes.

### How white is your noise?

I have cheated

Gaussian white noise looks like this:

The background of the image looks like this:

They actually look different, and if you watch the spectra :

Background not flat -> not white

The explanation has to do with the maze. Here you have its spectrum:

Looks the same as the background up to some shiny edges which represent the maze structure.
The central plateau accounts for low frequencies or low-resolution details -> the thick walls and cells.
The floor represents fine-grain noise, high frequencies, high resolution.
(As a matter of fact, video coding standards wildly reduce such high-res features, that's the reason
they are more visible in the GIF than in the video above).

the background is fake "white noise" that mimicks the maze. I create it by generating coarse wn,
zooming in and adding fine-grain wn. That's why its spectrum looks similar as.
But not only spectra look similar: That's a comparison of a half-emerged maze against the actual background
vs. the same rendering against pure white noise:

If I used pure white noise for the background, the transition would look patchy instead of gradual.

Background and maze do have a similarity with this Gaussian white noise. Look at their histograms:

This is an example that the histogram of a stochastic signal does not say anything about its
spectrum, and vice-versa. This is a very common pitfall for undergraduate and graduate students
in Engineering, including myself. 

**Disclaimer:** The histogram of maze and background is not Gaussian strictly speaking, but a mixture of
two Gaussian curves (one for dark values -> walls and another for white values -> cells) that lie
so close together that appear as a single one in the histogram. However, if you look at the histograms
you see a notch between the Gaussian fit and the actual count bins, which accounts for this fact.


## Further references

- [Maze generation algorithms](https://en.wikipedia.org/wiki/Maze_generation_algorithm)
- [Mike Bostock's Blocks][mike-bostock]---
Cool animations showing how Wilson's algorithm generates mazes.
- [Time Series Analysis for Financial Data II — Auto-Regressive Models](https://medium.com/auquan/time-series-analysis-ii-auto-regressive-models-d0cb1a8a7c43) ---
Post from a miniseries on the topic by Auquan on medium.com.

[mike-bostock]: https://bl.ocks.org/mbostock/11357811
