---
title: "Sparse Complex Vectors"
excerpt: Not only are sparse complex vectors useful for real-world problems, but they also look amazing.
layout: single
author_profile: false
header:
    image: /assets/images/sparse-complex-vector.png
    teaser: /assets/images/sparse-complex-vector-th.png
gallery:
  - url: /assets/images/scv-0dB.png
    image_path: /assets/images/scv-0dB.png
    alt: "Sparse complex vector reconstruction. Normalized error around 0 dB"
  - url: /assets/images/scv-10dB.png
    image_path: /assets/images/scv-10dB.png
    alt: "Sparse complex vector reconstruction. Normalized error around -10 dB"
  - url: /assets/images/scv-35dB.png
    image_path: /assets/images/scv-35dB.png
    alt: "Sparse complex vector reconstruction. Normalized error around -35 dB"
---

{% include gallery caption="Reconstructed sparse complex vectors under different error levels. Within each picture, the left column shows a portion of the original vector and the right column its reconstruction." %}

The pictures I am showing you here are actually a by-product of a real project I participated in
at work. We were exploring some strategies to enable massive connectivity in 5G mobile networks,
which relied on the reconstruction of **sparse complex vectors**. As I went through the implementation,
I realized I needed a way to assess visually how good I was doing with the reconstruction,
so by serendipity I ended up
creating these minimalist patterns you could hang in your living room.

## What are sparse complex vectors?

Ok, so in order to find out what these colourful guys are, let us strip the full term down to
its minimal parts. Sparse complex vectors are...

- **Vectors**, that is, a list of numbers. What kind of numbers? Well, they are...
- **Complex** numbers. You may remember complex numbers from high school math. These are those
which include the square root of a negative number (the so called **imaginary** part) along with a "regular"
number like the ones we are used to (the **real** part).  Yes, even though you thought learning about
numbers that _do not exist_ was pointless, there are crazy engineers out there squeezing their brains
with imaginary figures to solve actual problems. But that is not all, as these complex vectors are also...
- **Sparse**. This means that a vast majority of the numbers in the vectors equal **zero**.

## Why do they look like that?

As I have said, this is some coding I utilized to visualize the vectors. Here I just wanted some rules to
map complex numbers to colors. Let us talk now about complex numbers and why this color mapping is more involved than
for real numbers.

### Complex numbers

Once again, complex numbers comprise of a real part and an imaginary part.
The real part can be any prosaic real number like 1, -3.8, 1846495.576945, $$\pi$$ or even 0.
The imaginary part, on the contrary, is the square root of a negative number. In order to write this
esoteric part, we notate $$i=\sqrt(-1)$$ and refer any other imaginary number to $$i$$, i.e.:
$$\sqrt(-4)=2i$$, $$\sqrt(-7)=\sqrt(7)i$$. The full complex number is only the sum of both terms, i.e.:

$$z=x+y\cdot i$$

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
bidimensional representation of color. Well, a wheel is bidimensional,
so why not use the color wheel?

### Color wheel

{% include figure image_path="assets/images/colorwheel.png" alt="Color wheel" caption="Color wheel. Source: ariya.io" %}

I could write a whole post about color theory, but let us restrict ourselves to the basics.
The colors we perceive can be described by three quantities. Which three? Well,
there are a handful of different possibilities: RGB, CMY, YCbCr, HSV... I have actually
chosen **hue**, **saturation** and **value** (HSV).

The color wheel above displays combinations of hue and saturation for a full value.
Colors at the edge are fully saturated, while the center exhibits a completely
unsaturated white. The hue is given by the section or _piece of pie_ we slice out of the
wheel.

As you see, talking in terms of angle and magnitude comes naturally. Hence, we can
extend our color wheel as a blanket on the complex plane until all the complex values
we want to depict get covered by it.

{% include figure image_path="assets/images/complexcolor.svg" alt="Color mapping on the complex plane" %}

Do you recall that our vectors are not only complex, but also sparse?
And how this means that most of the vector elements are zero?
Now pay attention which color the value zero is assigned.
There you have the reason
why there are so many white spaces in the images;
that is where all the zero-valued elements are.

**Note** So far, this color mapping assumes that the color value is fixed.
In my experiments, I set the value to 95% but I raised it to 100% for those values
equal (and not only close) to zero. As a result, all non-white colors are a little
bit darker than in the displayed color wheel and
the reconstructed values who should be zero but are _only_ close to zero
appear as light gray spots.
{: .notice--info }

## How does this relate to mobile communications?

## What approach did you actually use?