---
title: Display images from the internet
title: 显示网络上的远程图片
prev:
  title: Implement swipe to dismiss
  title: 实现「滑动清除」效果
  path: /docs/cookbook/gestures/dismissible
next:
  title: Fade in images with a placeholder
  title: 占位符和网络图片淡入
  path: /docs/cookbook/images/fading-in-images
---

Displaying images is fundamental for most mobile apps.
Flutter provides the [`Image`][] widget to
display different types of images.

对大多数移动应用来说，图片显示是一项基础功能。Flutter 提供了 [`Image`]({{site.api}}/flutter/widgets/Image-class.html) 来显示不同类型的图片。

To work with images from a URL, use the
[`Image.network()`][] constructor.

使用 [`Image.network`]({{site.api}}/flutter/widgets/Image/Image.network.html) 构造函数来处理来自 URL 的图片。

<!-- skip -->
```dart
Image.network(
  'https://picsum.photos/250?image=9',
)
```

## Bonus: animated gifs

## 意外之喜：Gif 动画

One useful thing about the `Image` widget:
It supports animated gifs.

`Image` Widget 令人兴奋的特性之一：提供了开箱即用的 gif 动画支持！

<!-- skip -->
```dart
Image.network(
  'https://github.com/flutter/plugins/raw/master/packages/video_player/doc/demo_ipod.gif?raw=true',
);
```

## Placeholders and caching

## 占位符与缓存

The default `Image.network` constructor doesn't handle more advanced
functionality, such as fading images in after loading, or caching images
to the device after they're downloaded. To accomplish these tasks, see
the following recipes:

默认的 `Image.network` 构造函数并没有提供诸如加载后的图片淡入或下载之后将图片缓存到设备等更进一步的功能。请参阅以下链接来实现这些功能

  * [Fade in images with a placeholder][]
  
    [占位符和网络图片淡入][Fade in images with a placeholder]
  
  * [Working with cached images][]
  
    [使用缓存图片][Working with cached images]

## Complete example

## 完整样例

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    var title = 'Web Images';

    return MaterialApp(
      title: title,
      home: Scaffold(
        appBar: AppBar(
          title: Text(title),
        ),
        body: Image.network(
          'https://picsum.photos/250?image=9',
        ),
      ),
    );
  }
}
```

![Network image demo](/images/cookbook/network-image.png){:.site-mobile-screenshot}


[Fade in images with a placeholder]: /docs/cookbook/images/fading-in-images
[`Image`]: {{site.api}}/flutter/widgets/Image-class.html
[`Image.network()`]: {{site.api}}/flutter/widgets/Image/Image.network.html
[Working with cached images]: /docs/cookbook/images/cached-images
