# vue-carousel-component

### Demo
###### CodeSandbox: https://codesandbox.io/s/github/giga0/vue-carousel-component
###### Full screen: https://v1yvx85ly7.codesandbox.io/

### Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Introduction
This is component made in Vue.js. Everything inside is written in vanilla JS and styles are in scss.

Carousel is made with the idea to handle images as slides with fixed height and a variable width depending on the image format (height). Carousel itself will take 100% of width and height of a parent. Arrows appear on hover of a carousel-wrapper (desktop behavior, on mobile they're always visible). It's possible to move slides via keyboard left and right arrow keys (on keydown, left or right arrow will appear shortly for better UX). If there is no more slides to the left or right, slider will reset to the first or last one. It's also possible to jump on specific slide clicking on a dot below slider. It's not possible to move slides by dragging them (for now).
On image click, component emit event imgClicked to a parent alongside with image object as a second parameter. It's also possible to listen beforeSlideMove and afterSlideMove events.

There is several props (none of them are required) that can be passed to Carousel component defining it appearance and functionality:

| Prop | Type | Default value | Additional info |
| ------ | ------ | ------ | ------ |
| images | Array | none | in case there is no slides (images), note will appear |
| goToSlide | String, Number | none | it's possible to move slides from parent, passing the value (left, right or number of slide) through this prop |
| imgHeight | String | 88% | if % is used it'll be relative to the carousel parent |
| slideCursor | String | default | this will result with slide style - cursor: default; |
| gap | Number | 10 | this will result with slide style - margin-right: 10px; needs to be a number for calculations inside component |
| arrows | Boolean | true | if we don't need arrows, should be false |
| arrowIcon | Object | svg inside template | it's possible to pass desired arrow icon as a prop so the component can render it |
| dots | Boolean | true | if we don't need dots, should be false |
| arrowsColors | Object | { background-color: '#0080FF', color: '#fff' } |  |
| arrowsDimension | Object | { width: '30px', height: '30px' } |  |
| arrowsRotation | Object | { leftArrow: '0deg', rightArrow: '0deg' } |  |
| arrowsGap | Object | { left: '5px', right: '5px' } | this will result with left and right arrow positioning relative to carousel-wrapper |
| dotsMarginTop | String | 18px | dots go below carousel-wrapper |
| dotsJustify | String | flex-start | this will result with dots style - flex: flex-start; |
| dotsDimension | Object | { width: '9px', height: '9px' } |  |
| dotsGap | String | 9px | this will result with dot style - margin-right: 9px; |
| noImgNote | String | No image(s) to show. |  |
