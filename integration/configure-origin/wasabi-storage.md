# Wasabi Storage

You can configure ImageKit.io to fetch images from your Wasabi Storage bucket. This allows you to start leveraging ImageKit.io real-time image resizing, optimization, and fast CDN delivery for thousands or millions of existing images within minutes.

Wasabi Storage is an [S3-compatible storage provider](https://docs.imagekit.io/integration/configure-origin/s3-compatible-external-storages), which means you can configure ImageKit to access it the same way it accesses an AWS S3 bucket. To configure your Wasabi Storage bucket with your ImageKit account, you will need Wasabi's endpoint parameter. This value generally depends upon the region in which your bucket belongs

Visit the official [Wasabi Documentation](https://wasabi-support.zendesk.com/hc/en-us/articles/360015106031-What-are-the-service-URLs-for-Wasabi-s-different-regions-) page on service URLs and find out the correct endpoint corresponding to the region in which your bucket is configured.

For example, buckets configured in the US West 1 region will have the endpoint: `https://s3.us-west-1.wasabisys.com` Similarly, find out the correct endpoint for the region that your bucket is configured in and verify with Wasabi's official documentation. 

Once you have this endpoint, follow the instructions on ImageKit's doc page on [How to configure S3-Compatible external storage with ImageKit.](https://docs.imagekit.io/integration/configure-origin/s3-compatible-external-storages)
