var moment = require('alloy/moment');
// Cloudinary credentials - **Replace these with your creds!!!
var cloudName = 'Your_Cloud_Name';
var apiKey = 999999999999;
var apiSecret = 'XXXXXXXXXX-XXXXX-XX';
// Image API URL base
var apiBaseUrl = 'http://api.cloudinary.com/v1_1/';

// Create and send alert on error
function alert(message, title, ok, callback) {
  a = Ti.UI.createAlertDialog({
    message: message,
    title: title || L('alert_title', 'Alert'),
    ok: ok || L('alert_ok', 'OK')
  });
  a.show();
  if (callback) {
    a.addEventListener('click', callback);
  }
}

exports = {
  /**
   * Upload image from a TiBlob object and return the transformation url
   *
   * @param blob {Object} - TiBlob object of the image to be uploaded/transformed
   * @param transformParams {String} - List of supported query string parameters for performing
   * transforms on the image uploaded - read more on supported parameters at
   * http://cloudinary.com/documentation/image_transformations
   * @param callback {Function} - Optional callback function, called with an argument object containing the
   * response `public_id` and transform url to call the Cloudinary API with to perform the transformation
   */
  uploadAndTransform: function(blob, transformParams, callback) {
    var nowUnix = moment().unix();
    var signature = Ti.Utils.sha1('timestamp=' + nowUnix + apiSecret);
    var url = apiBaseUrl + cloudName + '/image/upload';
    var client = Ti.Network.createHTTPClient({
      onload: function(e) {
        Ti.API.info('Upload Response', this.responseText);
        var response = JSON.parse(this.responseText);
        var transformImgUrl = 'http://res.cloudinary.com/' + cloudName + '/image/upload/' + transformParams + '/' + response.public_id + '.png';
        if (callback) {
          callback({
            public_id: response.public_id,
            transformImgUrl: transformImgUrl
          });
        }
      },
      onerror: function(e) {
        Ti.API.info('Cloudinary Upload Error', JSON.stringify(e));
        alert('Please verify your data connection and try again.', 'Uh Oh');
      },
      timeout: 4000
    });
    client.setRequestHeader('enctype', 'multipart/form-data');
    client.setRequestHeader('Content-Type', 'image/png');
    client.open('POST', url);

    var params = {
      api_key: apiKey,
      file: blob,
      //public_id: someUniqueName, // if added, make sure the signature sha1 starts with:'public_id=' + someUniqueName + '&timestamp=...'
      signature: signature,
      timestamp: nowUnix,
    };
    client.send(params);
  },

  /**
   * Delete uploaded image
   *
   * @param public_id {String} - `public_id` of the image to be deleted
   */
  deleteImage: function(public_id) {
    var url = apiBaseUrl + cloudName + '/resources/image/upload?public_ids[]=' + public_id;
    var client = Ti.Network.createHTTPClient({
      onload: function(e) {
        Ti.API.info('Delete Response', this.responseText);
      },
      onerror: function(e) {
        Ti.API.info('Cloudinary Delete Error', JSON.stringify(e));
      },
      timeout: 4000
    });
    client.setUsername(apiKey);
    client.setPassword(apiSecret);
    client.open('DELETE', url);
    client.send();
  }
};