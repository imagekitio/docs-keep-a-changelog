# Firebase storage

You can configure ImageKit.io to fetch images from your Firebase storage. This allows you to start using ImageKit.io real-time image resizing, optimization, and fast CDN delivery for thousands or millions of existing images within minutes. Also, you get to leverage Firebase's authentication and authorization in your application.

**Note:** We do not start copying images from your storage as soon as you add the origin. Instead, we will fetch the particular image when you request it through ImageKit.io URL-endpoint. [Learn more](../how-it-works.md) to understand how this works. The images accessed from this origin will not appear in your [Media library](../../media-library/overview/).

## Step 1: Configure origin

1. Go to the [external storage section](https://imagekit.io/dashboard#external-storage) in your ImageKit.io dashboard, and under the Origins section, click on the "Add origin" button.
2. Choose **Web server** from the origin type dropdown.
3. Give your origin a name. It will appear in the list of origins you have added. For example - **My Firebase Storage**.
4. Enter `https://firebasestorage.googleapis.com` in the base URL field.
5. Leave the [advanced options](web-server-origin.md#advanced-options-for-web-server-origin) as it is for now.
6. Click on the Submit button.

## Step 2: Access the image through ImageKit.io URL-endpoint

When you add your first origin in the dashboard, the origin is by default made accessible through the [default URL-endpoint](../url-endpoints.md#default-url-endpoint) of your ImageKit.io account. For subsequent origins, you can either create a separate URL-endpoint or edit existing URL-endpoint (including default) and make this newly added origin accessible by editing the [origin preference list](../url-endpoints.md#image-origin-preference). 

Let's look at a few examples to fetch the images:

* **Original image through Firebase storage (old URL)**\
  [https://firebasestorage.googleapis.com/rest-of-the-path.jpg?alt=media\&token={TOKEN}](https://firebasestorage.googleapis.com/rest-of-the-path.jpg?alt=media\&token={TOKEN})
* **The same master image using ImageKit.io URL-endpoint**\
  [https://ik.imagekit.io/your_imagekit_id/rest-of-the-path.jpg?alt=media\&token={TOKEN}](https://ik.imagekit.io/your_imagekit_id/rest-of-the-path.jpg?alt=media\&token={TOKEN})
* **Resized 300x300 image**\
  [https://ik.imagekit.io/your_imagekit_id/`tr:w-300,h-300`/rest-of-the-path.jpg?alt=media\&token={TOKEN}](https://ik.imagekit.io/your_imagekit_id/tr:w-300,h-300/rest-of-the-path.jpg?alt=media\&token={TOKEN})

So when you request `https://ik.imagekit.io/your_imagekit_id/rest-of-the-path.jpg?alt=media&token={TOKEN}`, ImageKit.io internally fetches the file from `https://firebasestorage.googleapis.com/rest-of-the-path.jpg?alt=media&token={TOKEN}`

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

## Step 3: Integrate and Go live

To start using the ImageKit.io URL-endpoint in your application you will need to replace the base URL of images in your application code. Here are the steps and code samples:

1. Get the Firebase URL for the asset by calling `.getDownloadURL()` method on the storage reference.
2. In the above download URL replace `https://firebasestorage.googleapis.com` with your ImageKit.io URL-endpoint.

{% tabs %}
{% tab title="Web" %}
```javascript
storageRef.child('images/stars.jpg').getDownloadURL().then(function(url) {
  // `url` is the download URL for 'images/stars.jpg'
  
  // Replace the firebase URL with ImageKit.io URL-endpoint
  url = url.replace("https://firebasestorage.googleapis.com","https://ik.imagekit.io/your_imagekit_id");

  // This can be downloaded directly:
  var xhr = new XMLHttpRequest();
  xhr.responseType = 'blob';
  xhr.onload = function(event) {
    var blob = xhr.response;
  };
  xhr.open('GET', url);
  xhr.send();

  // Or inserted into an <img> element:
  var img = document.getElementById('myimg');
  img.src = url;
}).catch(function(error) {
  // Handle any errors
});
```
{% endtab %}

{% tab title="iOS" %}
```swift
// Create a reference to the file you want to download
let starsRef = storageRef.child("images/stars.jpg")

// Fetch the download URL
starsRef.downloadURL { url, error in
  if let error = error {
    // Handle any errors
  } else {
    /*
       Replace https://firebasestorage.googleapis.com 
       with your ImageKit.io URL-endpoint.
    */
    let imageKitURL = url.replacingOccurrences(of: "https://firebasestorage.googleapis.com", with: "https://ik.imagekit.io/your_imagekit_id")
    // Use imageKitURL in your application
  }
}
```
{% endtab %}

{% tab title="Android (Java)" %}
```java
storageRef.child("users/me/profile.png").getDownloadUrl().addOnSuccessListener(new OnSuccessListener<Uri>() {
    @Override
    public void onSuccess(Uri uri) {
        // Got the download URL for 'users/me/profile.png'
        
        /*
           Replace https://firebasestorage.googleapis.com 
           with your ImageKit.io URL-endpoint.
        */
    }
}).addOnFailureListener(new OnFailureListener() {
    @Override
    public void onFailure(@NonNull Exception exception) {
        // Handle any errors
    }
});
```
{% endtab %}
{% endtabs %}

### **Next steps**

Now start using ImageKit.io URL endpoint in your application to accelerate image loading.

**Get started with our quick start guides and SDKs:**

{% content-ref url="../../getting-started/quickstart-guides/" %}
[quickstart-guides](../../getting-started/quickstart-guides/)
{% endcontent-ref %}

{% content-ref url="../../api-reference/api-introduction/sdk.md" %}
[sdk.md](../../api-reference/api-introduction/sdk.md)
{% endcontent-ref %}

#### **Learn about real-time image resizing:**

{% content-ref url="../../features/image-transformations/" %}
[image-transformations](../../features/image-transformations/)
{% endcontent-ref %}
