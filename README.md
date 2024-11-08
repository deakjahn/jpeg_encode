# JPEG encoder

[![pub package](https://img.shields.io/pub/v/jpeg_encode.svg)](https://pub.dev/packages/jpeg_encode)

Skia, the engine behind `dart:ui` has no support for JPEG, only PNG and raw RGBA. And the source
code comments make it clear that they don't intend to add any more formats, those should be left to
external packages.

While there are packages that offer full support for image manipulation, many a time we don't need
that, just to be able to convert a file to JPG for use outside of our app, maybe to send it to the web.
This package does that and nothing else. It's a minimalistic JPEG encoder that doesn't offer any extra
functionality but to create the simplest JPEG file. Not necessarily the best or most compressed one,
although it's fairly good, nonetheless.

The code is just a straight Dart port of an existing implementation of _Jon Olick_ from
https://www.jonolick.com/code.html.

## How to use

Let's suppose you have a `dart:ui` `Image` from somewhere, maybe you converted from an incoming image:

```dart
import "dart:ui" as ui;

Uint8List bytes;
final codec = await ui.instantiateImageCodec(bytes);
final frame = await codec.getNextFrame();
final image = frame.image;
```

And you want to convert it to JPG (`quality` is between 0 and 100, as usual):

```dart
final data = await image.toByteData(format: skia.ImageByteFormat.rawRgba);
final jpg = JpegEncoder().compress(data!.buffer.asUint8List(), image.width, image.height, 90);
```

In other words, the pixel array is expected to be in row major format (all columns of first row,
then of the second row, etc), four bytes for each pixel: R, G, B and A. Alpha is ignored but the byte
is expected to be there. Consequently, the length of the array has to be `width * height * 4`.

Before you ask, saving in Flutter is as simple as:

```dart
File(path).writeAsBytesSync(jpg); // or
await File(path).writeAsBytes(jpg);
```

# Support

If you like this package, please consider supporting it.

<a href="https://www.buymeacoffee.com/deakjahn" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Book" height="60" width="217"></a>