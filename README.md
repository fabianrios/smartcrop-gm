# smartcrop-gm

This is an adapter module for using [smartcrop.js](https://github.com/jwagner/smartcrop.js)
with node.js using [gm](https://github.com/aheckmann/gm) for image decoding.

It requires image magick to be installed and available in `$PATH` to function.

## Installation
```
npm install --save smartcrop-gm
```

## API

## crop(image, options)

**Image:** string (path to file) or buffer

**Options:** options object to be passed to smartcrop

**returns:** A promise for a cropResult

## Example

```javascript
var request = require('request');
var gm = require('gm').subClass({imageMagick: true});
var smartcrop = require('smartcrop-gm');

function applySmartCrop(src, dest, width, height) {
  request(src, {encoding: null}, function process(error, response, body){
    if (error) return console.error(error);
    smartcrop.crop(body, {width: width, height: height}).then(function(result) {
      var crop = result.topCrop;
      gm(body)
        .crop(crop.width, crop.height, crop.x, crop.y)
        .resize(width, height)
        .write(dest, function(error){
            if (error) return console.error(error);
        });
    });
  });
}

var src = 'https://raw.githubusercontent.com/jwagner/smartcrop-gm/master/test/flower.jpg';
applySmartCrop(src, 'flower-square.jpg', 128, 128);

```
