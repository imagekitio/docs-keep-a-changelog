# Cloudinary URL-rewriter

ImageKit can translate transformations written in Cloudinary's syntax to ImageKit's syntax. Learn how to use a URL-rewriter on [this page](/features/url-rewriters).

{% hint style="info" %}
**Beta feature**\
This feature is currently in beta. Some transformations may not work as expected.
{% endhint %}

## Supported Transformations

The following Cloudinary transformations are supported for translation:

| Cloudinary transform | Example translation | Remarks |
| - | - | - |
| <p>width</p> | <ul> <li> `w_100` => `w-100`</li> </ul> |  |
| <p>height</p> | <ul> <li>`h_100` => `h-100`</li> </ul> |  |
| <p>aspect ratio</p> | <ul> <li>`ar_2.5` => `ar-2.5-1`</li><li>`ar_3` => `ar-3-1`</li><li>`ar_4:3` => `ar-4-3`</li> </ul> |  |
| <p>gravity</p> | <ul> <li>`g_north` => `fo-top`</li><li>`g_center` => `fo-center`</li><li>`g_auto` => `fo-auto`</li><li>`g_face` => `fo-face`</li> </ul> | Supported values: <ul><li>north</li><li>south</li><li>east</li><li>west</li><li>north_west</li><li>north_east</li><li>south_east</li><li>south_west</li><li>auto</li><li>custom</li><li>face</li></ul> |
| <p>x</p> | <ul> <li>`x_100` => `x-100`</li> </ul> |  |
| <p>y</p> | <ul> <li>`y_100` => `y-100`</li> </ul> |  |
| <p>format</p> | <ul> <li>`f_jpg` => `f-jpg`</li> </ul> | Supported values: <ul><li>auto</li><li>jpg</li><li>jpeg</li><li>png</li><li>webp</li><li>gif</li><li>avif</li></ul> |
| <p>quality</p> | <ul> <li>`q_60` => `q-60`</li><li>`q_auto` => `q-70`</li><li>`q_auto:low` => `q-50`</li> </ul> | Auto quality mapping (images): <ul><li>low: 50</li><li>eco: 60</li><li>good: 70</li><li>best: 86</li></ul> Auto quality mapping (videos): <ul><li>low: 40</li><li>eco: 45</li><li>good: 50</li><li>best: 60</li></ul> |
| <p>angle</p> | <ul> <li>`a_90` => `rt-90`</li><li>`a_-67` => `rt-N67`</li> </ul> |  |
| <p>radius</p> | <ul> <li>`r_20` => `r-20`</li><li>`r_20:30:40:50` => `r-20`</li><li>`r_max` => `r-max`</li> </ul> |  |
| <p>effects</p> | <ul> <li>`e_blur` => `bl-1`</li><li>`e_blur:500` => `bl-5`</li><li>`e_grayscale` => `e-grayscale`</li><li>`e_contrast:100` => `e-contrast`</li><li>`e_sharpen:500` => `e-sharpen-5`</li><li>`e_unsharp_mask:500` => `e-usm-0-5-1-0.05`</li><li>`e_unsharp_mask:250` => `e-usm-0-2.5-1-0.05`</li><li>`e_trim` => `t-true`</li><li>`e_trim:20` => `t-20`</li><li>`e_trim:20:red` => `t-20`</li>  </ul> | Supported values: <ul><li>blur</li><li>grayscale</li><li>contrast</li><li>sharpen</li><li>unsharp_mask</li><li>trim</li></ul>Final output from ImageKit may vary from output from Cloudinary |
| <p>border</p> | <ul> <li>`bo_5px_solid_red` => `b-5-red`</li><li>`bo_5px_solid_BLUE` => `b-5-blue`</li><li>`bo_5px_solid_rgb:AAAA54` => `b-5-AAAA54`</li><li>`bo_5px_solid_rgb:AAAA54A9` => `b-5-AAAA5466`</li><li>`bo_5px_solid_rgb:B4F` => `b-5-BB44FF`</li><li>`bo_5px_solid_rgb:B4F9` => `b-5-BB44FF60`</li> </ul> | <ol><li>Color values are case insensitive</li><li>Alpha values in RGB codes are accepted in the range [00-FF] and translated to a value in the range [00-99]</li></ol> |
| <p>background</p> | <ul> <li>`b_red` => `bg-red`</li><li>`b_BLUE` => `bg-blue`</li><li>`b_rgb:AAAA54` => `bg-AAAA54`</li><li>`b_rgb:AAAA54A9` => `bg-AAAA5466`</li><li>`b_rgb:B4F` => `bg-BB44FF`</li><li>`b_rgb:B4F9` => `bg-BB44FF60`</li> </ul> | <ol><li>Color values are case insensitive</li><li>Alpha values in RGB codes are accepted in the range [00-FF] and translated to a value in the range [00-99]</li></ol> |
| <p>colorize</p> | <ul> <li>`co_red` => `otc-red`</li><li>`co_BLUE` => `otc-blue`</li><li>`co_rgb:AAAA54` => `otc-AAAA54`</li><li>`co_rgb:AAAA54A9` => `otc-AAAA5466`</li><li>`co_rgb:B4F` => `otc-BB44FF`</li><li>`co_rgb:B4F9` => `otc-BB44FF60`</li> </ul> | <ol><li>Only supported in the context of text overlays</li><li>Color values are case insensitive</li><li>Alpha values in RGB codes are accepted in the range [00-FF] and translated to a value in the range [00-99]</li></ol> |
| <p>DPR</p> | <ul> <li>`dpr_3.0` => `dpr-3.0`</li> </ul> |  |
| <p>crop</p> | <ul> <li>`c_crop` => `cm-extract`</li><li>`c_fill` => `c-maintain_ratio`</li><li>`c_fit` => `c-at_max`</li><li>`c_pad` => `cm-pad_resize`</li><li>`c_scale` => `c-force`</li>  </ul> | Supported values: <ul><li>crop</li><li>fill</li><li>fit</li><li>pad</li><li>scale</li></ul> |
| <p>text overlays</p> | <ul> <li>`l_text:Roboto_50:<value>` => `ot-<value>,otf-Roboto,ots-50`</li><li>`l_text:Roboto_50_bold_left:<value>` => `ot-<value>,otf-Roboto,ots-50,ott-b,otia-left`</li><li>`l_text:<fontPath>_50_bold_left:<value>` => `ot-<value>,otf-<fontPath>,ots-50,ott-b,otia-left`</li> </ul> | Supported text style qualifiers: <ul><li>bold</li><li>italic</li><li>left</li><li>right</li><li>center</li><li>start</li><li>end</li></ul> [Custom fonts](/features/image-transformations/overlay.md#using-custom-fonts) can be used by specifying full path to a font file in your Media Library |
| <p>image overlays</p> | <ul> <li>`l_<imagePath>` => `oi-<imagePath>`</li> </ul> | Full path to the overlaid image in the Media Library must be specified |
| <p>named transform</p> | <ul> <li>`t_<transform_name>` => `n-<transform_name>`</li> </ul> | ImageKit does not translate the actual underlying transformations associated with your named transforms. They must be manually translated and [added in your ImageKit dashboard](/features/named-transformations.md#creating-named-transformations) while retaining the same name. |
| <p>fl_progressive</p> | <ul> <li>`fl_progressive` => `pr-true`</li><li>`fl_progressive:steep` => `pr-true`</li><li>`fl_progressive:semi` => `pr-true`</li><li>`fl_progressive:none` => `pr-false`</li> </ul> | |
| <p>fl_layer_apply</p> |  | Marks the end of a group of transformations being applied in the context of an overlay. ImageKit detects the presence of an `fl_layer_apply` and applies the relevantly positioned transforms to the overlaid asset instead of the main asset. |
| <p>start offset</p> | <ul> <li>`so_10.5599` => `so-10.56`</li> </ul> |  |
| <p>end offset</p> | <ul> <li>`eo_10.5522` => `eo-10.55`</li> </ul> |  |
| <p>duration</p> | <ul> <li>`du_10.5500` => `du-10.55`</li> </ul> |  |
| <p>zoom</p> | <ul> <li>`z_2.0` => `z-2.0`</li><li>`z_0.5` => `z-0.5`</li></ul> |  |

**Note:** The rewriter will silently ignore any transformation that has a valid key, but with an invalid value. For example, `h_100,w_<invalid>/a_90`  will be translated to `h-100:rt-90`. Refer to the table to see what constitutes a valid value for different transforms.
