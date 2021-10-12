# Alibaba Object Storage Service

You can configure ImageKit.io to fetch images from your Alibaba Object Storage bucket. This allows you to start leveraging ImageKit.io real-time image resizing, optimization, and fast CDN delivery for thousands or millions of existing images within minutes.

Alibaba Object Storage is an [S3-compatible storage provider](https://docs.imagekit.io/integration/configure-origin/s3-compatible-external-storages), which means you can configure ImageKit to access it the same way it accesses an AWS S3 bucket. To configure your Alibaba Object Storage bucket with your ImageKit account, you will need the `endpoint` parameter provided by Alibaba. This value generally depends upon the region in which your bucket belongs.

Visit the official [Alibaba Documentation](https://www.alibabacloud.com/help/doc-detail/31837.htm?spm=a2c63.l28256.a3.37.3a1f5139AomWDt) page on Regions and endpoints to find out the correct endpoint corresponding to the region in which your bucket is configured. We need the **Public endpoint** value from the table given on that page.

For example, buckets configured in the Singapore region will have the endpoint:\
`https://oss-ap-southeast-1.aliyuncs.com`\
Similarly, find out the correct endpoint for the region that your bucket is configured in and verify with Alibaba's official documentation. 

Once you have this endpoint, follow the instructions on ImageKit's doc page on [How to configure S3-Compatible external storage with ImageKit.](https://docs.imagekit.io/integration/configure-origin/s3-compatible-external-storages)\
