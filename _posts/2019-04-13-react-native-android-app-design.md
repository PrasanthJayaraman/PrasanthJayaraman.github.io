---
title: "React Native - Scale Android TV app for all TV resolutions"
categories:
  - Javascript
tags:
  - React-Native
comments: true
---

When i started working on a react-native project for android TV, i faced a very common scaling issue, how to design my application
that fit multiple screen sizes of different resolutions.

First let's check the web (as many of us come from web background), In web development we can set `viewport` where we set content width
and initial scaling values. Also we have frameworks like Bootstrap, Foundation, UIKit, etc.

Now let's come to react-native, In react-native we have `flexbox` which helps to provide consistent UI for different screen sizes.
But the flexbox alone is not helpful. We may also need to use `%` in the styles as `react-native@0.42` support % in the CSS styles.

But i had a thought that using '%' is still not a recommended way where my application gonna be used in thousands of TVs of different
screen sizes. So i looked further in react-native documentation and found [**Dimensions**](https://reactnative.dev/docs/dimensions)
, [**PixelRatio**](https://reactnative.dev/docs/pixelratio) and [**React Native Pixel Perfect**](https://www.npmjs.com/package/react-native-pixel-perfect)

## Dimensions

`Dimensions` in react native helps to get the window's width and height and also we can use the event listener available in the Dimensions
module to know if there is a change in window height and width.

```
const windowWidth = Dimensions.get('window').width;
const windowHeight = Dimensions.get('window').height;
```

## PixelRatio

`PixelRatio` in react native helps to get the device pixel density. Pixel density will varies device to device based on the resolution and pixels per inch (ppi).

For an mdpi android device `PixelRatio.get() === 1` and hdpi android device `PixelRatio.get() === 1.5` and [**so on**](https://material.io/resources/devices/).

## React Native Pixel Perfect

`react-native-pixel-perfect` helps to create pixel perfect app of any screen sizes. It has a `create` function where we need to supply the device resolution so it generates a function which computes the correct size for different screen sizes.

```
const designResolution = {
    width: 1125,
    height: 2436
};
const perfectSize = create(designResolution);
perfectSize(50);
```

`perfectSize(50)` will return the actual size needed to make 50 fit the screen perfectly according to original design. `50` is `50px` in CSS styles.

Let's say for High Definition TVs

```
720p Resolution TV = 1280 x 720 progressive scan. (Total pixels = 921,600)
perfectSize(50) is 23.589

similarly,

1080p Resolution TV = 1920 x 1080 progressive scan. (Total pixels = 2,073,600)
perfetctSize(50) is 15.726
```

So using the module we can achieve pixel perfect application which reflects our design UI perfectly.

Now let's combine all these together for a perfect and dynamic adaptive application.

```
import {PixelRatio, Dimensions} from 'react-native';
import {create} from 'react-native-pixel-perfect';

let displayProps = {
  width: PixelRatio.roundToNearestPixel(
    Dimensions.get('window').width * PixelRatio.get(),
  ),
  height: PixelRatio.roundToNearestPixel(
    Dimensions.get('window').height * PixelRatio.get(),
  ),
};

export const perfectSize = create(displayProps);
```

you can export the perfectSize fn and import it in any component to use it for pixel perfect size.

```
import { perfectSize } from './helper.js';

const styles = StyleSheet.create({
   container: {
    width: perfectSize(500),
    height: perfectSize(300)
   }
});
```

Finally achieved the design scaling for different screen sizes. :)
