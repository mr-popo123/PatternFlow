# Transforms: _**downscale_local_mean**_

<font color=#F08080>_**downscale_local_mean(image, factors, cval=0)**_</font>

## **Description**
This function is used to down_sample N-dimensional image by local averaging. It calculates the local mean of elelments n each block of size _factors_ in the input image.

The image is padded with _cval_ if it is not perfectly dividible by the integer factors.

This function is equvalent to <font color=#FF6347>_skimage.transform.downscale_local_mean_</font> function.

#### Parameters
> **image**: ndarray   
> &emsp; N-dimensional input image.   
> **factor**: array_like   
> &emsp; Array containing down-sampling integer factor along each axis.   
> **cval**: float, optional   
> &emsp; Constant padding value if image is not perfectly divisible by the integer factors.

#### Returns
> **image**: ndarray   
> &emsp; Down-sampled image with same number of dimensions as input image. 

## **Principles**
_Downscale_local_mean_ serves the purpose of down-sampling an n-dimensional image by integer factors using the local mean on the elements of each block of the size factors given as a parameter to the function.

The following picture displays how the _downscale_local_mean_ function works in the situations '_image_ is perfectly divided by _factors_' and '_image_ is not perfectly divded by _factors_ (with _cval_ being 0)'.

![](image/algorithm_principle.jpg)

#### Dependencies
* _**block_reduce(image, block_size, func=tf.reduce_sum, cval=0)**_  
A helper function for _downscale_local_mean_. Equivalent to _skimage.measure.block.block_reduce_. It is used to down_sample the given image by applying function to local blocks.
* _**view_as_blocks(arr_in, block_shape)**_   
A helper function for _block_reduce_. Equivalent to _skimage.measure.shape.view_as_blocks_. It is used to compute the block view of the input n-dimensional array using re-striding.
* _**as_strided(x, shape=None, strides=None, writeable=True)**_   
A helper function for _as_strided_. Equivalent to _numpy.lib.stride_tricks.as_strided_. It is used to create a view into the array with the given shape and strides.

## **Examples**
#### Example 1
```python
>>> a = np.arrage(15).reshape(3, 5)
>>> a
array([[  0,  1,  2,  3,  4],
       [  5,  6,  7,  8,  9],
       [ 10, 11, 12, 13, 14]])
>>> downscale_local_mean(a, (2,3))
array([[ 3.5, 4. ],
       [ 5.5, 4,5]])
```
#### Example 2
```python
import matplotlib.pyplot as plt

from skimage import data, color
from downscale_local_mean import downscale_local_mean

image = color.rgb2gray (data.coffee())
image_downscaled = downscale_local_mean(image, (4, 3))
fig, axes = plt.subplots(nrows=1, ncols=2)

ax = axes.ravel()

ax[0].imshow(image, cmap='gray')
ax[0].set_title("Original image")

ax[1].imshow(image_downscaled, cmap='gray')
ax[1].set_title("Downscaled image (no aliasing)")

ax[0].set_xlim(0, 600)
ax[0].set_ylim(400, 0)
plt.tight_layout()
plt.show()
```
The picture below is generated by the above code. The image on the left is the input _image_ with shape (400, 600). The image on the right is the output we get from the function _downscale_local_mean_ using _factors_=(4, 3). Its shape is (100, 200).

![](image/example.png)
