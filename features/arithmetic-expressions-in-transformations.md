# Arithmetic expressions in transformations

ImageKit allows use of arithmetic expressions in certain dimension and position-related parameters, making media transformations more flexible and dynamic. They allow for more flexible and complex transformations without the need to hard code specific values in scenarios where the exact dimensions or positions may not be known beforehand or when they need to be adjusted based on other parameters.

Example use cases

* Dynamic resizing:
  You can use arithmetic expressions to set the dimensions of an image relative to its original dimensions or other parameters. For instance, making the width of an image half of its original size using `w-iw_div_2`.

* Positioning overlays:
  Arithmetic expressions can be used to calculate the position of overlays or watermarks based on the dimensions of the original image. For instance, placing a watermark at a position that is 10% from the top and right edges of the image using transformation `lx-bw_mul_0.9,ly-bh_mul_0.1`.

* Adjusting border in image:
  Arithmetic expressions can be used to calculate these values based on other parameters like the image's dimensions or aspect ratio using `b-ch_mod_5_yellow`.

You can create arithmetic expressions by using arithmetic operators with expression variables or positive numbers. A simple expression follows the `{value}_{operator}_{value}` syntax, where the value can be expression variables, positive integers, or positive decimal numbers. You can also directly assign expression variables like `ih`, or `bw` to supported parameters. For example, you can use expressions like `bh_div_2` or `ch_mul_0.25`. Furthermore, you can combine multiple operators and values to create complex expressions, such as `ih_mul_0.8_add_iw_mul_0.4`.

### Supported expression variables
| Property | Description |
| -------- | ----------- |
| <p>ih</p> | Initial height of the asset. Inside a layer transformation, it indicates the initial height of the asset being overlaid. |
| <p>iw</p> | Initial width of the asset. Inside a layer transformation, it indicates the initial width of the asset being overlaid. |
| <p>iar</p> | Initial aspect ratio of the asset. Inside a layer transformation, it indicates the initial aspect ratio of the asset being overlaid. |
| <p>ch</p> | It represents the height resulting from the last applied transformation in a chain of transformations. Similarly, inside a layer transformation, it indicates the current height of the asset being overlaid. |
| <p>cw</p> | It represents the width resulting from the last applied transformation in a chain of transformations. Similarly, inside a layer transformation, it indicates the current width of the asset being overlaid. |
| <p>car</p> | It represents the aspect ratio resulting from the last applied transformation in a chain of transformations. Similarly, inside a layer transformation, it indicates the current aspect ratio of the asset being overlaid. |
| <p>bh</p> | It represents the current height of the base asset upon which the layer will be overlaid. It can only be used within a layer transformation.|
| <p>bw</p> | It represents the current width of the base asset upon which the layer will be overlaid. It can only be used within a layer transformation. |
| <p>bar</p> | It represents the current aspect ratio of the base asset upon which the layer will be overlaid. It can only be used within a layer transformation. |

### Supported operators
| Operator name | Operation |
| -------- | ----------- |
| add | add |
| sub | sub |
| mul | mul |
| div | div |
| mod | mod (remainder)|
| pow | pow |

### Parameter supporting arithmetic expressions
| Parameter | Examples | Remarks |
| - | - | - |
| <p>h</p> |<ul> <li>h-ih_add_10</li> <li>h-ch_mul_0.8_add_bh_mul_0.6</li> </ul> |  |
| <p>w</p> | <ul> <li>w-ih_add_10</li> <li>w-ch_mul_0.8_add_bh_mul_0.6</li> </ul>|  |
| <p>ar</p> |<ul> <li>ar-car_mul_0.5</li> <li>ar-bar</li> </ul> | The calculated value of the expression will represent the `width/height` ratio. |
| <p>x, y</p> | <ul> <li>x-iw_sub_cw</li> <li>y-ih_sub_ch</li> </ul>|  |
| <p>xc, yc</p> | <ul> <li>xc-cw_mul_0.25</li> <li>yc-ch_mul_0.25</li> </ul>|  |
| <p>b</p> | <ul> <li>b-ih_mul_0.05_red</li> <li>b-cw_mod_5_AAAA54</li> </ul>| The border follows the `{expression}_{color}` syntax, where the calculated expression value is used as the border width. |
| <p>lx, ly</p> | <ul> <li>lx-bw_div_2</li> <li>ly-bh_div_2</li> <li>lx-cw_div_2_add_iw_div_2</li>  <li>ly-ch_div_2_add_cw_div_2</li> </ul>| These are layer positional parameters that can be placed in any order within a chain of transformations inside a layer transformation but are always executed last. Hence `ch`, `cw`, and `car` expression variables when used along with `lx` or `ly` parameter always represent the final height, width, and aspect ratio respectively of the asset being overlaid. |
| <p>pa</p> |<ul> <li>pa-bh_mul_0.05</li> <li>pa-bw_mod_5</li> </ul> | The calculated expression value will be applied as uniform padding along all four edges of the text.  |
| <p>fs</p> |<ul> <li>fs-bh_mul_0.1</li> <li>fs-bw_div_50</li> </ul> |  |

### Calculation Sequence for multiple operators
<ol> <li>Multiplication and Division: Multiplication and division are evaluated first from left to right.</li>
<li>Addition and Subtraction: Addition and subtraction are evaluated next from left to right.</li>
<li>Modulus: Modulus is evaluated then from left to right.</li>
<li>Power: Power is evaluated last from left to right.</li></ol>

{% hint style="info" %}
* If you're using expressions inside a layer, then the layer should be a separate chain transformation.

* Only the `bw`, `bh`, and `bar` expression variables are allowed inside text layers and solid image layers.
{% endhint %}

### Examples

#### Resizing

To resize the height to half of the original, the width to one-fourth of the original, and then add a yellow color border that's 5% of the current width, you can use the following URL.

URL - [https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-iw_div_4,h-ih_div_2:b-cw_mul_0.05_yellow](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-iw_div_4,h-ih_div_2:b-cw_mul_0.05_yellow)

![](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-iw_div_4,h-ih_div_2:b-cw_mul_0.05_yellow)

To crop the image with half height and width while extracting from a position that is 20% from top and left edges, you can use the following URL.

URL - [https://ik.imagekit.io/demo/medium_cafe_B1iTdD0C.jpg?tr=w-iw_div_2,h-ih_div_2,cm-extract,x-iw_mul_0.2,y-ih_mul_0.2](https://ik.imagekit.io/demo/medium_cafe_B1iTdD0C.jpg?tr=w-iw_div_2,h-ih_div_2,cm-extract,x-iw_mul_0.2,y-ih_mul_0.2)

![](https://ik.imagekit.io/demo/medium_cafe_B1iTdD0C.jpg?tr=w-iw_div_2,h-ih_div_2,cm-extract,x-iw_mul_0.2,y-ih_mul_0.2)

#### Image overlay

You can also resize and position the image layer relative to the base asset. For example, to first resize the image being overlaid to half of the base image dimensions and then place it one-eight from the top and left edges, you can use the following URL.

URL - [https://ik.imagekit.io/demo/medium_cafe_B1iTdD0C.jpg?tr=l-image,i-logo-white_SJwqB4Nfe.png,cm-pad_resize,bg-yellow,w-bw_div_2,h-bh_div_2,lx-bw_div_8,ly-bh_div_8,l-end](https://ik.imagekit.io/demo/medium_cafe_B1iTdD0C.jpg?tr=l-image,i-logo-white_SJwqB4Nfe.png,cm-pad_resize,bg-yellow,w-bw_div_2,h-bh_div_2,lx-bw_div_8,ly-bh_div_8,l-end)

![](https://ik.imagekit.io/demo/medium_cafe_B1iTdD0C.jpg?tr=l-image,i-logo-white_SJwqB4Nfe.png,cm-pad_resize,bg-yellow,w-bw_div_2,h-bh_div_2,lx-bw_div_8,ly-bh_div_8,l-end)

Expressions can also conveniently be used to resize and position nested layers in relation to their outer layers. In this example, we overlay the resized `women-dress.jpeg` with a red border, positioning it 10% from the top and left edges of `medium_cafe_B1iTdD0C.jpg`, and then add the `imagekit.io` logo, positioning it at 20% from the top and left edges of `women-dress.jpeg`.

URL - [https://ik.imagekit.io/demo/tr:l-image,i-women-dress.jpeg,w-bw_div_3,b-iw_mul_0.02_red,lx-bw_mul_0.1,ly-bh_mul_0.1:l-image,i-logo-white_SJwqB4Nfe.png,w-bw_div_2,lx-bw_mul_0.1,ly-bh_mul_0.1,l-end,l-end/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:l-image,i-women-dress.jpeg,w-bw_div_3,b-iw_mul_0.02_red,lx-bw_mul_0.1,ly-bh_mul_0.1:l-image,i-logo-white_SJwqB4Nfe.png,w-bw_div_2,lx-bw_mul_0.1,ly-bh_mul_0.1,l-end,l-end/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:l-image,i-women-dress.jpeg,w-bw_div_3,b-iw_mul_0.02_red,lx-bw_mul_0.1,ly-bh_mul_0.1:l-image,i-logo-white_SJwqB4Nfe.png,w-bw_div_2,lx-bw_mul_0.1,ly-bh_mul_0.1,l-end,l-end/medium_cafe_B1iTdD0C.jpg)

#### Text overlay

To control the text layer's width and font size with respect to the base layer, you can use the following URL.

URL - [https://ik.imagekit.io/demo/medium_cafe_B1iTdD0C.jpg?tr=l-text,i-overlay%20made%20easy,fs-bh_div_20,w-bh_div_2,l-end](https://ik.imagekit.io/demo/medium_cafe_B1iTdD0C.jpg?tr=l-text,i-overlay%20made%20easy,fs-bh_div_20,w-bh_div_2,l-end)

![](https://ik.imagekit.io/demo/medium_cafe_B1iTdD0C.jpg?tr=l-text,i-overlay%20made%20easy,fs-bh_div_20,w-bh_div_2,l-end)

To add padding to a text layer with a background color, you can use the following URL.

URL - [https://ik.imagekit.io/demo/medium_cafe_B1iTdD0C.jpg?tr=l-text,i-overlay%20made%20easy,bg-yellow,pa-bw_mul_0.01,l-end](https://ik.imagekit.io/demo/medium_cafe_B1iTdD0C.jpg?tr=l-text,i-overlay%20made%20easy,bg-yellow,pa-bw_mul_0.01,l-end)

![](https://ik.imagekit.io/demo/medium_cafe_B1iTdD0C.jpg?tr=l-text,i-overlay%20made%20easy,bg-yellow,pa-bw_mul_0.01,l-end)

#### Video overlay

Expressions can also be used inside video layers for resizing and positioning in parameters that allow arithmetic expressions.