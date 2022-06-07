# JPEG encoder

Skia, the engine behind `dart:ui` has no support for JPEG, only PNG and raw RGBA. And the source
code comments make it clear that they don't intend to add any more format, those should be left to
external packages.

While there are packages that offer full support for image manipulation, many times we don't need
that, just to be able to convert a file to JPG for use outside out app, maybe to send it to the web.
This package does that and nothin else. It's a simplistic JPEG encoder that doesn't offer any extra
functionality but to create the simplest JPEG file. Not necessarily the best or most compressed one,
although it's fairly good, nonetheless.

The code is just a straight Dart port of an existing implementation of _Jon Olick_ from
https://www.jonolick.com/code.html.

## How to use

Let's suppose you have a `dart:ui` `Image` from somewhere, maybe you converted from an incoming image:

```dart
import "dart:ui" as ui;

Uint8List pixels;
final codec = await ui.instantiateImageCodec(pixels);
final frame = await codec.getNextFrame();
final image = frame.image;
```

And you want to convert it to JPG (`quality` is between 0 and 100, as usual):

```dart
final data = await image.toByteData(format: skia.ImageByteFormat.rawRgba);
final jpg = JpegEncoder().compress(data!.buffer.asUint8List(), image.width, image.height, 90);
```

Before you ask, saving in Flutter is as simple as:

```dart
File(path).writeAsBytesSync(jpg); // or
await File(path).writeAsBytes(jpg);
```
