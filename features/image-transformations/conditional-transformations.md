---
description: >-
  You can apply transformations conditionally based on certain properties of the input asset
---

# Conditional transformations

Transformations can be applied conditionally i.e. only if certain properties of the input asset satisfy a given condition. To acccomplish this, you can create `if-else` - like constructs within an asset URL.

The syntax to build and specify a conditional transformation block is `if-{condition},{transformations_on_true},if-else,{transformations_on_false},if-end`.

For example, you can resize all landscape images to a certain width, say 200, while resizing portrait images to width 300. The value of the *ar* (aspect ratio) property of an image helps determine whether it has a landscape (> 1) or portrait (< 1) orientation.

{% tabs %}
{% tab title="Conditional transformation" %}
```markup
                                    1       2             3                   
                               ┌────────┐ ┌───┐         ┌───┐
https://ik.imagekit.io/demo/tr:if-ar_gt_1,w-200,if-else,w-300,if-end/default-image.jpg
```

1. **condition**: A condition based on a property of the image that evaluates to either true or false. It has the format `if-{property}_{operator}_{value}`. [Read more](/features/image-transformations/conditional-transformations#how-to-specify-the-condition-part)
2. **transformations_on_true**: Transformations that are applied only if the preceeding condition evaluates to true. This can be a [chain](/features/image-transformations/chained-transformations.md) of transformations as well.
2. **transformations_on_false** (optional): Transformations that are applied only if the preceeding condition evaluates to false. This can be a [chain](/features/image-transformations/chained-transformations.md) of transformations as well. This part along with the preceeding `if-else` are optional and can be skipped.
{% endtab %}
{% endtabs %}

## How to specify the condition part
The *condition* part in the above syntax can be built using a predefined set of image properties, comparison operators, and logical operators. A singular condition follows the `{property}_{operator}_{value}` syntax. For example, `if-h_gt_100` evaluates to true if the height of the image is greater than 100.

You can combine multiple conditions using the `and` and `or` logical operators. For example, `if-h_gte_100_and_h_lte_400_or_ar_lt_4-3` evaluates to true only if the image height is between 100 and 400, or the aspect ratio is less than 4:3.

{% hint style="info" %}
`and` operator has precedence over `or` operator during condition evaluation
{% endhint %}

### Supported properties
| Property | Allowed values | Description |
| - | - | - |
| <p>h</p> | <ul> <li>Numbers</li> </ul> | Height of the image at the current step in a chain of transforms. eg. 100, 200.55 |
| <p>w</p> | <ul> <li>Numbers</li> </ul> | Width of the image at the current step in a chain of transforms. eg. 100, 200.55 |
| <p>ar</p> | <ul> <li>Numbers</li> <li>Fraction - an aspect ratio of 4:3 can be specified as 4-3</li> </ul> | Aspect ratio of the image at the current step in a chain of transforms. eg. 100, 200.55, 4-3|
| <p>ih</p> | <ul> <li>Numbers</li> </ul> | Height of the original, untransformed image. eg. 100, 200.55 |
| <p>iw</p> | <ul> <li>Numbers</li> </ul> | Width of the original, untransformed image. eg. 100, 200.55 |
| <p>iar</p> | <ul> <li>Numbers</li> <li>Fraction - an aspect ratio of 4:3 can be specified as 4-3</li> </ul> | Aspect ratio of the original, untransformed image. eg. 100, 200.55, 4-3 |

### Supported operators
| Operator | Interpretation |
| - | - |
| eq | `h_eq_100` evaluates to true if the image height is equal to 100 |
| ne | `h_ne_100` evaluates to true if the image height is not equal to 100 |
| gt | `h_gt_100` evaluates to true if the image height is greater than 100 |
| gte | `h_gte_100` evaluates to true if the image height is greater than or equal to 100 |
| lt | `h_lt_100` evaluates to true if the image height is less than 100 |
| lte | `h_lte_100` evaluates to true if the image height is less than or equal to 100 |

## How to specify transformations
You can specify a set of any supported transformations, or a [chain](/features/image-transformations/chained-transformations.md) of transformations, in place of the *transformations_on_true* and *transformations_on_false* parts. The former will be applied if the condition evaluates to true. The latter will be applied if the condition evaluates to false. The `if-else` and the succeeding transformations are optional.

For example, `if-ar_gt_1,w-200,if-else,w-300,if-end` and `if-ar_gt_1,w-200,if-end` are both valid.

{% hint style="info" %}
Conditional transformations are not supported for *gif* files yet
{% endhint %}

## Using with chained transformations
An if block can be one of the steps in a [chain](/features/image-transformations/chained-transformations.md) of transformations. For example, in `h-200,c-pad_resize:if-h_eq_200,w-100,if-end:rt-90`, the if block is the second step in a chain of three steps. The height value that this if block receives for evaluating the `h_eq_100` condition will be 200 since the previous step in the chain resizes the image to a height of 200. Hence, the condition will evaluate to true.

{% hint style="warning" %}
A step containing an if block cnanot contain any other comma-separated transform within that step. For example, `h-400:rt-90,if-h_eq_200,w-100,if-end:rt-90` or `h-400:if-h_eq_200,w-100,if-end,bl-10:rt-90` or `h-100,if-h_eq_200,w-100,if-end` are all invalid.
{% endhint %}
