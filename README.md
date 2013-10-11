**png-baker.js** is a small (1.3k minified and gzipped) library that
makes it easy to read and write custom textual data in PNG files from
the browser.

The file simply reads the chunks in a PNG file as per the
[PNG specification][]. It leaves all chunks untouched except for
`tEXt` chunks, which it parses and exposes via an API. Clients can
then manipulate this data and re-encode the baked PNG.

The library requires support for [Typed Arrays][] and the [File API][].
It has been verified to work on Internet Explorer 10, Chrome 30, Opera 12,
Safari 6, and Firefox 24.

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

// To write the result back out to a file, use the toBlob() method.
var img = new Image();
img.src = URL.createObjectURL(baker.toBlob());
document.body.appendChild(img);
```

## Limitations

* Part of the reason this library is relatively small is because it
  completely ignores actual image data. Of course, this is also a
  limitation of the library, too. If you need such functionality, you
  may want to look at [png.js][].

* The API is fully synchronous and holds all PNG data in memory (i.e., it's
  not streaming). So you might not want to use this library with ridiculously
  large pictures.

* The PNG specification recommends that `tEXt` chunks be encoded in
  Latin-1. This library doesn't do that for you, though you're welcome to
  do the encoding/decoding yourself.

* When serializing to a PNG, the `tEXt` chunks are always inserted after
  the first `IHDR` chunk in no specific order. This means that reading
  a PNG and immediately re-serializing it doesn't necessarily result in
  the exact same binary data.

## License

Public Domain [CC0 1.0 Universal][cczero].

  [Typed Arrays]: http://caniuse.com/#feat=typedarrays
  [File API]: http://caniuse.com/#feat=fileapi
  [PNG specification]: http://www.w3.org/TR/REC-png-multi.html
  [png.js]: https://github.com/devongovett/png.js
  [cczero]: http://creativecommons.org/publicdomain/zero/1.0/
