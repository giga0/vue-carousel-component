# vue-carousel-component

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

## Introduction
```
This is component made for Vue.js projects. Everything inside it is written in vanilla JS and styles are in scss.

Carousel is made with the idea to handle images as slides with fixed height and a variable width depending on the image format. Carousel itself will take 100% of width and height of a parent. Arrows appear on hover of a carousel-wrapper. It's possible to move slides via keyboard left and right arrow keys (on keydown, left or right arrow will appear shortly for better UX). If there is no more slides to the left or right, slider will reset to the first or last one. It's also possible to jump on specific slide clicking on a dot below slider.
On image click, component emit event imgClicked to a parent alongside with image object as a second parameter. It's also possible to listen beforeSlideMove and afterSlideMove events.

There is several props (none of them are required) that can be passed to Carousel component defining it appearance and functionality:
- images | type: Array | default value: none | in case there is no slides (images), note will appear
- goToSlide | type: String, Number | default value: none | it's possible to move slides from parent, passing the value (left, right or number of slide) through this prop
- imgHeight | type: String | default value: '88%' | if % is used it'll be relative to the carousel parent
- slideCursor | type: String | default value: 'default' | this will result with slide style - cursor: default;
- gap | type: Number | default value: 10 | this will result with slide style - margin-right: 10px; needs to be a number for calculations inside component
- arrows | type: Boolean | default value: true | if we don't need arrows, should be false
- arrowIcon | type: Object | default value: svg inside template | it's possible to pass desired arrow icon as a prop so the component can render it
- dots | type: Boolean | default value: true | if we don't need dots, should be false
- arrowsColors | type: Object | default value: { 'background-color': '#0080FF', color: '#fff' }
- arrowsDimension | type: Object | default value: { width: '30px', height: '30px' }
- arrowsRotation | type: Object | default value: { leftArrow: '0deg', rightArrow: '0deg' }
- arrowsGap | type: Object | default value: { left: '5px', right: '5px' } | this will result with left and right arrow positioning relative to carousel-wrapper
- dotsMarginTop | type: String | default value: '18px' | dots go below carousel-wrapper
- dotsJustify | type: String | default value: 'flex-start' | this will result with dots style - flex: flex-start;
- dotsDimension | type: Object | default value: { width: '9px', height: '9px' }
- dotsGap | type: String | default value: '9px' | this will result with dot style - margin-right: 9px;
- noImgNote | type: String | default value: 'No image(s) to show.'
```
