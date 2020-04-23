---
title: "Sparse Complex Vectors"
excerpt_separator: "<!--more-->"
header:
    image: /assets/images/sparse-complex-vector.png
    teaser: /assets/images/sparse-complex-vector-th.png
gal_scv:
  - url: /assets/images/scv-0dB.png
    image_path: /assets/images/scv-0dB.png
    alt: "Sparse complex vector reconstruction. Normalized error around 0 dB"
  - url: /assets/images/scv-10dB.png
    image_path: /assets/images/scv-10dB.png
    alt: "Sparse complex vector reconstruction. Normalized error around -10 dB"
  - url: /assets/images/scv-35dB.png
    image_path: /assets/images/scv-35dB.png
    alt: "Sparse complex vector reconstruction. Normalized error around -35 dB"

gal_opcam:
  - url: /assets/images/cs_soccer.png
    image_path: /assets/images/cs_soccer.png
    alt: "Images of a football acquired with a one-pixel camera"
  - url: /assets/images/cs_books.png
    image_path: /assets/images/cs_books.png
    alt: "Image of books acquired with a one-pixel camera"
  - url: /assets/images/cs_soccer.png
    image_path: /assets/images/cs_cat.png
    alt: "Image of a sleeping cat acquired with a one-pixel camera"
---

Engineering problems are often too abstract to catch the eyes of laymen.
A right representation can, however, unveil the hidden
beauty of the underlying math.

<!--more-->

Take a look at the following images:

{% include gallery id="gal_scv" caption="Reconstructed user activity under different error levels (about 0, -10 and -35 dB respectively). Within each picture, the left column shows a portion of the original vector and the right column its reconstruction." %}

They are actually a by-product of a real project I participated in
at work. We were exploring some strategies to enable massive connectivity in 5G mobile networks,
which relied on reconstructing compressed information
about mobile users' activity. As I went through the implementation,
I realized I needed a way to visually assess how good I was doing with the reconstruction,
so by serendipity I ended up
creating these minimalist patterns you could hang in your living room.

## Massive connectivity and user activity

Let me explain the concept with simple words before diving into details.
The key point about massive mobile connectivity is to keep more users within your
network than you can technically serve. The reason why this works is because not
all users want to communicate at the same time.
Imagine a 5G mobile cell can provide resources for 100 users at the same time.
If the antenna in the cell knows that only 1 out of 10 users are actually active at any given time,
then it can promise connection to up to 1000 users as long as the activity rate stays at that level.

In order to keep track of their activity, every user receives a unique word
to transmit to the antenna whenever they want to connect. So for example when
Alice needs connection she will inform the antenna by saying the word Apple.
Bob will use Banana instead, Carol will go with Cherry, and so on.
If users shouted their connection words out loud all at once the antenna would not understand
anything but fruity gibberish,
but if only _some_ users do, it may manage to tell apples from pears.

This is what I mean by compressed information. The only thing the antenna receives
is a short and juicy fruit salad from which it has to guess which users are active and
which ones are not. Thus, every colored stripe in the pictures accounts for a single user's activity.
Intense colors indicate intense activity, i.e., "how loud" the antenna perceives
the corresponding connection word. On the other hand, White regions are groups of inactive users.
Every picture contains two columns, left for the original information and right
for the reconstruction, so you can actually assess how well the reconstruction process went.

## Phone waves, complex numbers and colors

You may think I chose such an eye-catching palette as a frivolous excess, just to have something
to write about. I cannot deny I lent wings to my creativity,
but this rainbow color mapping was also required by the way we encode user activity.

In order to communicate, mobile phones transmit electromagnetic waves.
These transmitted waves traverse space and time until reaching the receiver,
which affects some of their physical properties. More specifically,
their **amplitude** decreases (they become _more quiet_) and their **phase** changes
(they arrive at a _delayed_ point in time). What we encode as user activity is a compact
representation of amplitude and phase using [complex numbers].

<i class="fas fa-graduation-cap"></i>
If you studied **complex numbers** in high school you may have wondered
why are they even useful, since they even include a so-called _imaginary_ part.
I hope you are glad to find out there are engineers out there squeezing
their brains with imaginary figures to solve real problems.
{: .notice}

### Complex numbers

Complex numbers are composed of two values known as **real** and **imaginary** part.
This means that, unlike real numbers,
we cannot place them on a line.
We need a two-coordinate system instead, the so-called complex plane:

{% include figure image_path="assets/images/phasor.svg" alt="Number in the complex plane" caption="Number in the complex plane. Adapted from Wikipedia" %}

Here the amplitude, marked as $$r$$, is represented as the vector length,
while the phase  $$\varphi$$ is just the angle between the horizontal axis and the vector.

Now, if we have a bunch of real numbers we want to assign colors to,
the task is relatively easy: Check the minimum and maximum numbers to map, assign black and white color
to them respectively and give any number in between an intermediate level of gray depending on its distance
to the extremes. However, we cannot order complex numbers like that. A grayscale is not enough, we need a
bidimensional representation of color.

Well, a wheel is bidimensional,
so why not use the color wheel?

### Color wheel

{% include figure image_path="assets/images/colorwheel.png" alt="Color wheel" caption="Color wheel. Source: ariya.io" %}

I could write a whole post about color theory, but let us restrict ourselves to the basics.
The colors we perceive can be described by three quantities. Which three? Well,
there are a handful of different possibilities: RGB, CMY, YCbCr... but I have actually
chosen **hue**, **saturation** and **value** (HSV).

The color wheel above displays combinations of hue and saturation for a value of 100%.
Colors at the edge are fully saturated, while the center exhibits a completely
unsaturated white. The hue is given by the angular sector or _piece of pie_ we slice out of the
wheel.

As you see, talking in terms of angle and amplitude comes in naturally.
We can thus
extend our color wheel as a picnic blanket on the complex plane until
we cover all the complex values we want to represent.

{% include figure image_path="assets/images/complexcolor.svg" alt="Color mapping on the complex plane" %}

There we go, we have found a way to assign colors to complex numbers.
Since numbers that lie close to each other in the complex plane also have similar
colors as per the color wheel, we can verify very quickly how faithful the reconstruction is
by placing it side by side with the original information.

This also explains why inactive users are given a white color. Their user activity equals zero,
as they remain quiet without issuing their connection word.
Now pay attention which color the zero value is assigned by our color mapping.
_Blanco y en botella_[^1].


**Note** So far, this color mapping assumes that the color value is fixed.
In my experiments, I set the value to 95% but I raised it to 100% for those values
equal (and not only close) to zero. As a result, all non-white colors are a little
bit darker than in the displayed color wheel and
the reconstructed values who should be zero but are _only_ close to zero
appear as light gray stripes.
{: .notice--info }

## Reconstructing compressed information

Retrieving a large amount of data from a reduced combination of their elements is a challenging problem
in the field of **signal and information theory**.
In fact, classical math will tell you that you will fail to solve the problem
if you try to write the equations down.

In our project, we used a technique called **compressive sensing** (CS)
that has been around since the late 2000s. Its applications extend far beyond
mobile communications and they are usually related to the acquisition of a signal from
a reduced set of measurements. To have a feeling of how powerful this technique can be,
I really encourage you to have a look at this magnificent
[Wired article](https://www.wired.com/2010/02/ff_algorithm/) of 2010,
which explains how the algorithm works and how it 
was applied to magnetic resonance imaging to
save a 2-year-old toddler's life back in 2009.

Compressive sensing is an exciting research area, with hundreds of top-level scientists devoted to
push its boundaries day by day. In our implementation, we took as a reference a 2017 paper by
Mark Borgerding, Philip Schniter and Sundeep Rangan which enhances CS algorithms by including
ideas from neural networks and artificial intelligence.

## Further references

TODO

[^1]: Spanish for "White and bottled". This is actually a saying to point out something obvious, like the fact that if someone is talking about a white and bottled substance they probably refer to milk.

[complex numbers]: https://en.wikipedia.org/wiki/Complex_number