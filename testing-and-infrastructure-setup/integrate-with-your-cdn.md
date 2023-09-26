# Using with your CDN

By default, ImageKit.io offers a global CDN (Amazon CloudFront). It means that not only are your images optimized for size and you get to transform them as per your requirements, you get a superfast delivery network with ImageKit.io that helps deliver images to your end users in milliseconds. You can use ImageKit.io's integrated CDN for delivering not only images, but also JS/CSS/fonts, or any other static asset.

However, you can integrate ImageKit.io with your CDN as well. This could be useful if you are in a long-term contract with any existing CDN vendor.

{% hint style="info" %}
**Custom Enterprise Paid plan feature**\
****This setup is only supported for paid accounts which requires a minimum billing and traffic commitment per month. Please get in touch with the support team if you want to use ImageKit.io with your CDN.
{% endhint %}

## Supported CDNs

We have integrated and tested ImageKit.io with the following CDNs so far:

* [x] Akamai
* [x] Azure
* [x] Alibaba Cloud CDN
* [x] CloudFlare (only on Enterprise plan in CloudFlare)
* [x] CloudFront
* [x] Fastly
* [x] Google CDN
* [x] Zenedge

## Features availability

| Feature                          | Available      |
| -------------------------------- | -------------- |
| Image transformation             | Yes            |
| Automatic image optimization     | <p>Yes<br></p> |
| Automatic format conversion      | Yes            |
| Network-based media optimization | Yes            |
| Data Saver                       | No             |
| Security                         | Yes            |

## Integration steps

1. Write to us at [support@imagekit.io](mailto:support@imagekit.io) or [sales@imagekit.io](mailto:sales@imagekit.io).
2. Our team will work with you to do the necessary configuration changes in your CDN to support automatic format conversion, client hints, and data saver mode.
3. Test the configuration of your CDN and go live.
