# Product Data Imports

There are two methods for importing Product Data into ChannelApe:

1. CSV Upload
1. API

## CSV Upload

The CSV Upload is by far the esaiest method and as a bonus our standard CSV template closely matches Shopify's Product Export.

### Data Requirements

The following fields are required to use the CSV Uploader.  If you do not have a Product Name you can may use the Variant SKU, but we strongly encourage using the Product Name as it will improve the experience for users in our web app.

- Variant SKU
- Product Name
- Active

### Limitations

Product catalogs with more than 10,000 rows will need to be split into multiple uploads.

### Examples

These examples already include the Unit of Measure and may be used as a template for further additions.

- [product-data-template-part-1.csv](product-data-template-part-1.csv)
- [product-data-template-part-2.csv](product-data-template-part-2.csv)

### Instructions

1. Login[^1] to [appx.channelape.io](https://appx.channelape.io)
1. Take note of the business selector in the top right, the product data that you will be uploading will be associated with the business that is selected.
1. Navigate to Product Upload
1. Drag & drop the file onto the upload screen or click on the box to browse for the file on your computer
1. Click Next
1. Select the header row with the first row selected by default.
1. Click Next
1. Map the columns from the CSV file to the product data attributes on the right.  If you're using the standard template this should map 1:1.
1. Review the mappings and click Next.
1. Fix any errors reported after validation and once all the errors are resolved click on submit.
1. Wait for the data to be uploaded.

[^1]: You can authenticate through Google SSO and will be able to see the businesses that you have access to.

## API

API Documentation is coming soon.