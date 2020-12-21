# Digital Ocean Spaces

You can configure ImageKit.io to fetch images from your Digital Ocean Spaces bucket. This allows you to start leveraging ImageKit.io real-time image resizing, optimization, and fast CDN delivery for thousands or millions of existing images within minutes.

Digital Ocean Spaces is an [S3-compatible storage provider](https://docs.imagekit.io/integration/configure-origin/s3-compatible-external-storages), which means you can configure ImageKit to access it the same way it accesses an AWS S3 bucket. To configure your Digital Ocean Spaces bucket with your ImageKit account, you will need the `endpoint` parameter provided by Digital Ocean Spaces. This value generally depends upon the region in which your bucket belongs. 

As specified in the [Digital Ocean Spaces documentation](https://developers.digitalocean.com/documentation/spaces/), the general template to get this endpoint value is: `https://${REGION}.digitaloceanspaces.com` , where ${REGION} should be replaced by the region code for your region.

For example, buckets configured in the Singapore region have the endpoint: `https://sgp1.digitaloceanspaces.com`   
Similarly, find out the correct endpoint for the region that your bucket is configured in, and verify with Digital Ocean's official documentation. 

Once you have this endpoint, follow the instructions on ImageKit's doc page on [how to configure S3-Compatible external storage with ImageKit.](https://docs.imagekit.io/integration/configure-origin/s3-compatible-external-storages)

