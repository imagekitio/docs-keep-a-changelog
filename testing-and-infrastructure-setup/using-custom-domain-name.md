# Using with a custom domain name

## What is a custom domain?

Let's say that your website is loading at `https://www.example.com`, now you can configure ImageKit.io to serve your images via a custom domain, for example - `https://images.example.com`.

## How to configure a custom domain?

Custom domain only works on ImageKit.io's Premium Plan or a larger custom plan. On the Premium plan, one custom domain is available for free. The additional custom domains can be purchased at an additional cost. The ImageKit.io support team handles the SSL verification, certificate procurement, and deployment process. Once you are ready, tell us the custom domain you want to configure via support chat or by emailing us at [support@imagekit.io](mailto:support@imagekit.io). Our team will provide you with the next steps.

{% hint style="info" %}
**Automatic SSL certificate procurement**\
****We use Let’s Encrypt to procure and renew certificates for your domains automatically. After the initial setup, we will take care of certificate renewals.
{% endhint %}

## Accessing the image through a custom domain

Once the custom domain name is configured, you will be able to fetch the image using this domain instead of ImageKit.io URL endpoint `https://ik.imagekit.io/your_imagekit_id`.

Suppose the old URL was - `https://ik.imagekit.io/your_imagekit_id/rest-of-the-path.jpg`

The new URL would become - `https://images.example.com/rest-of-the-path.jpg`
