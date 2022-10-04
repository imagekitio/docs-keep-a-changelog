# Google Cloud storage

You can configure ImageKit.io to fetch images from your Google Cloud bucket. This allows you to start leveraging ImageKit.io real-time image resizing, optimization, and fast CDN delivery for thousands or millions of existing images within minutes.

**Note:** We do not start copying images from your bucket as soon as you add it. Instead, we will fetch the particular image when you request it through ImageKit.io URL-endpoint. [Learn more](../how-it-works.md) to understand how this works. The images accessed from this origin will not appear in your [Media library](../../media-library/overview/).

The following tutorial will help you create a virtual identity for ImageKit to access your Google Cloud Storage bucket. This allows you to restrict ImageKit to only reading objects from your bucket. i.e. ImageKit will not be able to create/delete/update objects in your bucket.

In order for ImageKit to be able to access your Google Cloud Storage bucket, you will need to create a new Google Service Account and assign it the _`Storage Object Viewer`_ role for the bucket that you wish to integrate with ImageKit. This tutorial will help you do that. [Learn more about Google Cloud Storage's Identity and Access Management](https://cloud.google.com/storage/docs/access-control/iam).

{% hint style="info" %}
In case you find the following instructions invalid/outdated, you can refer to Google's official, albeit more verbose instructions on these pages:

* Step 1:** **[Creating and managing service accounts](https://cloud.google.com/iam/docs/creating-managing-service-accounts).
* Step 2: [Creating and managing service account keys](https://cloud.google.com/iam/docs/creating-managing-service-account-keys).
* Step 3: [Assign the bucket reader role to the service account](https://cloud.google.com/storage/docs/access-control/using-iam-permissions).
{% endhint %}

{% hint style="info" %}
If you have granted anonymous public access to your Google bucket, then you can choose to configure your bucket as a web server. Simply provide your bucket URL as the web-server URL in the web server configuration steps.[ See how to configure a web server.](https://docs.imagekit.io/integration/configure-origin/web-server-origin)\
\
However, note that it is advisable to configure your origin using secure integration as explained below.
{% endhint %}

## Step 1: Create access keys for the Service Account

The service account is a separate virtual identity that will be used by ImageKit to access the objects in your bucket. 

1. In the Cloud Console, go to the **Service Accounts** page. \
   [Go to the Service Accounts page](https://console.cloud.google.com/iam-admin/serviceaccounts).
2. Click **Select a project**, choose your project, and click **Open**.
3. Click **Create a Service Account**.
4. Enter a service account name (e.g. for-imagekit-access), an optional description, and then click **Save**.

## Step 2: Create access keys for the Service Account

We need to create access keys that will be used by ImageKit to authenticate itself to the Google Cloud Storage APIs.

1.  In the Cloud Console, go to the **Service Accounts** page.

    [Go to the Service Accounts page](https://console.cloud.google.com/iam-admin/serviceaccounts).
2. Click **Select a project**, choose a project, and click **Open**.
3. Click on the service account that we created in Step 1.
4. On the page for the service account, click on the **Add key **button in the **Keys **section.
5. Select **JSON** as the key type and click **Create**. This will download a JSON file.
6. You will upload this file to the ImageKit dashboard in [step 4](google-cloud-storage.md#step-4-configure-your-google-storage-bucket-with-your-imagekit-account).

 Make sure to store this file securely, as it cannot be downloaded again.

## Step 3: Assign the Bucket Reader role to the Service Account

1. In the Cloud Console, navigate to the bucket that you wish to integrate with ImageKit
2. Click on the **Permissions **tab
3. Click on **Add members** button
4. Enter the name of the service account we created in Step 1, and select the               **Cloud Storage -> Storage Object Viewer **role in the _Select a role_ option
5. Click **Save**

## Step 4: Configure your Google storage bucket with your ImageKit account

We have now created a virtual identity for ImageKit and granted it the Reader role for your bucket, which means that ImageKit can do nothing more than just read objects from your bucket. At this point, you should have the following with you:

* **Google Storage Bucket Name: **Name of the bucket that you want to integrate with ImageKit
* **Google Service Account key JSON file: **The JSON file you downloaded in step 2

Now, go to the [External Storage](https://imagekit.io/dashboard#external-storage) section in the dashboard, click on the **Add New Origin **button, select Google Cloud Storage in the **Origin Type** field, and enter the corresponding values, upload the JSON key file in the file field, and click submit.

## Step 5: Access the image through ImageKit.io URL-endpoint

When you add your first origin in the dashboard, the origin is by default made accessible through the [default URL-endpoint](../url-endpoints.md#default-url-endpoint) of your ImageKit.io account. For subsequent origins, you can either create a separate URL-endpoint or edit the existing URL-endpoint (including default) and make this newly added origin accessible by editing the [origin preference list](../url-endpoints.md#image-origin-preference). 

Let's look at a few examples to fetch the images:

* **Original image through Google Cloud Storage (old URL)**\
  ****`https://storage.cloud.google.com/bucket-name/rest-of-the-path.jpg`
* **The same master image using ImageKit.io URL-endpoint**\
  ****`https://ik.imagekit.io/your_imagekit_id/rest-of-the-path.jpg`
* **Resized 300x300 image**\
  ****`https://ik.imagekit.io/your_imagekit_id/tr:w-300,h-300/rest-of-the-path.jpg`

So when you request`https://ik.imagekit.io/your_imagekit_id/rest-of-the-path.jpg`, \
ImageKit.io uses the credentials provided by you to fetch the original image from the path `rest-of-the-path.jpg` in the Google Storage bucket using the official Google Storage SDK.

{% tabs %}
{% tab title="URL structure" %}
```markup
            URL-endpoint                transformation      image path                                    
┌─────────────────────────────────────┐┌─────────────┐┌───────────────────┐
https://ik.imagekit.io/your_imagekit_id/tr:w-300,h-300/rest-of-the-path.jpg
```
{% endtab %}
{% endtabs %}

If you get a "Not found" error while accessing the image, check out this [troubleshooting guide](../../limits-and-troubleshooting/404-not-found-error-troubleshooting.md).

{% hint style="info" %}
:man_mage:**Tips:** You can also use a [custom domain](../../testing-and-infrastructure-setup/using-custom-domain-name.md) like images.example.com.
{% endhint %}

## Step 6: Integrate and Go live

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
