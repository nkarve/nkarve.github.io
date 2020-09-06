---
layout: post
title: Convolutions in 4D
excerpt_separator:  <!--more-->
---

I wanted to see a 4-D convolutional layer. I was tired of sifting through the Tensorflow source code on Github only to find layers upon layers (pun intended) of what seemed like circular references. After hours of arduous searching (okay, maybe not) I thought I'd hit the jackpot, but through the mist, all I saw was an arcane cuDNN call rearing its ugly head. :(

Of course, the Internet is absolutely littered with 1-D and 2-D convolution algorithms which no-one would in a serious setting (~~I'm looking at you, Medium~~) -- not least because using *seven* nested `for` loops leaves a bad taste in the mouth.

But enough chit-chat. Let's hop into what I'm gonna be showing you: Three algorithms --

 1. Convolutional forward pass
 2. Convolutional backward pass (transfer the gradient to the previous layer)
 3. Calculation of the convolutional filter gradient, so we can update it via an optimiser

## Convolutions

The layer is entirely specified by its filter and an optional bias -- which I will ignore for now. The filter is a tensor (a scalar is a 0-dimensional tensor, a vector is a 1-d tensor, a matrix is 2-d -- I don't think there are any special names for higher dimensional tensors, but generally when people say 'tensor', they're referring to >2-dimensional ones) -- a 4-tensor to be precise, having dimensions: `(height, width, input_channels, output_channels)`, for example, `(5, 5, 3, 4)`.

The input (I'll call it the 'image' for simplicity, though in general it may not be a coherent image, or even an image at all) is another 4-tensor, but it has different dimensions: `(batch_size, image_height, image_width, input_channels)`, like `(64, 28, 28, 3)`. You'll notice that `input_channels` is there in both of them - it could represent RGB channels in a colour image, for example.

Here's a 6 step process on how to grasp what this means exactly:

 1. Forget about the image's `batch_size` for now. In the end, you can just imagine applying the process to each image in the batch. So we have `64` of these `(28, 28, 3)` images
 2. Instead of one 4-tensor, imagine the filter as `output_channels` many 3-tensors (of course these are equivalent representations). In our example, this means that there are `4` such`(5, 5, 3)` tensors
 3. Now you see that we have two 3-tensors, whose last dimension matches. You can visualise this as 2 cuboids, whose length and breadth might be different, but whose depths are the same. Now we just do the simple element-wise multiplication that every ML aficionado loves like their first-born child.
 4. Normally, you slide around (say) a `(5, 5)` filter around a `(28, 28)` image, multiplying the elements at each step. Well, here it's the same thing, except with another dimension! 
	- First you put your little `(5, 5, 3)` cuboid at the top left of the `(28, 28, 3)` cuboid. It'll fit in snugly depth-wise.
	- Multiply each element of the filter with the corresponding element of the image subsection. You'll end up doing $5\times5\times3=75$ multiplications in all.
	- Sum up all the results that you got (all 75 numbers in this case), to get a single scalar value
	- Put this value as element `(0, 0)` in your result matrix.
	- Shift the filter cuboid one unit to the right. If it's already at the right edge, move it *down* one unit and slide it all the way to the left. Repeat the steps until you've slid it over every part of the image cuboid. 
	- In this case, you'll end up with a `(24, 24)` result matrix. In general, it'll be `(image_height - filter_height + 1, image_width - filter_width + 1)`
 5. If you got the previous step then it's all smooth from here. Notice how we ended up with a matrix at the end of the slidy thing for two cuboids. Now all we gotta do is factor in the multiple filters as well as the batch size. Let's start with the first: If we have `4` filters, we apply the previous process *for each filter*. Obviously, we'll end up with `4` of these `(24, 24)` result matrices (note that they will have different values since each filter has different values). We can just concatenate these along a new axis to form a `(24, 24, 4)` tensor (a bit like how you can concatenate row vectors to form a matrix)
6. Finally, the `batch_size`. We apply steps 4 and 5 *for each image* in the batch (64 of them in this example) and accordingly obtain `64` such `(24, 24, 4)` tensors. As we did in the previous step, we can stack these up on another axis (let's do the first axis this time) to get our **final** result: a newborn `(64, 24, 24, 4)` tensor. The general result is a tensor of shape `(batch_size, image_height - filter_height + 1, image_width - filter_width + 1, output_channels)`

That's it! No, it wasn't that hard, and *yes*, this is how it's done in Tensorflow and PyTorch too (although, from what I've seen, they delegate the work to super-fast NVIDIA GPU kernels). The observant reader might notice that I've left out a little detail: *strides*. If you're lazy, you can just skip this part without missing out on much.

Striding involves a bit of a change in step 4: instead of sliding our filter *one* unit to the right and *one* unit down, we slide it $S_w$ units to the right and $S_h$ units down - these values are specified while constructing the network. This changes the step 4 result matrix size to $(\frac{I_h - F_h}{S_h} + 1, \frac{I_w - F_w}{S_w} + 1)$, i.e. it's effectively scaled down by the stride size, so the final output has the shape $(B, \frac{I_h - F_h}{S_h} + 1, \frac{I_w - F_w}{S_w} + 1, D)$, where $D$ is the number of output channels. Note that there's no striding along the image channels axis, unless you're the sort of weirdo who enjoys doing that kind of thing.

For the mathsy-types among you who are super restless right now because I haven't been *entirely* rigorously (or haven't used enough $\LaTeX$): here's the formula that I derived for the forward pass (warning: cover your eyes if you have a weak stomach):

$$
y_{b, i, j, d} = \sum_{m=0}^{f_h - 1}\sum_{n=0}^{f_w - 1}\sum_{c=0}^{C} x_{b, \,S_hi+m, \,S_wj+n,\,c} \,f_{m,n,c,d}
$$ 

Behold, summation hell. I didn't say it would be pretty.

After all that, we just have *one* of the three algorithms I promised to share, and I haven't even shown you how to implement it! That's all right, as long as I've managed to build up a little intuition regarding the matter, you might even be able to do it yourself -- besides, the backpropagation voodoo stuff involves heavy code reuse of the forward pass that I just described.

You may be asking, "How can we *possibly* implement something that complicated without using the 7 `for` loops that you laughed at a minute ago?"
Well here's the amazing thing: my code contains a grand total of *0* `for` loops -- 
"What, even with strides?"
 -- *yep*, even with strides, but that's not all: it takes up just *6 lines in its entirety*. That's right. 6 lines. As you stutter in disbelief, I'll give you the final kicker:
*3 of those lines* are tuple unpackings! At this point, you may need a cool drink as you collapse in your seat. 
 
Here it is in all its glory:

# Code
```python
import numpy as np
from numpy.lib.stride_tricks import as_strided

def conv4d_forward(x, f, s):
   	B, H, W, C = x.shape
	Fh, Fw, C, D = f.shape
	Sh, Sw = s

	strided_shape = B, 1 + (H - Fh) // Sh, 1 + (W - Fw) // Sw, Fh, Fw, C  
	xs = as_strided(x, strided_shape, strides=(x.strides[0], Sh * x.strides[1], Sw * x.strides[2], *x.strides[1:4]))
	
	return np.einsum('wxyijk,ijkd->wxyd', xs, f)
```

Okay, okay, the 5th line may violate PEP's 80 character convention, but who's counting? (sorry mobile users). Let's break it down, line by line:

`B, H, W, C = x.shape` $\leftarrow$ That's just what  I called `(batch_size, image_height, image_width, input_channels)` previously, if you remember.

`Fh, Fw, C, D = f.shape` $\leftarrow$ Same spiel here, but with `(height, width, input_channels, output_channels)` for the filter.

`Sh, Sw = s` $\leftarrow$ This is left as an exercise for the reader

This is where the fun begins.
`strided_shape = B, 1  + (H - Fh) // Sh, 1  + (W - Fw) // Sw, Fh, Fw, C` looks familiar - it's kind of what the output shape is, but instead of `D`, we've got `Fh, Fw, C` at the tail end. This becomes significant in view of our next line:

```python
x =  as_strided(x, strided_shape, strides=(x.strides[0], Sh * x.strides[1],
	Sw * x.strides[2], *x.strides[1:4]))
```

What? What even is that? If you're an experienced NumPy dev, the name '`as_strided`' may send shivers down your spine as you recall painful memories of tangling up dimensions and strides. It may well be the most misunderstood, feared function in the entire library: so much so that even the library makers advise you to avoid it as far as possible. It is also superbly elegant and powerful - enough to kickstart in anyone a lifelong interest in low-level matrix operations. To understand what it does, you have to take a detour down to the bones of matrix implementations.

## Strides

How does a matrix (or indeed, a tensor) store its elements? To preserve *locality of reference* during matrix operations, all elements are stored sequentially in contiguous memory locations. The way NumPy converts an indexing operation on an array, like `a[i, j]`, is by calculating the `offset = a.strides[0] * i + a.strides[1] * j` from the location of the first element (note: I am assuming C-style element ordering). Don't confuse these strides with the ones in the `conv4d` algorithm. These 'intrinsic' array strides, `a.strides[n]`, tell us how many bytes we need to move across to get the next element along the `nth` axis. This is incredibly powerful. For example, transpositions are super fast because *they don't change the element ordering at all*, they just reverse the strides tuple; broadcasting sets the stride along the new axis to be `0`; even diagonals can be extracted via this method. [This SciPy lecture](http://scipy-lectures.org/advanced/advanced_numpy/#indexing-scheme-strides) is an excellent source on strides and indexing.

According to the NumPy documentation, "`as_strided` creates a view into the array given the exact strides and shape". So it is usually very cheap, manipulating the `strides` tuple rather than the actual data. I think showing rather than telling here is more effective, so:
```python
'''
This converts the image to a 6-dimensional tensor, where the two extra
dimensions represent strided, windowed 'snapshots' of each image (along the
H and W dimensions) for every batch and channel

E.g. if each image is Bx5x5xC, the filter is 3x3xCxD and the strides are
(1, 1), let's take:

x[0, :, :, 0] = [[ 1.  2.  3.  4.  5.]
		 [ 6.  7.  8.  9. 10.]
		 [11. 12. 13. 14. 15.]
		 [16. 17. 18. 19. 20.]
		 [21. 22. 23. 24. 25.]]

as our guinea pig. After the as_strided() call, this is converted to:

xs[0, :, :, :, :, 0] = 
[ 
  [[[ 1.  2.  3.]
    [ 6.  7.  8.]
    [11. 12. 13.]]
   
   [[ 2.  3.  4.]
    [ 7.  8.  9.]
    [12. 13. 14.]]

   [[ 3.  4.  5.]
    [ 8.  9. 10.]
    [13. 14. 15.]]]

  [[[ 6.  7.  8.]
    [11. 12. 13.]
    [16. 17. 18.]]
	
   [[ 7.  8.  9.]
    [12. 13. 14.]
    [17. 18. 19.]]
	
   [[ 8.  9. 10.]
    [13. 14. 15.]
    [18. 19. 20.]]]

  [[[11. 12. 13.]
    [16. 17. 18.]
    [21. 22. 23.]]

   [[12. 13. 14.]
    [17. 18. 19.]
    [22. 23. 24.]]

   [[13. 14. 15.]
    [18. 19. 20.]
    [23. 24. 25.]]]
]

i.e. mimicking a 3x3 filter sliding over the image. This is done for each
batch and channel, thus obtaining a 6-dimensional strided image.
'''
```
Yeah, I just copied and pasted my own docs. So the previous two lines were there to get the original image to a strided form, which is a lot easier to work with for the next step. You might notice that I'm throwing the word 'strided' around a lot, so even if you can't remember what each one means, just nod your head as if in deep thought - it'll impress any Javascript developers who happen to be looking over your shoulder. Onto the last line!

## Einsum

`return np.einsum('wxyijk,ijkd->wxyd', xs, f)`. Ah yes, `einsum`. Another one of those supposedly fiddly functions (not true) that can greatly increase your stature in the workplace, just by mentioning it in casual conversations (true - along with other cryptic terms like "kernel", "RAII" and "The Rust Programming Language"). I found [this](https://ajcr.net/Basic-guide-to-einsum/) to be a great intro. It's short for 'Einstein Summation', named, of course, after everyone's favourite physicist, Mr Summation. Here's a (very) quick primer:

 - See that arrow? On the right-hand side of it is the output; on the left side is a comma-separated list of inputs. By 'input' and 'output', I specifically mean their dimensions.
 - What's with the fancy letters? Each one labels an axis size - so `'ij'` means a matrix with shape `(i, j)`; `'ij,jk'` means two matrices with shapes `(i, j`) and `(j, k)` respectively.
 - If an index appears on the left-hand side but not the right, all axes labelled with that index get summed over (called 'contraction'). Here, take a look at these examples:
 
`C = np.einsum('ij,jk->ik', A, B)` is a shorthand for
$$
C_{ik} = \sum_j A_{ij} B_{jk}
$$
If you have an - aha! - moment here, you're right: that's just matrix multiplication! There doesn't necessarily have to be anything on the right side though: `C = np.einsum('ii->', A)` means
$$
C = \sum_i A_{ii},
$$
which is, of course, the trace.

If you think it's convoluted (see what I did there?) and frankly, a bit useless, you'll be surprised to know that a lot of computational quantum physicists and deep learning researchers think almost exclusively in `einsum`. In fact, once you use it a bit more, you'll start to realise how feeble `dot`, `*` and `for` loops are in front of the mighty `einsum`.

Now I can introduce one of my favourite techniques of solving tensor problems like these in general: dimensional analysis (similar to a technique of the same name in physics involving units rather than tensor dimensions). If you know what output shape you want, as well as the input shape(s), you can often work out the exact operation that you want to perform. Let's say you suddenly forget how matrix multiplication works (don't laugh, it's more common than you think). Pause for a sec, and just *think*:

|$\mathbf A$ |$\mathbf B$ |$\mathbf C$ |
|--|--|--|
|$ij$|$jk$|$ik$|

"Hold on just a moment now! There's an $i$ and a $k$ on both sides, but the $j$ is only on the left! Which means all I need to do is contract over $j$ - so $\mathbf{C = AB} = \sum_j\mathbf A_{ij} \mathbf B_{jk}$!"
Admittedly, this isn't the best example, because you have to remember the dimensions of the matrices. But I for one can recount several instances of where my mind freezes when I'm trying to decide whose rows and whose columns to multiply together. The best part is when you realise that you can just plug the dimensions into `einsum` and it does all the work for you! With that in mind, let's tackle the final line.

What are we trying to combine here?
|Name|Shape|
|--|--|
| `xs`|`strided_shape = B, 1  + (H - Fh) // Sh, 1  + (W - Fw) // Sw, Fh, Fw, C`|
|`f`|`Fh, Fw, C, D`|
|`output`|`B, 1  + (H - Fh) // Sh, 1  + (W - Fw) // Sw, D`|

We're on the home stretch now. Now it's just a matter of stuffing everything into `einsum`, making sure to label matching indices correctly. This is our template:
`return np.einsum('______,____->____')`

The first three dimensions of `xs` appear in `output`, so no contraction there. We'll call the axes `w`, `x` and `y`.
`return np.einsum('wxy___,____->wxy_')`

The last three dimensions need to be contracted with the first three dimensions of `f`. We'll call the contracted indices `i`, `j` and `k`.
`return np.einsum('wxyijk,ijk_->wxy_')`

Finally, the last dimension of `f` remains as it is. We'll call this one `d`.
`return np.einsum('wxyijk,ijkd->wxyd')`

So we've recovered the last line in great style. If you head back up to the docs example, you can see that each filter gets multiplied element-wise with each 3-d block in `xs` - exactly the behaviour we want. 

Seemed like a lot of explanation for a couple of lines of code, right? However, once you practice enough, it almost becomes second nature. Oops, forgot to do one thing. Let's check if all this is correct (notice how Tensorflow's one is called 'conv2d' - in hindsight that was probably a better name, since the actual convolution - read: sliding - is only happening over 2 dimensions):

```python
from tensorflow.nn import conv2d
tf.enable_eager_execution()

x = np.random.randn(64, 28, 28, 3)
f = np.random.randn(6, 6, 3, 2)
strides = 2, 2

y = conv4d_forward(x, f, strides)

tf_x, tf_f =  tf.Variable(x), tf.Variable(f)
tf_y = conv2d(tf_x, tf_f, strides, padding='VALID')

print(np.allclose(y, tf_y))
```
`> True`

The code for my entire neural network project (codename: Twist) will be up on Github shortly.
