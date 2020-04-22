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
However, the right representation can often unveil the hidden beauty of the math inside.

<!--more-->

Take a look at the following images:

{% include gallery id="gal_scv" caption="Reconstructed sparse complex vectors under different error levels (about 0, -10 and -35 dB respectively). Within each picture, the left column shows a portion of the original vector and the right column its reconstruction." %}

They are actually a by-product of a real project I participated in
at work. We were exploring some strategies to enable massive connectivity in 5G mobile networks,
which relied on the reconstruction of compressed and corrupted information
in the form of **sparse complex vectors**. As I went through the implementation,
I realized I needed a way to visually assess how good I was doing with the reconstruction,
so by serendipity I ended up
creating these minimalist patterns you could hang in your living room.

## What are sparse complex vectors?

Ok, so in order to find out what these colourful guys are, let us strip the full term down to
its minimal parts. Sparse complex vectors are...

- **Vectors**, that is, a list of numbers. What kind of numbers? Well, they are...
- **Complex** numbers. You may remember complex numbers from high school math. These are those
which include the square root of a negative number (the so called **imaginary** part) along with a
"regular" number like the ones we are used to (the **real** part).  Yes, even though you thought learning about
numbers that _do not exist_ was pointless, there are crazy engineers out there squeezing their brains
with imaginary figures to solve real problems. But that is not all, as these complex vectors are also...
- **Sparse**. This means that a vast majority of the vector elements equal **zero**.

## Why do they look like that?

As I have said, this is just some coding I utilized to visualize the original data
(left column) and its reconstruction (right column) after compressing it down
to a noisy version with a noticeably smaller number of elements.
Here I just wanted some rules to
map complex numbers to colors. Let us talk now about complex numbers and why this color
mapping is more involved than for real numbers.

### Complex numbers

Once again, complex numbers comprise of a real part and an imaginary part.
The real part can be any prosaic real number like $$1$$, $$-3.8$$,
$$1846495.576945$$, $$\pi$$ or even $$0$$.
The imaginary part, on the contrary, is the square root of a negative number. In order to write this
esoteric part, we notate $$i=\sqrt{-1}$$ and refer any other imaginary number to $$i$$, i.e.:
$$\sqrt{-4}=2i$$, $$\sqrt{-7}=\sqrt{7}i$$. The full complex number is only the sum of both terms, i.e.:

$$z=x+yi$$

TODO: Link complex numbers with waves

Where $$x$$ and $$y$$ are real numbers. You may know that real numbers
can be easily represented on the real line, but we need something else for complex numbers,
as we have two distinct parts. Here is where the complex plane comes to aid: draw a horizontal line for
the real part, a vertical line for the imaginary part and plot the complex number as a two-dimensional
vector. Yes, complex numbers can also be regarded as vectors, which means that complex vectors are technically
vectors of vectors.

{% include figure image_path="assets/images/phasor.svg" alt="Number in the complex plane" caption="Number in the complex plane. Adapted from Wikipedia" %}

The vector representation on the complex plane also reveals another way to describe complex numbers
through its magnitude $$r$$ and phase  $$\varphi$$. The magnitude indicates the distance from the vector to
the origin, while the phase is just the angle between the horizontal axis and the vector.

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

As you see, talking in terms of angle and magnitude comes in naturally. Hence, we can
extend our color wheel as a picnic blanket on the complex plane until
we cover with it all the complex values we want to depict.

{% include figure image_path="assets/images/complexcolor.svg" alt="Color mapping on the complex plane" %}

There we go, we have found a way to depict our vectors.
Every colored stripe you see above accounts for a complex number.
Since numbers that lie close to each other in the complex plane also have similar
colors as per the color wheel, we can verify very quickly how faithful the reconstruction is
by placing it side by side with the original vector.

Besides the colorful bits, you may wonder what the white spaces represent.
Remember what sparse means? Most of the vector elements are zero.
Now pay attention which color the zero value is assigned by our color mapping.
_Blanco y en botella_[^1].


**Note** So far, this color mapping assumes that the color value is fixed.
In my experiments, I set the value to 95% but I raised it to 100% for those values
equal (and not only close) to zero. As a result, all non-white colors are a little
bit darker than in the displayed color wheel and
the reconstructed values who should be zero but are _only_ close to zero
appear as light gray stripes.
{: .notice--info }

## What is this useful for?

You surely wonder why we would like to compress a bunch of numbers, corrupt them by adding noise
and then try to recover the original information.

### Compressed sensing

This is a well studied situation in mathematics and signal processing theory known as
**linear inverse problems**.
We would like to retrieve some information expressed as a list of, say, 1000 numbers,
but we do not have direct access to it. Luckily, we have a combination of these elements
in a reduced set of 400 numbers, for instance, and we know how the data was
(linearly) combined. Can we go back to our original 1000 numbers from this?

Classical maths do not shed too much optimism, unfortunately. In the end, we know the
combination process and the resulting 400 values,
which would allow us to write 400 equations. This is far from being enough, as we
theoretically need 1000 independent equations to solve the system and recover the 1000
original numbers. Furthermore, we cannot entirely rely on our observed data,
since some noise may have sneaked in during the process, adding more uncertainty to the problem.

In order to recover the original data, we need to leverage what we already know about it.
We are positive that the vectors are **sparse**, so that most elements are zero. We may know
that only around 100 elements will be non-zero. Does this mean that we only need 100 equations
and we can already solve the problem? Not yet, but we are getting close.
We know (roughly) _how many_ elements are non-zero, but we do not know _where_
they are located. Maybe these are only the first 100 elements, or the ones at the end;
maybe they are scattered randomly throughout the entire vector length. We still need to recover
1000 values, whether they are zero or not.

Under the assumption of **sparsity** we can actually reconstruct our original data from
reduced observations.
This is also known as **compressed sensing**: we have _sensed_ some data, not in all its glory
but in a _compressed_ form.

At a conceptual level, the algorithms that allow us to do so contain the following steps:

1. Get the compressed information.
2. Make an educated guess about the original data that has produced your observations.
3. Make sure that your guess fulfills your assumptions about the data (i.e. sparsity).
4. You know how the combination process works, so compress your guess accordingly.
5. Compare your compressed guess with the compressed observations.
6. Are they close enough? If yes, you are done. No problems if they are not,
You just use the discrepancy to refine your guess. Go back to step 2. and keep going
iteratively until the result is satisfactory.

### Applications

Enough technical details, where is this applied in practice?

As you may have already guessed, this can enable an extremely efficient acquisition of signals.
One paradigmatic example are **one-pixel cameras**. Just as you heard it,
some engineers at Bell Labs managed to acquire good-resolution pictures using a single-pixel camera.

{% include gallery id="gal_opcam" caption="Different images acquired with a one-pixel camera. Source: Huang et al., 2013" %}

The major breakthrough compressed sensing has accomplished appears in the health sector,
more specifically in magnetic resonance imaging. I could try and convince you how
significant this is, but I will rather link this magnificent
[Wired article](https://www.wired.com/2010/02/ff_algorithm/) of 2010
which explains how this algorithm saved a 2-year-old boy's life in 2009.

TODO explain application in 5G.

## What approach did you actually use?

TODO mention LAMP

## Further references

TODO

[^1]: Spanish for "White and bottled". This is actually a saying to point out something obvious, like the fact that if someone is talking about a white and bottled substance they probably refer to milk.