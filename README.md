**png-baker.js** is a small (1.3k minified and gzipped) library that
makes it easy to read and write custom textual data in PNG files from
the browser.

The file simply reads the chunks in a PNG file as per the
[PNG specification][]. It leaves all chunks untouched except for
`tEXt` chunks, which it parses and exposes via an API. Clients can
then manipulate this data and re-encode the baked PNG.

The library requires support for [Typed Arrays][] and the [File API][].
It has been verified to work on Internet Explorer 10, Chrome 30, Opera 12,
and Firefox 24.

## Usage

```javascript
// dot courtesy of http://en.wikipedia.org/wiki/Data_URI_scheme
var dot = "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCA" +
          "YAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxgljNBAAO9T" +
          "XL0Y4OHwAAAABJRU5ErkJggg==";

// The constructor can take either a Data URL or an ArrayBuffer.
var baker = new PNGBaker(dot);

// The textChunks object can be used to read/write information.
baker.textChunks['Description'] = 'Red dot';

// To write the result back out to a file, use the FileReader API.
var reader = new FileReader();
reader.onloadend = function() {
  var img = new Image();
  img.src = reader.result;
  document.body.appendChild(img);
};
reader.readAsDataURL(baker.toBlob());
```

## Limitations

The PNG specification recommends that `tEXt` chunks be encoded in
Latin-1. This library doesn't do that for you, though you're welcome to
do the encoding/decoding yourself.

## License

Public Domain [CC0 1.0 Universal][cczero].

  [Typed Arrays]: http://caniuse.com/#feat=typedarrays
  [File API]: http://caniuse.com/#feat=fileapi
  [PNG specification]: http://www.w3.org/TR/REC-png-multi.html
  [cczero]: http://creativecommons.org/publicdomain/zero/1.0/
