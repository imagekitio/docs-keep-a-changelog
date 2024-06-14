---
description: >-
  Embeddable media library widget to integrate ImageKit Media Library in your
  existing web application or CMS.
---

# Media library widget

The [media library widget](https://github.com/imagekit-developer/embeddable-media-library) provides a way to easily integrate [ImageKit Media Library](../../media-library/overview/) into your CMS or any other web application. Using this, you can access all the assets stored in your Media Library from your existing CMS or application.

Integrating the media library widget is straightforward as you will discover in this document.

You can also see the live hosted demo on [codesandbox.io](https://codesandbox.io/p/sandbox/media-library-widget-mp484k). Tweak parameters to understand the different options.

## Media library widget plugin features

The JavaScript-based plugin provides a way to seamlessly access your [ImageKit Media Library](../../media-library/overview/) within your own CMS or web application. You can search all assets stored in the media library and consume them in your application.

**The plugin allows you to:**

* Login to ImageKit from within your CMS.
* Search and insert images directly into your CMS from your ImageKit Media Library.
* Configure UI view options, such as inline or modal-based Media Library panel.
* Supply a custom container class so that you can customize the styling to match your application theme.
* Open the Media Library directly to a specified state, such as opening to a specific folder, or opening a specific asset.
* Allowing single/multiple asset(s) insertion or limiting the maximum number of assets that can be inserted via the plugin.

![](../../.gitbook/assets/15-mlw-home.png)

## **How to integrate the plugin in your app**

### Integration overview

Integrating the ImageKit Media Library plugin into a page in your web application or CMS interface is straightforward. We will go through the following steps in details one by one:

1. [**Include the plugin script and create a container element**](./#1-include-the-plugin-script) - Include the plugin script file in the web page to embed the Media Library. Create a container element where the widget will be rendered.

2. [**Initialize the media library widget**](https://app.gitbook.com/@imagekit-io/s/docs/\~/drafts/-MOeRVlxk26WIzUeOQ4D/sample-projects/embeddable-media-library#2-initialize-the-media-library-widget) - Provide the configuration options and callback to initialize the widget. These options include the mandatory container within which the Media Library Widget UI will be rendered and some other optional settings as described later in the page.

3. [**Instantiate the Media library widget and open it**](./#4-instantiate-the-embedded-media-library-and-open-it) - Using the configuration options and callback method, now the plugin can be instantiated through the `IKMediaLibraryWidget` constructor and used by navigating to the webpage where it has been embedded.

4. [**Insert images from the Media Library via the plugin**](./#5-embed-images-from-the-media-library-via-the-plugin) - Choose images and other files from within the media library interface and insert them in your CMS or web application.

### **1. Include the plugin script and create a container element**

Insert the following script on the web page where you want to access the Media Library plugin:

```markup
<script src="https://unpkg.com/imagekit-media-library-widget/dist/imagekit-media-library-widget.min.js"></script>
```

{% hint style="info" %}
Internet Explorer does not have native support for all Media Library Widget features. We recommend accessing it on a supported browser, such as Google Chrome or Mozilla Firefox.
{% endhint %}

Create an HTML container element where the widget will be rendered:

```markup
<div id="container"></div>
```

### 2. Initialize the media library widget

Using the configuration options and callback function, let’s instantiate the plugin:

```javascript
const mediaLibraryWidget = new IKMediaLibraryWidget(config, callback);
```

#### Parameters

* **config -** Configuration options that are applied to the Media library widget instance. 
* **callback -** This function is called after the user clicks "Insert" button in the Media library. The callback receives a JSON payload of the selected images.

{% hint style="info" %}
**Note:** Check that `renderOpenButton` option is set to `true` in the plugin configuration option for the view control to become available.
{% endhint %}

#### Plugin options

The plugin accepts the following configuration options, including the mandatory `container`. Apart from this, it also accepts some optional settings that control the plugin's behavior and styling.

| Option             | Datatype               | Description                                                                                                                                                      | Default value                                                                             |
| ------------------ | ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| `container`        | String, or DOM element | <p><strong>Required</strong><br><em><strong></strong></em>The name of the container within which the Media Library will be rendered. Supports CSS selectors.</p> | None                                                                                      |
| `className`        | String                 | Optional styling class to apply to the container element.                                                                                                        | None                                                                                      |
| `dimensions`       | Object                 | Dimensions of the Media Library `container` element.                                                                                                             | <p><code>{ height: '100%',</code></p><p><code>width: '100%' </code><br><code>}</code></p> |
| `view`             | String                 | Toggle Media Library interface mode: `modal` or `inline`                                                                                                         | `'modal'`                                                                                 |
| `renderOpenButton` | Boolean                | Toggle whether button to open Media Library UI is displayed. Set this to`false` if using a custom editor plugin or custom open trigger.                          | `true`                                                                                    |
`mlSettings`         | Object                |<p><strong>Optional</strong><br><em><strong></strong></em> Used to customise/control Media library behaviour based on given settings. See the table below to learn about various settings.</p> | None

`mlSettings` can have the following properties:
 Option             | Datatype               | Description                                                                                                                                                      | Default value                                                                             |
| ------------------ | ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| `initialView`| Object | Instructs the widget to open in specified initial state. See the table below to learn about its subfields. | None |
| `multiple`| Boolean | Whether to allow users to select multiple assets during insertion via plugin. | None |
| `maxFiles`| Number | Max number of media assets that can be inserted via plugin in a single operation. Only relevant when `multiple=true`. | None |

`initialView` can only have one of the following properties:

{% hint style="info" %}
You cannot pass more than one property in `initialView`. For anything custom, use `searchQuery`.
{% endhint %}

 Option             | Datatype               | Description                                                                                                                                                      | Default value                                                                             |
| ------------------ | ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| `searchQuery`        | String | Query string in a Lucene-like query language same as the `searchQuery` parameter used in [Search API](/api-reference/media-api/list-and-search-files.md#advanced-search-queries). It will instructs the widget to open in search view with the  results filtered as per query passed. | None |
| `folderPath`        | String | Instructs the widget to open in specified folder. For e.g. `folderPath: '/some-folder'` | None |
| `fileId`        | String | Instructs the widget to open in file detail view of the specified ID. For e.g. `fileId: 'some-id'` | None 
| `collection`        | Object | <p>Instructs the widget to open in collections view.<br>Passing an empty object opens the collection view showing all collections, for example: `collection: {}`<br>Passing a collection ID opens the widget with a specific collection displayed, for example: `collection: { id: 'some-id' }`</p> | None|
| `fileType` | String | <p>Instructs the widget to open in search view with the  results filtered to a specific asset type.<br>Accepts four values:<ul><li>`image`: only search in image type files</li><li>`video`: only search in video type files</li><li>`cssJs`: only search in css, js or ts files</li><li>`others`: other than images or videos</li></ul></p> | None |


#### **Configuration options samples:**

##### **Open Media library(ML) home**

```javascript
const config = {
  container: '#container',   // the element in which the widget will be rendered
  className: 'media-library-widget',
  dimensions: {
    height: '100%',
    width: '100%',
  },
  view: 'modal',  // modal (default) | inline
  renderOpenButton: true  // false | true (default)
};
```

![](../../.gitbook/assets/15-mlw-home.png)

##### **Open ML at a specific path**

The following config will open ML widget at path: `/marketing/banner`.
```javascript
const config = {
  container: '#container',   // the element in which the widget will be rendered
  className: 'media-library-widget',
  dimensions: {
    height: '100%',
    width: '100%',
  },
  view: 'modal',  // modal (default) | inline
  renderOpenButton: true,  // false | true (default)
  mlSettings: {
    initialView: {
      folderPath: '/marketing/banner'
    },
  }
};
```

![](../../.gitbook/assets/16-mlw-specific-folder.png)

##### **Open specific file**

The following config will open specific file detail page
```javascript
const config = {
  container: '#container',   // the element in which the widget will be rendered
  className: 'media-library-widget',
  dimensions: {
    height: '100%',
    width: '100%',
  },
  view: 'modal',  // modal (default) | inline
  renderOpenButton: true,  // false | true (default)
  mlSettings: {
    initialView: {
      fileId: "5fd874c040308546019f0500"
    },
  }
};
```

![](../../.gitbook/assets/14-mlw-file-detail.png)

##### **Open certain type of files**

Only open `videos` type files

```javascript
const config = {
  container: '#container',   // the element in which the widget will be rendered
  className: 'media-library-widget',
  dimensions: {
    height: '100%',
    width: '100%',
  },
  view: 'modal',  // modal (default) | inline
  renderOpenButton: true,  // false | true (default)
  mlSettings: {
    initialView: {
      fileType: 'videos'
    },
  }
};
```

![](../../.gitbook/assets/09-mlw-file-type.png)

##### **Custom search query**

Opens in a search view with all the assets with name starting with `pexel`

```javascript
const config = {
  container: '#container',   // the element in which the widget will be rendered
  className: 'media-library-widget',
  dimensions: {
    height: '100%',
    width: '100%',
  },
  view: 'modal',  // modal (default) | inline
  renderOpenButton: true,  // false | true (default)
  mlSettings: {
    initialView: {
      searchQuery: '(name : "pexel")'
    },
  }
};
```

![](../../.gitbook/assets/10-mlw-custom-search.png)

##### **Open all collections**

The following config will show view with all collections

```javascript
const config = {
  container: '#container',   // the element in which the widget will be rendered
  className: 'media-library-widget',
  dimensions: {
    height: '100%',
    width: '100%',
  },
  view: 'modal',  // modal (default) | inline
  renderOpenButton: true,  // false | true (default)
  mlSettings: {
      collection: {}
  }
};

```

![](../../.gitbook/assets/13-mlw-all-collections.png)

##### **Open specific collection**

The following config will open collection with specified ID
```javascript
const config = {
  container: '#container',   // the element in which the widget will be rendered
  className: 'media-library-widget',
  dimensions: {
    height: '100%',
    width: '100%',
  },
  view: 'modal',  // modal (default) | inline
  renderOpenButton: true,  // false | true (default)
  mlSettings: {
      collection: {
        id: "5fd874c040308546019f0500"
      }
  }
};
```
![](../../.gitbook/assets/12-mlw-collection.png)

##### **Allow single file selection**

The following config will allow user to select only single asset
```javascript
const config = {
  container: '#container',   // the element in which the widget will be rendered
  className: 'media-library-widget',
  dimensions: {
    height: '100%',
    width: '100%',
  },
  view: 'modal',  // modal (default) | inline
  renderOpenButton: true,  // false | true (default)
  mlSettings: {
    multiple: false
  }
};
```

![](../../.gitbook/assets/11-mlw-single-selection.png)

##### **Specify max number of files to be selected**

The following config will allow user to select only single asset
```javascript
const config = {
  container: '#container',   // the element in which the widget will be rendered
  className: 'media-library-widget',
  dimensions: {
    height: '100%',
    width: '100%',
  },
  view: 'modal',  // modal (default) | inline
  renderOpenButton: true,  // false | true (default)
  mlSettings: {
    multiple: true,
    maxFiles: 3
  }
};
```

![](../../.gitbook/assets/08-mlw-max-selection.png)

#### Callback function and payload

This callback function is called after the user clicks "Insert" button in the Media library. The callback receives a JSON payload of the selected images. It is up to you how you consume this data in your application.

```javascript
function callback (payload) {
  // this is the callback handler
  // … consume json payload …
}
```

#### Sample payload data:

The following shows an example of the JSON payload returned after selecting and inserting an image from the Media Library Widget UI.

```javascript
{
  eventType: 'INSERT',
  data: [{
    createdAt: "2020-12-15T08:33:04.570Z",
    customCoordinates: null,
    fileId: "5fd874c040308546019f0500",
    filePath: "/rally_s_tK613HYyf.jpg",
    fileType: "image",
    isPrivateFile: false,
    name: "rally_s_tK613HYyf.jpg",
    tags: null,
    thumbnail: "https://ik.imagekit.io/o00s3beva/tr:n-media_library_thumbnail/rally_s_tK613HYyf.jpg",
    type: "file",
    url: "https://ik.imagekit.io/o00s3beva/rally_s_tK613HYyf.jpg"
  }]
}
```

### 3. Run the application

Navigate to your application webpage in the browser. You should see the following button:

![Open Media Library button](../../.gitbook/assets/02-mlw-button.png)

Upon clicking it, if you are already logged in to ImageKit on this browser, the Media Library view should open up directly. Otherwise, the login screen will be displayed.

![ImageKit login screen in modal popup](../../.gitbook/assets/03-mlw-login.png)

Login to ImageKit with your email and password.

Upon logging in successfully, you should automatically be routed to the Media Library view.

![Media Library Widget panel in modal popup](../../.gitbook/assets/15-mlw-home.png)

### 4. Insert images from the Media Library via the plugin

The Media Library Widget looks and works similar to the ImageKit dashboard. An additional feature is the ability to insert selected images or files from the plugin.

In the upper right area of the screen, an "Insert" button should be present. Click on this to trigger the image selection handling callback on your web app page.

![Insert images](../../.gitbook/assets/05-mlw-insert-image.png)

The modal view should close automatically. Open the browser console and verify that the image payload data has been logged successfully:

![Data payload received successfully](../../.gitbook/assets/06-mlw-response.png)

## Sample application

{% tabs %}
{% tab title="HTML" %}
{% code title="index.html" %}
```markup
<!DOCTYPE html>
<html>

<head>
  <title>ImageKit Media Library Widget</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="styles.css">
</head>

<body>
  <!-- Media Library -->
  <div class="wrapper">
    <div id="container"></div>
  </div>
</body>

<script src="https://unpkg.com/imagekit-media-library-widget/dist/imagekit-media-library-widget.min.js"></script>

<script>
// configuration options
const config = {
  container: '#container',   // the element in which the widget will be rendered
  className: 'media-library-widget',
  dimensions: {
    height: '100%',
    width: '100%',
  },
  view: 'modal',  // modal | inline
  renderOpenButton: true  // false | true (default)
};

// define callback handler  
function callback (payload) {
  // this is the callback handler
  // … consume json payload …
  console.log('Image data:', payload.data);
}

// instantiate the plugin
const mediaLibraryWidget = new IKMediaLibraryWidget(config, callback);
</script>

</html>
```
{% endcode %}
{% endtab %}

{% tab title="Styles" %}
{% code title="styles.css" %}
```css
html {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
}

body {
  margin: 0;
  font-family: Arial, Helvetica, sans-serif;
}

#container {
  position: relative;
  height: 100%;
  width: 100%;
}

.wrapper {
  width: 100%;
  height: 100%;
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Note on using Google Chrome in Incognito mode

In order to use this plugin on Google Chrome in Incognito mode, you need to enable third-party cookies:

![](../../.gitbook/assets/07-mlw-incognito-sm.png)
