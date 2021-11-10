# Custom metadata fields API

Imagekit.io allows you to define a schema for your metadata keys and the value filled against that key will have to adhere to those rules. You can [create](create-custom-metadata-field.md), [read](get-custom-metadata-field.md) and [update](update-custom-metadata-field.md) custom metadata rules and update your file with custom metadata value in [file update API](../media-api/update-file-details.md) or [file upload API](../upload-file-api/).

## Some real-life applications

Custom metadata fields can be used to store any type of data about your images in key-value pairs.

For example, if you store product images in the media library, you can add custom metadata such as **product** **SKU**, **price**, **brand**, **discount, description,** etc. &#x20;

```javascript
{
    "SKU": "VS882HJ2JD",
    "price": 599.99,
    "brand": "H&M",
    "discount": 30,
    "description": "Black casual shirt, has a spread collar, a full button placket, long sleeves with roll-up button tabs, a patch pocket and a curved hem."
}
```

### Custom metadata fields schema

You can create schemas for the above custom metadata fields.

**SKU**

```javascript
{
    "name": "SKU",
    "schema": {
        "type": "Text"
    }
}
```

This type of schema will allow string values** **in `SKU` custom metadata field, like `"VS882HJ2JD"`, `"2JD93NMS2G"` etc.&#x20;

Invalid values of other data types like number (e.g.`5682342234`), boolean (e.g. `true`) etc. will be rejected with an error.

**Price**

```javascript
{
    "name": "price",
    "schema": {
        "type": "Number",
        "minValue": 1000,
        "maxValue": 3000
    }
}
```

This schema will allow numerical values within the specified range in `price` field, like `1000`, `1299`, `2459.59`, `3000` etc.

Invalid values of other data types like text (e.g.`"one thousand three"`), boolean (e.g. `true`), or numbers below range (e.g. `900`) and above range (e.g. `3001`) etc. will be rejected with an error.

**Size**

```javascript
{
    "name": "size",
    "schema": {
        "type": "SingleSelect",
        "selectOptions": ["XS", "S", "M", "L", "XL", "XXL"],
        "isValueRequired": true,
        "defaultValue": "M"
    }
}
```

This schema will allow only one of the values listed in `selectOptions` in `size` field, i.e. `S`, `L`, `XXL` etc. It is a required field with the default value `M` that will be applied on all files if none is provided.\
\
Invalid values i.e. values that aren't present in `selectOptions` like `XXS`, `XXXL` etc will be rejected with an error.

**Description**

```javascript
{
    "name": "description",
    "schema": {
        "type": "Textarea",
        "minLength": 100,
        "maxLength": 500
    }
}
```

This schema will allow valid text values within the specified length in `decription` custom metadata field key, e.g.&#x20;

"_Black casual shirt, has a spread collar, a full button placket, long sleeves with roll-up button tabs, a patch pocket, and a curved hem._"

Invalid values of other data types like number (e.g.`675`), boolean (e.g. `true`), or text input below or above the length requirement (e.g. _string with less than 100 characters_) will be rejected with an error.&#x20;

**Brand**

```javascript
{
    "name": "brand",
    "schema": {
        "type": "Text"
    }
}
```

This type of schema will allow string values like `H&M`, `Nike` etc. in `brand` field.&#x20;

Invalid values of other data types like number (e.g.`8738`), boolean (e.g. `true`) etc. will be rejected with an error.

**Discount**

```javascript
{
    "name": "discount",
    "schema": {
        "type": "Number",
        "minValue": 0,
        "maxValue": 60,
        "defaultValue": 20
    }
}
```

This schema will allow numerical values within the specified range in `discount` field e.g. `0`, `10`, `22.5`, `60` etc. If no value is supplied, it will set `discount` to `20` by default.&#x20;

Invalid values of other data types like text (e.g.`"fifty percent"`), boolean (e.g. `true`), or numbers below range (e.g. `-40`) and above range (e.g. `70`) etc. will be rejected with an error.\
\
After [creating these custom metadata fields](create-custom-metadata-field.md) using the API, you can add values for these respective keys in your files using [update file API](../media-api/update-file-details.md) or during [file upload](../../media-library/overview/upload-files.md).&#x20;

### Allowed values for custom metadata field type in schema

Allowed values - `Text`, `Textarea`, `Number`, `Date`, `Boolean`, `SingleSelect`,  `MultiSelect`

| Field type     | Description                                                                      | Examples                                                                                                                                    |
| -------------- | -------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `Text`         | Accepts `string` values, meant to be used for short strings.                     | `Nike`, `Red cashmere sweater`                                                                                                              |
| `Textarea`     | Accepts `string` values, meant to be used for longer strings, like descriptions. | _"Black casual shirt, has a spread collar, a full button placket, long sleeves with roll-up button tabs, a patch pocket and a curved hem."_ |
| `Number`       | Accepts numerical values including negative numbers and decimals.                | `100`, `-10`, `0.09`, `7000`                                                                                                                |
| `Date`         | Accepts ISO8601 string Date values.                                              | `2010-11-05T05:06:07`                                                                                                                       |
| `Boolean`      | Accepts `boolean` values.                                                        | `true`, `false`                                                                                                                             |
| `SingleSelect` | Accepts an `array` of `string`, `number` or `boolean` values                     | `["S", "M", 30, 40, false]`                                                                                                                 |
| `MultiSelect`  | Accepts an `array` of `string`, `number` or `boolean` values                     | `["L", "XL", 60, 90, true]`                                                                                                                 |

{% hint style="info" %}
All the possible values of `schema`object can be found [here](create-custom-metadata-field.md#allowed-values-in-the-schema-object).
{% endhint %}
