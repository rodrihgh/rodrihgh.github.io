---
title: "Sparse Complex Vectors"
excerpt_separator: "<!--more-->"
header:
    overlay_color: "#888888"
    overlay_filter: "0.25"
    overlay_image: /assets/images/sparse-complex-vector.png
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

Not only are sparse complex vectors useful for real-world problems, but they also look amazing.
<!--more-->

**Warning** <i class="fas fa-exclamation-triangle"></i> Project under construction.
{: .notice--warning}

{% include gallery caption="Reconstructed sparse complex vectors under different error levels." %}

The pictures I am showing you here are actually a by-product of a real project I participated in
at work. We were exploring some strategies to enable massive connectivity in 5G mobile networks,
which relied on the reconstruction of **sparse complex vectors**. As I went through the implementation,
I realized I needed a way to assess visually how good I was doing, so by serendipity I ended up
creating these minimalist patterns you could hang in your living room.

## What are sparse complex vectors?

Ok, so in order to find out what these colourful guys are, let us strip the full term down to
its minimal parts. Sparse complex vectors are...

- **Vectors**, that is, a list of numbers. What kind of numbers? Well, they are...
- **Complex** numbers. You may remember complex numbers from high school math. These are those
which include the square root of a negative number (the so called **imaginary** part) along with a "regular"
number like the ones we are used to (the **real** part).  Yes, even though you thought learning about
numbers that _do not exist_ was pointless, there are crazy engineers out there squeezing their brains
with imaginary figures to solve actual problems. But that is not all, as these complex vectors are...
- **Sparse**. This means that a vast majority of the numbers in the vectors equal **zero**.

## Why do they look like that?

As I have said, this is some coding I used to visualize the vectors. I just wanted some rules to
map complex numbers to colors. Let us talk in more detail about complex numbers and how to represent them.

Once again, complex numbers comprise of a real part and an imaginary part.
The real part can be any prosaic real number like 1, -3.8, 1846495.576945, $$\pi$$ or even 0.
The imaginary part, on the contrary, is the square root of a negative number. In order to write this
esoteric part, we notate $$i=\sqrt(-1)$$ and refer any other imaginary number to $$i$$, i.e.:
$$\sqrt(-4)=2i$$, $$\sqrt(-7)=\sqrt(7)i$$.

## How does this relate to mobile communications?

## What approach did you actually use?