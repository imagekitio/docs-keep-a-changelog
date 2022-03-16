---
description: Default limits on input or output media in ImageKit
---

# Limits

Your ImageKit account has some default limits and quotas to prevent abuse and protect our infrastructure. This ensures that ImageKit.io is fast and stable for everyone.

{% hint style="info" %}
Most of these limits can be adjusted upon request. Contact support@imagekit.io if you need to increase these limits on your account.
{% endhint %}

There are certain limits, like those on image transformation dimensions, which are a limitation of the underlying image format. These limits cannot be modified.

## Image limits

| Name                                                                                                             | Default  | Adjustable |
| ---------------------------------------------------------------------------------------------------------------- | -------- | ---------- |
| <p>Image input size</p><p>(accessed from your origin or media library)</p>                                       | 40MB     | Yes        |
| <p>Input image resolution <br>(width x height; for example 1000 x 1000 = 1MP)</p>                               | 100 MP   | Yes        |
| Max image transformation dimensions (dimensions greater than this in the transformation string will get ignored) | 65535 px | No         |
| Max WebP image transformation dimensions (dimensions greater than this will not work for transformation)         | 16383 px | No         |

## Video limits

Video real-time manipulation and optimization service is in Beta release. Refer to [limits](../features/video-transformation/#limitations-of-the-beta-release) before using in production.



