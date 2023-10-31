# Media API

ImageKit.io allows you to manage media assets programmatically. All the functionality exposed in the ImageKit dashboard is available via APIs. You can integrate these APIs in your CMS to achieve any integration you can imagine.

* [Listing and searching assets](list-and-search-files.md) - Search existing assets in the Media library by the combination of multiple asset attributes e.g., name, location, associated tags, embedded and custom metadata associated with the asset.
* [Updating asset](update-file-details.md) - Updating tags, adding custom coordinates for cropping, applying background removal extension, updating tags using machine learning, or updating associated custom metadata with the asset.
* [Getting individual file details](get-file-details.md). - Get details of an individual current file.
* [Getting individual file version details](get-file-version-details.md). - Get details of an individual file version.
* [Get file versions](get-file-versions.md). - Get details of all versions of a file.
* [Deleting assets](delete-file.md).
* [Delete assets in bulk](delete-files-bulk.md).
* [Delete file version](delete-file-version.md). - Delete a non-current version of a file.
* [Restore file version](restore-file-version.md). - Restore to a non-current version of a file.
* [Purging cache](purge-cache.md) on CDN.
* [Getting image metadata](../metadata-api/get-image-metadata-for-uploaded-media-files.md) for an image file.
* Bulk [add](add-tags-bulk.md) or [remove](remove-tags-bulk.md) user added tags.
* Bulk [remove](remove-aitags-bulk.md) AI tags.
* [Copy](copy-file.md), [move](move-file.md) and [rename](rename-file.md) assets.
* [Create](create-folder.md), [copy](copy-folder.md), [move](move-folder.md), and [delete](delete-folder.md) folder.
* [Get bulk job status](copy-move-folder-status.md) for copy and move folder jobs.

All media APIs accept JSON-encoded request bodies and return JSON-encoded responses.

## File object structure

```javascript
{
    "type": "file",
    "name": "sample-car.jpeg",
    "createdAt": "2021-12-11T00:58:39.685Z",
    "updatedAt": "2022-05-19T10:57:07.511Z",
    "fileId": "61b3f7bf75889b0b19970117",
    "tags": [
        "luxury"
    ],
    "AITags": [
        {
            "name": "Wheel",
            "confidence": 95.44,
            "source": "google-auto-tagging"
        },
        {
            "name": "Tire",
            "confidence": 94.22,
            "source": "google-auto-tagging"
        },
        {
            "name": "Vehicle",
            "confidence": 93.95,
            "source": "google-auto-tagging"
        },
        {
            "name": "Hood",
            "confidence": 91.83,
            "source": "google-auto-tagging"
        },
        {
            "name": "Grille",
            "confidence": 91.77,
            "source": "google-auto-tagging"
        },
        {
            "name": "Car",
            "confidence": 99.87,
            "source": "aws-auto-tagging"
        },
        {
            "name": "Tire",
            "confidence": 98.93,
            "source": "aws-auto-tagging"
        },
        {
            "name": "Sports Car",
            "confidence": 98.85,
            "source": "aws-auto-tagging"
        },
        {
            "name": "Wheel",
            "confidence": 98.46,
            "source": "aws-auto-tagging"
        },
        {
            "name": "Coupe",
            "confidence": 97.82,
            "source": "aws-auto-tagging"
        }
    ],
    "versionInfo": {
        "id": "61b3f7bf75889b0b19970117",
        "name": "Version 1"
    },
    "embeddedMetadata": null,
    "customCoordinates": null,
    "customMetadata": {},
    "isPrivateFile": false,
    "url": "https://ik.imagekit.io/demo/sample-car.jpeg",
    "thumbnail": "https://ik.imagekit.io/demo/tr:n-ik_ml_thumbnail/sample-car.jpeg",
    "fileType": "image",
    "filePath": "/sample-car.jpeg",
    "height": 638,
    "width": 1342,
    "size": 1451076,
    "hasAlpha": true,
    "mime": "image/png"
}
```

| Property name     | Description                                                                                                                                                                                                                             |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| fileId            | The unique fileId of the uploaded file. All versions of a file will have the same <code>fileId</code> associated with it.                                                                                                               |
| type              | Type of item. It can be `file` or `folder`.                                                                                                                                                                                                        |
| name              | Name of the file.                                                                                                                                                                                                                       |
| filePath          | The relative path of the file. In the case of an image, you can use this path to construct different transformations.                                                                                                                   |
| tags              | The array of tags associated with the image. If no tags are set, it will be `null`.                                                                                                                                                     |
| AITags            | Array of `AITags` associated with the image. If no `AITags` are set, it will be `null`. These tags can be added using the `google-auto-tagging` or `aws-auto-tagging` [extensions](../../extensions/overview/ai-based-auto-tagging.md). |
| versionInfo       | An object containing the file or file version's <code>id</code> (versionId) and <code>name</code>.                                                                                                                                      |
| isPrivateFile     | Is the file marked as private. It can be either `true` or `false`.                                                                                                                                                                      |
| customCoordinates | <p>Value of custom coordinates associated with the image in the format <code>x,y,width,height</code>. If customCoordinates are not defined, then it is <code>null</code>.<br></p>                                                       |
| url               | A publicly accessible URL of the file.                                                                                                                                                                                                  |
| thumbnail         | In the case of an image, a small thumbnail URL.                                                                                                                                                                                         |
| fileType          | The type of file could be either `image` or `non-image`.                                                                                                                                                                                |
| mime              | MIME Type of the file. For example - `image/jpeg`                                                                                                                                                                                       |
| height            | Height of the media file in pixels                                                                                                                                                                                                      |
| width             | Width of the media file in pixels                                                                                                                                                                                                       |
| size              | Size of the file in Bytes                                                                                                                                                                                                               |
| bitRate           | Bitrate of the media file (Only for videos)                                                                                                                                                                                             |
| videoCodec        | Video codec of the first stream for the media file (Only for videos)                                                                                                                                                                    |
| audioCodec        | Audio codec of the first stream for the media file (Only for videos)                                                                                                                                                                    |
| duration          | Duration of the media file content in seconds (Only for videos)                                                                                                                                                                         |
| hasAlpha          | A boolean indicating if the image has an alpha layer or not.                                                                                                                                                                            |
| customMetadata    | A key-value data associated with the asset. Before setting any custom metadata on an asset, you have to create the field using [custom metadata fields API](../custom-metadata-fields-api/).                                            |
| embeddedMetadata  | Consolidated embedded metadata associated with the file. It includes `exif`, `iptc`, and `xmp` data.                                                                                                                                    |          
| createdAt         | The date and time when the file was first uploaded. The format is `YYYY-MM-DDTHH:mm:ss.sssZ`                                                                                                                                            |
| updatedAt         | The date and time when the file was last updated. The format is `YYYY-MM-DDTHH:mm:ss.sssZ`                                                                                                                                              |

## Embedded metadata object structure

Below is the embedded metadata stored in this [sample image](https://ik.imagekit.io/ikmedia/IPTC-PhotometadataRef-Std2019.1_8O33PO0Jjss.jpg?tr=md-true) provided by https://iptc.org.

```javascript
{
    ExifVersion: '0232',
    ImageDescription: 'The description aka caption (ref2019.1)',
    XResolution: 72,
    YResolution: 72,
    ResolutionUnit: 'inches',
    Artist: 'Creator1 (ref2019.1)',
    Copyright: 'Copyright (Notice) 2019.1 IPTC - www.iptc.org  (ref2019.1)',
    DateTimeOriginal: new Date('2019-10-16T19:01:03.000Z'),
    OffsetTimeOriginal: '+00:00',
    ComponentsConfiguration: 'Y,Cb,Cr,-',
    FlashpixVersion: '0100',
    ColorSpace: 'Uncalibrated',
    ObjectAttributeReference: 'A Genre (ref2019.1)',
    ObjectName: 'The Title (ref2019.1)',
    SubjectReference: ['IPTC:1ref2019.1', 'IPTC:2ref2019.1', 'IPTC:3ref2019.1'],
    Keywords: ['Keyword1ref2019.1', 'Keyword2ref2019.1', 'Keyword3ref2019.1'],
    SpecialInstructions: 'An Instruction (ref2019.1)',
    TimeCreated: '19:01:03+00:00',
    Byline: 'Creator1 (ref2019.1)',
    BylineTitle: "Creator's Job Title  (ref2019.1)",
    Sublocation: 'Sublocation (Core) (ref2019.1)',
    ProvinceState: 'Province/State(Core)(ref2019.1)',
    CountryPrimaryLocationCode: 'R19',
    CountryPrimaryLocationName: 'Country (Core) (ref2019.1)',
    OriginalTransmissionReference: 'Job Id (ref2019.1)',
    CopyrightNotice: 'Copyright (Notice) 2019.1 IPTC - www.iptc.org  (ref2019.1)',
    CaptionAbstract: 'The description aka caption (ref2019.1)',
    WriterEditor: 'Description Writer (ref2019.1)',
    ApplicationRecordVersion: 4,
    CountryCode: 'R19',
    CreatorCity: "Creator's CI: City (ref2019.1)",
    CreatorCountry: "Creator's CI: Country (ref2019.1)",
    CreatorAddress: "Creator's CI: Address, line 1 (ref2019.1)",
    CreatorPostalCode: "Creator's CI: Postcode (ref2019.1)",
    CreatorRegion: "Creator's CI: State/Province (ref2019.1)",
    CreatorWorkEmail: "Creator's CI: Email@1, Email@2 (ref2019.1)",
    CreatorWorkTelephone: "Creator's CI: Phone # 1, Phone # 2 (ref2019.1)",
    CreatorWorkURL: 'http://www.Creators.CI/WebAddress/ref2019.1',
    IntellectualGenre: 'A Genre (ref2019.1)',
    Location: 'Sublocation (Core) (ref2019.1)',
    Scene: ['IPTC-Scene-Code1 (ref2019.1)', 'IPTC-Scene-Code2 (ref2019.1)'],
    SubjectCode: ['IPTC:1ref2019.1', 'IPTC:2ref2019.1', 'IPTC:3ref2019.1'],
    AboutCvTermCvId: 'http://example.com/cv/about/ref2019.1',
    AboutCvTermId: 'http://example.com/cv/about/ref2019.1/code987',
    AboutCvTermName: 'CV-Term Name 1 (ref2019.1)',
    AboutCvTermRefinedAbout: 'http://example.com/cv/refinements2/ref2019.1/codeX145',
    AdditionalModelInformation: 'Additional Model Info (ref2019.1)',
    ArtworkCircaDateCreated: 'AO Circa Date: between 1550 and 1600 (ref2019.1)',
    ArtworkContentDescription: 'AO Content Description 1 (ref2019.1)',
    ArtworkContributionDescription: 'AO Contribution Description 1 (ref2019.1)',
    ArtworkCopyrightNotice: 'AO Copyright Notice 1 (ref2019.1)',
    ArtworkCreator: [
      'AO Creator Name 1a (ref2019.1)',
      'AO Creator Name 1b (ref2019.1)'
    ],
    ArtworkCreatorID: ['AO Creator Id 1a (ref2019.1)', 'AO Creator Id 1b (ref2019.1)'],
    ArtworkCopyrightOwnerID: 'AO Current Copyright Owner ID 1 (ref2019.1)',
    ArtworkCopyrightOwnerName: 'AO Current Copyright Owner Name 1 (ref2019.1)',
    ArtworkLicensorID: 'AO Current Licensor ID 1 (ref2019.1)',
    ArtworkLicensorName: 'AO Current Licensor Name 1 (ref2019.1)',
    ArtworkDateCreated: new Date('1919-10-16T19:01:00.000Z'),
    ArtworkPhysicalDescription: 'AO Physical Description 1 (ref2019.1)',
    ArtworkSource: 'AO Source 1 (ref2019.1)',
    ArtworkSourceInventoryNo: 'AO Source Inventory No 1 (ref2019.1)',
    ArtworkSourceInvURL: 'AO Source Inventory URL (ref2019.1)',
    ArtworkStylePeriod: [
      'AO Style Baroque (ref2019.1)',
      'AO Style Italian Baroque (ref2019.1)'
    ],
    ArtworkTitle: 'AO Title 1 (ref2019.1)',
    DigitalImageGUID: 'http://example.com/imageGUIDs/TestGUID12345/ref2019.1',
    DigitalSourceType: 'http://cv.iptc.org/newscodes/digitalsourcetype/softwareImage',
    EmbeddedEncodedRightsExpr: 'The Encoded Rights Expression (ref2019.1)',
    EmbeddedEncodedRightsExprType: 'IANA Media Type of ERE (ref2019.1)',
    EmbeddedEncodedRightsExprLangID: 'http://example.org/RELids/id4711/ref2019.1',
    Event: 'An Event (ref2019.1)',
    GenreCvId: 'http://example.com/cv/genre/ref2019.1',
    GenreCvTermId: 'http://example.com/cv/genre/ref2019.1/code1369',
    GenreCvTermName: 'Genre CV-Term Name 1 (ref2019.1)',
    GenreCvTermRefinedAbout: 'http://example.com/cv/genrerefinements2/ref2019.1/codeY864',
    ImageRegionName: ['Listener 1', 'Listener 2', 'Speaker 1'],
    ImageRegionOrganisationInImageName: [
      'Organisation name no 1 in region persltr2 (ref2019.1)',
      'Organisation name no 1 in region persltr2 (ref2019.1)',
      'Organisation name no 1 in region persltr3 (ref2019.1)'
    ],
    ImageRegionPersonInImage: [
      'Person name no 1 in region persltr2 (ref2019.1)',
      'Person name no 1 in region persltr3 (ref2019.1)',
      'Person name no 1 in region persltr1 (ref2019.1)'
    ],
    ImageRegionBoundaryH: [0.385],
    ImageRegionBoundaryShape: ['rectangle', 'circle', 'polygon'],
    ImageRegionBoundaryUnit: ['relative', 'relative', 'relative'],
    ImageRegionBoundaryW: [0.127],
    ImageRegionBoundaryX: [0.31, 0.59],
    ImageRegionBoundaryY: [0.18, 0.426],
    ImageRegionCtypeName: [
      'Region Boundary Content Type Name (ref2019.1)',
      'Region Boundary Content Type Name (ref2019.1)',
      'Region Boundary Content Type Name (ref2019.1)'
    ],
    ImageRegionCtypeIdentifier: [
      'https://example.org/rctype/type2019.1a',
      'https://example.org/rctype/type2019.1b',
      'https://example.org/rctype/type2019.1a',
      'https://example.org/rctype/type2019.1b',
      'https://example.org/rctype/type2019.1a',
      'https://example.org/rctype/type2019.1b'
    ],
    ImageRegionID: ['persltr2', 'persltr3', 'persltr1'],
    ImageRegionRoleName: [
      'Region Boundary Content Role Name (ref2019.1)',
      'Region Boundary Content Role Name (ref2019.1)',
      'Region Boundary Content Role Name (ref2019.1)'
    ],
    ImageRegionRoleIdentifier: [
      'https://example.org/rrole/role2019.1a',
      'https://example.org/rrole/role2019.1b',
      'https://example.org/rrole/role2019.1a',
      'https://example.org/rrole/role2019.1b',
      'https://example.org/rrole/role2019.1a',
      'https://example.org/rrole/role2019.1b'
    ],
    ImageRegionBoundaryRx: [0.068],
    ImageRegionBoundaryVerticesX: [0.05, 0.148, 0.375],
    ImageRegionBoundaryVerticesY: [0.713, 0.041, 0.863],
    LinkedEncodedRightsExpr: 'http://example.org/linkedrightsexpression/id986/ref2019.1',
    LinkedEncodedRightsExprType: 'IANA Media Type of ERE (ref2019.1)',
    LinkedEncodedRightsExprLangID: 'http://example.org/RELids/id4712/ref2019.1',
    LocationCreatedCity: 'City (Location created1) (ref2019.1)',
    LocationCreatedCountryCode: 'R17',
    LocationCreatedCountryName: 'CountryName (Location created1) (ref2019.1)',
    LocationCreatedLocationId: 'Location Id (Location created1) (ref2019.1)',
    LocationCreatedLocationName: 'Location Name (Location created1) (ref2019.1)',
    LocationCreatedProvinceState: 'Province/State (Location created1) (ref2019.1)',
    LocationCreatedSublocation: 'Sublocation (Location created1) (ref2019.1)',
    LocationCreatedWorldRegion: 'Worldregion (Location created1) (ref2019.1)',
    LocationCreatedGPSAltitude: '480 m',
    LocationCreatedGPSLatitude: '48,16.5N',
    LocationCreatedGPSLongitude: '16,20.28E',
    LocationShownCity: [
      'City (Location shown1) (ref2019.1)',
      'City (Location shown2) (ref2019.1)'
    ],
    LocationShownCountryCode: ['R17', 'R17'],
    LocationShownCountryName: [
      'CountryName (Location shown1) (ref2019.1)',
      'CountryName (Location shown2) (ref2019.1)'
    ],
    LocationShownLocationId: [
      'Location Id 1a(Location shown1) (ref2019.1)',
      'Location Id 1b(Location shown1) (ref2019.1)',
      'Location Id 2a(Location shown2) (ref2019.1)',
      'Location Id 2b(Location shown2) (ref2019.1)'
    ],
    LocationShownLocationName: [
      'Location Name (Location shown1) (ref2019.1)',
      'Location Name (Location shown2) (ref2019.1)'
    ],
    LocationShownProvinceState: [
      'Province/State (Location shown1) (ref2019.1)',
      'Province/State (Location shown2) (ref2019.1)'
    ],
    LocationShownSublocation: [
      'Sublocation (Location shown1) (ref2019.1)',
      'Sublocation (Location shown2) (ref2019.1)'
    ],
    LocationShownWorldRegion: [
      'Worldregion (Location shown1) (ref2019.1)',
      'Worldregion (Location shown2) (ref2019.1)'
    ],
    LocationShownGPSAltitude: ['140 m', '120 m'],
    LocationShownGPSLatitude: ['48,8.82N', '47,57.12N'],
    LocationShownGPSLongitude: ['17,5.88E', '16,49.8E'],
    MaxAvailHeight: 20,
    MaxAvailWidth: 19,
    ModelAge: [25, 27, 30],
    OrganisationInImageCode: [
      'Organisation Code 1 (ref2019.1)',
      'Organisation Code 2 (ref2019.1)',
      'Organisation Code 3 (ref2019.1)'
    ],
    OrganisationInImageName: [
      'Organisation Name 1 (ref2019.1)',
      'Organisation Name 2 (ref2019.1)',
      'Organisation Name 3 (ref2019.1)'
    ],
    PersonInImage: ['Person Shown 1 (ref2019.1)', 'Person Shown 2 (ref2019.1)'],
    PersonInImageCvTermCvId: ['http://example.com/cv/test99/ref2019.1'],
    PersonInImageCvTermId: ['http://example.com/cv/test99/code987/ref2019.1'],
    PersonInImageCvTermName: ['Person Characteristic Name 1 (ref2019.1)'],
    PersonInImageCvTermRefinedAbout: ['http://example.com/cv/refinements987/codeY765/ref2019.1'],
    PersonInImageDescription: ['Person Description 1 (ref2019.1)'],
    PersonInImageId: [
      'http://wikidata.org/item/Q123456789/ref2019.1',
      'http://freebase.com/m/987654321/ref2019.1'
    ],
    PersonInImageName: ['Person Name 1 (ref2019.1)'],
    ProductInImageDescription: ['Product Description 1 (ref2019.1)'],
    ProductInImageGTIN: [123456782019.1],
    ProductInImageName: ['Product Name 1 (ref2019.1)'],
    RegistryEntryRole: [
      'Registry Entry Role ID 1 (ref2019.1)',
      'Registry Entry Role ID 2 (ref2019.1)'
    ],
    RegistryItemID: [
      'Registry Image ID 1 (ref2019.1)',
      'Registry Image ID 2 (ref2019.1)'
    ],
    RegistryOrganisationID: [
      'Registry Organisation ID 1 (ref2019.1)',
      'Registry Organisation ID 2 (ref2019.1)'
    ],
    Creator: 'Creator1 (ref2019.1)',
    Description: 'The description aka caption (ref2019.1)',
    Rights: 'Copyright (Notice) 2019.1 IPTC - www.iptc.org  (ref2019.1)',
    Subject: ['Keyword1ref2019.1', 'Keyword2ref2019.1', 'Keyword3ref2019.1'],
    Title: 'The Title (ref2019.1)',
    AuthorsPosition: 'Creator&#39;s Job Title  (ref2019.1)',
    CaptionWriter: 'Description Writer (ref2019.1)',
    City: 'City (Core) (ref2019.1)',
    Country: 'Country (Core) (ref2019.1)',
    Credit: 'Credit Line (ref2019.1)',
    DateCreated: new Date('2019-10-16T19:01:03.000Z'),
    Headline: 'The Headline (ref2019.1)',
    Instructions: 'An Instruction (ref2019.1)',
    Source: 'Source (ref2019.1)',
    State: 'Province/State(Core)(ref2019.1)',
    TransmissionReference: 'Job Id (ref2019.1)',
    CopyrightOwnerID: [
      'Copyright Owner Id 1 (ref2019.1)',
      'Copyright Owner Id 2 (ref2019.1)'
    ],
    CopyrightOwnerName: [
      'Copyright Owner Name 1 (ref2019.1)',
      'Copyright Owner Name 2 (ref2019.1)'
    ],
    ImageCreatorID: 'Image Creator Id 1 (ref2019.1)',
    ImageCreatorName: 'Image Creator Name 1 (ref2019.1)',
    ImageCreatorImageID: 'Image Creator Image ID (ref2019.1)',
    ImageSupplierID: 'Image Supplier Id (ref2019.1)',
    ImageSupplierName: 'Image Supplier Name (ref2019.1)',
    ImageSupplierImageID: 'Image Supplier Image ID (ref2019.1)',
    LicensorCity: ['Licensor City 1 (ref2019.1)', 'Licensor City 2 (ref2019.1)'],
    LicensorCountry: [
      'Licensor Country 1 (ref2019.1)',
      'Licensor Country 2 (ref2019.1)'
    ],
    LicensorEmail: ['Licensor Email 1 (ref2019.1)', 'Licensor Email 2 (ref2019.1)'],
    LicensorExtendedAddress: [
      'Licensor Ext Addr 1 (ref2019.1)',
      'Licensor Ext Addr 2 (ref2019.1)'
    ],
    LicensorID: ['Licensor ID 1 (ref2019.1)', 'Licensor ID 2 (ref2019.1)'],
    LicensorName: ['Licensor Name 1 (ref2019.1)', 'Licensor Name 2 (ref2019.1)'],
    LicensorPostalCode: [
      'Licensor Postcode 1 (ref2019.1)',
      'Licensor Postcode 2 (ref2019.1)'
    ],
    LicensorRegion: ['Licensor Region 1 (ref2019.1)', 'Licensor Region 2 (ref2019.1)'],
    LicensorStreetAddress: [
      'Licensor Street Addr 1 (ref2019.1)',
      'Licensor Street Addr 2 (ref2019.1)'
    ],
    LicensorTelephone1: ['Licensor Phone1 1 (ref2019.1)', 'Licensor Phone1 2 (ref2019.1)'],
    LicensorTelephone2: ['Licensor Phone2 1 (ref2019.1)', 'Licensor Phone2 2 (ref2019.1)'],
    LicensorURL: ['Licensor URL 1 (ref2019.1)', 'Licensor URL 2 (ref2019.1)'],
    ModelReleaseID: [
      'Model Release ID 1 (ref2019.1)',
      'Model Release ID 2 (ref2019.1)'
    ],
    PropertyReleaseID: [
      'Property Release ID 1 (ref2019.1)',
      'Property Release ID 2 (ref2019.1)'
    ],
    Rating: 1,
    UsageTerms: 'Rights Usage Terms (ref2019.1)',
    WebStatement: 'http://www.WebStatementOfRights.org/2019.1',
    DateTimeCreated: new Date('2019-10-16T19:01:03.000Z'),
    Caption: 'The description aka caption (ref2019.1)',
    Writer: 'Description Writer (ref2019.1)'
  }
```

