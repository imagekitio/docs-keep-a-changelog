# Quality Optimization

## Video quality

Similar to images, there is a tradeoff between visual quality and file sizes. However, it is possible to compress a video file without any loss in perceptual visual quality.

ImageKit.io allows you to choose a quality level between `1` and `100`. `1` results in the lowest perceptual quality and smallest file size. `100` results in the highest perceptual quality and biggest file size.

{% hint style="info" %}
Choosing 100 quality will not improve an already low-quality video. However, it will increase the size of the video file. It is recommended to not use a 100 quality setting.
{% endhint %}

## Frequently asked questions

### How to enable/disable video quality optimization

Video quality optimization is off by default. You can enable or disable it for your account under [video settings](https://imagekit.io/dashboard/settings/images) in your dashboard.

Select settings from the left main menu. Inside Videos, under optimization, toggle "optimize video quality" control.

### How to control video quality from the dashboard?

By default, ImageKit sets the video quality to 50. You can change this.

Select settings from the left main menu. Inside Videos, under optimization, select the default quality level.

![Video default quality settings](<../../.gitbook/assets/video-quality-optimisation.png>)

### How to override quality?

Use `q` parameter to override default quality settings.

For example, a video file served in `q-20` is 397KB in size and has relatively low perceptual quality compared to a `q-90` which is around 7.6MB.

![Quality 20](<../../.gitbook/assets/Screenshot 2021-07-16 at 5.12.33 PM.png>)

![Quality 90](<../../.gitbook/assets/Screenshot 2021-07-16 at 5.13.01 PM (1).png>)
