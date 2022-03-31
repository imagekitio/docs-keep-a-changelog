# URL Rewriters

After migrating to ImageKit.io from another service, ImageKit.io can translate your pre-existing transformation URLs written in the syntax of your previous service provider to ImageKit's syntax. This allows you to continue using your already written URLs with minimal changes.
For example, if you are migrating from Cloudinary, ImageKit can translate the following URL written in Cloudinary's syntax to ImageKit's syntax:

Cloudinary URL: `https://res.cloudinary.com/<cloudinary_cloud_name>/image/upload/h_300,w_300,c_crop/folder/image.jpg`<br>
ImageKit URL:  `https://ik.iamgekit.io/<your_imagekit_id>/tr:h-300,w-300,cm-extract/folder/image.jpg`

```markup
             static part                            transformations        file path
┌───────────────────────────────────────────┐┌─────────────────────────┐┌──────────────┐
https://res.cloudinary.com/<cloudinary_cloud_name>/image/upload/h_300,w_300,c_crop,g_north/folder/image.jpg
```

* The static part - `https://res.cloudinary.com/<cloudinary_cloud_name>/image/upload` must be manually replaced by you with `https://ik.iamgekit.io/<your_imagekit_id>`
* The transformations part - `h_300,w_300,c_crop,g_north` will be translated by ImageKit to produce `tr:h-300,w-300,cm-extract`
* The file path - `folder/medium_cafe_B1iTdD0C.jpg` will stay as it is unless your file structure has changed during migration

ImageKit.io currently supports rewriting URLs for the following services:

1. Cloudinary

## How to use a URL-rewriter

A URL-rewriter can be attached to a [URL-endpoint](https://docs.imagekit.io/integration/url-endpoints) in your account. For instance, If you attach the Cloudinary URL-rewriter to the endpoint with the `cl_rewrite` identifier, ImageKit will accept transformations written in Cloudinary's syntax for all URLs on this endpoint. An example URL that performs two chained tarnsformations might look like this `https://ik.iamgekit.io/<your_imagekit_id>/h_300,w_300,c_crop,g_north/a_50/folder/image.jpg`.

To attach a rewriter to one of your endpoints, please contact us at at support@imagekit.io.
