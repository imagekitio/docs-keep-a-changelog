# Akeneo PIM

{% hint style="info" %}
This integration is currently not available for videos. It works for images and other static files like doc, PDF, etc.
{% endhint %}

Akeneo is a popular PIM used by several organizations to store, manage and enrich the information about their products.

With this integration between ImageKit and Akeneo, you can access the images, and other non-media files (PDFs, docs, etc.) stored in your PIM and deliver optimized and transformed versions of them in real-time. 

ImageKit supports three kinds of media files from Akeneo.
1. [Product Media Files](https://api.akeneo.com/api-reference.html#Productmediafile)
2. [Asset Media Files](https://api.akeneo.com/api-reference.html#Assetmediafile)
3. [Reference Entities Media Files](https://api.akeneo.com/api-reference.html#Referenceentitymediafile)

{% hint style="info" %}
ImageKit does not copy files from your PIM to its Media Library. Like all other external storage integrations, this integration accesses Akeneo on-demand when you request a file through ImageKit, and the file is unavailable in the cache.
{% endhint %}


## Step 1: Create a connection in Akeneo
We need to create a connection in Akeneo that will give us values for `client ID`, `secret`, `username` and `password`. These values will then be used to connect Akeneo to ImageKit. You can find the latest instructions for creating a connection in different versions of Akeneo on their [official documentation](https://api.akeneo.com/documentation/authentication.html#client-idsecret-generation). 

1. The [flow type](https://help.akeneo.com/pim/serenity/articles/manage-your-connections.html#choose-your-flow-type) will be "Destination connection" flow (data is flowing out of Akeneo).
2. Ensure that you set the appropriate permissions for this connection as defined [here](https://help.akeneo.com/pim/serenity/articles/manage-your-connections.html#set-the-permissions). ImageKit only needs access to be able to get your media files, and to generate the necessary authentication token for the same.

Once the connection is created, you can copy the credentials generated. The password is only shown once to you after the connection creation. So, make sure you save it somewhere.

![](https://ik.imagekit.io/ikmedia/website-assets/akeneo-credentials-in-connection-form_VyJ6mxtfo.png?ik-sdk-version=javascript-1.4.3&updatedAt=1660032958761)


## Step 2: Adding Akeneo External Storage to ImageKit
Use the credentials generated in step 1 to add an external storage of type "Akeneo PIM" in the ImageKit dashboard. 

1. Go to the [external storage section](https://imagekit.io/dashboard/external-storage) in your ImageKit.io dashboard, and under the Origins section, click on the "Add origin" button.
2. Choose **Akeneo PIM** from the origin type dropdown.
3. Give your origin a name, it will appear in the list of origins you have added. For example - **My Akeneo PIM**.
4. Provide the Base URL or the endpoint of the Akeneo PIM instance you want to access. Add the other credential values in the corresponding fields.
5. Leave the [advanced options](akeneo-pim.md#advanced-options-for-akeneo-pim-origin) as it is for now.
6.  Click on the Submit button.

![](https://ik.imagekit.io/ikmedia/website-assets/akeneo-origin-imagekit_ktD-D9QLL.png?ik-sdk-version=javascript-1.4.3&updatedAt=1660033576176)


## Step 3: Access the image through ImageKit.io URL-endpoint

When you add your first origin in the dashboard, the origin is by default made accessible through the [default URL-endpoint](../url-endpoints.md#default-url-endpoint) of your ImageKit.io account. For subsequent origins, you can either create a separate URL-endpoint or edit the existing URL-endpoint (including default) and make this newly added origin accessible by editing the [origin preference list](../url-endpoints.md#image-origin-preference). 

Every media file in Akeneo has a `data` or a `code` value that looks like this `f/d/1/d/fd1d_myimagefilename.png`. To access a file, you need to create a URL using this `data` or `code` value, along with the media file type (`media-file`, `asset-media-file`, or `reference-entities-media-file`) and the URL endpoint. For example, for the above `code`, you can access the file as shown below.

* **If it is a product media file**\
  [https://ik.imagekit.io/your_imagekit_id/`media-files`/f/d/1/d/fd1d_myimagefilename.png](https://ik.imagekit.io/your_imagekit_id/media-files/f/d/1/d/fd1d_myimagefilename.png)
* **If it is an asset media file**\
  [https://ik.imagekit.io/your_imagekit_id/`asset-media-files`/f/d/1/d/fd1d_myimagefilename.png](https://ik.imagekit.io/your_imagekit_id/asset-media-files/f/d/1/d/fd1d_myimagefilename.png)
* **If it is a reference entity media file**\
  [https://ik.imagekit.io/your_imagekit_id/`reference-entities-media-files`/f/d/1/d/fd1d_myimagefilename.png](https://ik.imagekit.io/your_imagekit_id/reference-entities-media-files/f/d/1/d/fd1d_myimagefilename.png)

For any of the three media types above, you can transform them by adding the transformation string like you would usually do for any other image origin.
* **Transforming an image to 300 x 200 dimension**\
  [https://ik.imagekit.io/your_imagekit_id/`tr:w-300,h-200`/media-files/f/d/1/d/fd1d_myimagefilename.png](https://ik.imagekit.io/your_imagekit_id/media-files/f/d/1/d/fd1d_myimagefilename.png)

{% tabs %}
{% tab title="URL structure" %}
```markup
            URL-endpoint                transformation  media type     data or code                                    
┌─────────────────────────────────────┐┌─────────────┐┌───────────┐┌───────────────────┐
https://ik.imagekit.io/your_imagekit_id/tr:w-300,h-300/media-files/rest-of-the-path.jpg
```
{% endtab %}
{% endtabs %}

If you get a "Not found" error while accessing the image, check out this [troubleshooting guide](../../limits-and-troubleshooting/404-not-found-error-troubleshooting.md).

{% hint style="info" %}
:man_mage:**Tips:** You can also use a [custom domain](../../testing-and-infrastructure-setup/using-custom-domain-name.md) like images.example.com.
{% endhint %}

## Step 3: Integrate and Go live

Now start using ImageKit.io URL endpoint in your application to accelerate image loading.

**Get started with our quick start guides and SDKs:**

{% content-ref url="../../getting-started/quickstart-guides/" %}
[quickstart-guides](../../getting-started/quickstart-guides/)
{% endcontent-ref %}

{% content-ref url="../../api-reference/api-introduction/sdk.md" %}
[sdk.md](../../api-reference/api-introduction/sdk.md)
{% endcontent-ref %}

**Learn about real-time image resizing:**

{% content-ref url="../../features/image-transformations/" %}
[image-transformations](../../features/image-transformations/)
{% endcontent-ref %}

## Advanced options for Akeneo PIM origin

### Include canonical response header

When enabled, the image response contains a Link header with the appropriate URL and rel=canonical. You will have to specify the base URL for the canonical header.

For example, if you set `https://www.example.com` as the base URL for canonical header, then the image response for URL `https://ik.imagekit.io/your_imagekit_id/rest-of-the-path.jpg` will have a Link header like this:

```http
Link: <https://www.example.com/rest-of-the-path.jpg>; rel="canonical"
```
