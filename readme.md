### TiCloudinary

The easiest way to get started using Cloudinary in Appcelerator Titanium apps to upload, transform, and remove stored images via the Cloudinary API! Obviously you must signup for a Cloudinary account (no worries, they have a free plan). So once you have your Cloudinary API credentials, replace the dummy credentials with yours in Ti.Cloudinary.js

```
var cloudName = 'Your_Cloud_Name';
var apiKey = 999999999999;
var apiSecret = 'XXXXXXXXXX-XXXXX-XX';
```

Great, now that you're all setup...to begin using it, simply save or copy the TiCloudinary.js code I've provided here to your project's `lib` folder and then require it as a commonJS module in your project where you want to use it.

```
var TiCloudinary = require('/lib/TiCloudinary');
```

From here, you can upload images and start transforming them right away.  The `uploadAndTransform` function takes 3 arguments: `blob` - the TiBlob object of the image to be uploaded/transformed, `transformParams` - a list of supported query string parameters for performing transforms on the image uploaded (read more on supported parameters at http://cloudinary.com/documentation/image_transformations), and `callback` - the callback function called with an argument object containing the response `public_id` and transform url to call the Cloudinary API with to perform the transformation. An example call would look like this:

```
var transformSuccess = function(transformObj) {
  Ti.API.info('transformObj', JSON.stringify(transformObj)); // resulting in the url to call to perform a 134px x 134px, centered-by-face, circle-cropped PNG
};
var transformObj = TiCloudinary.uploadAndTransform(myBlob, 'w_134,h_134,c_fill,g_face,r_max', transformSuccess);
```

And lastly, when images no longer needed to be stored in Cloudinary, simply destroy them by calling the `deleteImage` function while passing it the `public_id` of the image you want to delete. An example call would look like this:

```
Ti.Cloudinary.deleteImage(transformObj.public_id);
```