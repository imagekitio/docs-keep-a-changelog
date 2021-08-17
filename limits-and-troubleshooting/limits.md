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

<table>
  <thead>
    <tr>
      <th style="text-align:left">Name</th>
      <th style="text-align:left">Default</th>
      <th style="text-align:left">Adjustable</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p>Image input size</p>
        <p>(accessed from your origin or media library)</p>
      </td>
      <td style="text-align:left">40MB</td>
      <td style="text-align:left">Yes</td>
    </tr>
    <tr>
      <td style="text-align:left">Output image resolution
        <br />(width x height; for example 1000 x 1000 = 1MP)</td>
      <td style="text-align:left">100 MP</td>
      <td style="text-align:left">Yes</td>
    </tr>
    <tr>
      <td style="text-align:left">Max image transformation dimensions (dimensions greater than this in the
        transformation string will get ignored)</td>
      <td style="text-align:left">65535 px</td>
      <td style="text-align:left">No</td>
    </tr>
    <tr>
      <td style="text-align:left">Max WebP image transformation dimensions (dimensions greater than this
        will not work for transformation)</td>
      <td style="text-align:left">16383 px</td>
      <td style="text-align:left">No</td>
    </tr>
  </tbody>
</table>

## Video limits

Video real-time manipulation and optimization service in alpha release. Refer to [limits](../features/video-transformation/#limitations-of-the-alpha-release) before using in production.





