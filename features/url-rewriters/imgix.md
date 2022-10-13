# Imgix URL-rewriter

ImageKit can translate transformations written in Imgix's syntax to ImageKit's syntax. Learn how to use a URL-rewriter on [this page](/features/url-rewriters).

{% hint style="info" %}
**Beta feature**\
This feature is currently in beta. As a result, some transformations may work differently than expected.
{% endhint %}

## Supported Transformations

The following Imgix transformations are supported for translation:

| Imgix transform | Example translation | Remarks |
| - | - | - |
| <p>width</p> | <ul> <li> `w=100` => `w-100`</li> </ul> |  |
| <p>height</p> | <ul> <li>`h=100` => `h-100`</li> </ul> |  |
| <p>aspect ratio</p> | <ul> <li>`ar=2:3` => `ar-2-3`</li><li>`ar=3:1` => `ar=3-1`</li><li>`ar=2:4` => `ar=2-4`</li> </ul> | <ul>Aspect ratio will only work when we provide either width or height (not both) and fit</ul> |
| <p>crop</p> | <ul> <li>`crop=top` => `fo-top`</li><li>`crop=top,left` => `fo-top_left`</li><li>`crop=left,top` => `fo-top_left`</li><li>`crop=faces` => `fo-auto`</li> </ul> | <ul>Supported values: <ul><li>top</li><li>left</li><li>right</li><li>bottom</li><li>top,left</li><li>top,right</li><li>bottom,right</li><li>bottom,left</li><li>left,top</li><li>right,top</li><li>right,bottom</li><li>left,bottom</li><li>faces</li></ul></ul> |
| <p>fit</p> | <ul> <li>`fit=clip` => `c-at_max_enlarge`</li><li>`fit=fill` => `cm-pad_resize`</li><li>`fit=scale` => `c-force`</li><li>`fit=crop` => `no mapping required`</li> <li>`fit=fillmax` => `cm-pad_resize`</li><li>`fit=max` => `c-at_max`</li><li>`fit=facearea` => `fo-face`</li> </ul> | <ul>Supported values: <ul><li>clip</li><li>crop</li><li>fill</li><li>fillmax</li><li>max</li><li>scale</li><li>facearea</li></ul></ul><ul>For fit=fillmax, if the requested dimensions are greater than the original dimensions, then padding will be added along either one of the dimensions (height or width), not both </ul> |
 | <p>fill</p> | |<ul>Supported values: <ul><li>solid</li></ul></ul><ul>blur will fallback to solid</ul>|
| <p>format</p> | <ul> <li>`fm=jpg` => `f-jpg`</li> </ul> | <ul>Supported values: <ul><li>jpeg->jpeg</li><li>jpg->jpg</li><li>jp2->jpg</li><li>png->png</li><li>webp->webp</li><li>gif->gif</li><li>avif->avif</li><li>png8->png</li><li>png32->png</li></ul></ul> |
| <p>quality</p> | <ul> <li>`q=60` => `q-60`</li> </ul> |  |
| <p>orient</p> | | <ul>Supported values: <ul><li>1->0</li><li>6->90</li><li>3->180</li><li>8->270</li><li>0->0</li><li>90->90</li><li>180->180</li><li>270->270</li></ul></ul><ul>Counterclockwise rotation via orient is not supported</ul> |
| <p>fill-color</p> | <ul> <li>`fill-color=red` => `bg-red`</li><li>`fill-color=BLUE` => `bg-blue`</li><li>`fill-color=AAAA54` => `bg-AAAA54`</li><li>`fill-color=A9AAAA54` => `bg-AAAA5466`</li><li>`fill-color=B4F` => `bg-BB44FF`</li><li>`fill-color=9B4F` => `bg-BB44FF60`</li> </ul> | <ol><li>Color values are case insensitive</li><li>Alpha values in RGB codes are accepted in the range [00-FF] and translated to a value in the range [00-99]</li></ol> |
| <p>bg</p> | <ul> <li>`bg=red` => `bg-red`</li><li>`bg=BLUE` => `bg-blue`</li><li>`bg=AAAA54` => `bg-AAAA54`</li><li>`bg=A9AAAA54` => `bg=AAAA5466`</li><li>`bg=B4F` => `bg-BB44FF`</li><li>`bg=9B4F` => `bg-BB44FF60`</li> </ul> | <ol><li>Color values are case insensitive</li><li>Alpha values in RGB codes are accepted in the range [00-FF] and translated to a value in the range [00-99]</li></ol> |
| <p>dpr</p> | <ul> <li>`dpr=3.0` => `dpr-3.0`</li> </ul> |  |
| <p>sharp</p> | <ul> <li>`sharp=10` => `e-sharpen-200`</li><li>`sharp=100` => `e-sharpen-2000`</li></ul> | <ul>Final output from ImageKit may vary from output from Imgix</ul> |
| <p>con</p> | <ul> <li>`con=30` => `e-contrast`</li><li>`con=-50` => `e-contrast`</li></ul> | <ul>Final output from ImageKit may vary from output from Imgix</ul> |
| <p>usm</p> | <ul> <li>`usm=40` => `e-usm-2.5-8.00-1-0.05`</li></ul> | <ul>Final output from ImageKit may vary from output from Imgix</ul> |
| <p>usmrad</p> | <ul> <li>`usm=-50&usmrad=2` => `e-usm-2-0.00-1-0.05`</li></ul> | <ul>usmrad only works in conjunction with usm</ul><ul>Final output from ImageKit may vary from output from Imgix</ul> |
| <p>blur</p> | <ul> <li>`blur=100` => `bl-5.00`</li><li>`blur=90` => `bl-4.50`</li></ul> | <ul>Final output from ImageKit may vary from output from Imgix</ul> |
| <p>trim</p> | <ul> <li>`trim=auto` => `t-true`</li><li>`trim=color` => `t-true`</li></ul> | <ul>Final output from ImageKit may vary from output from Imgix</ul> |



**Note:** The rewriter will silently ignore any transformation with a valid key but an invalid value. For example, `h=100&w=<invalid>&blur=100`  will be translated to `h-100,blur-5.00`. Refer to the table to see what constitutes a valid value for different transforms.
