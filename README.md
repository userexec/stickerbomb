# Stickerbomb
**Single-command, in-browser guided image editor**

![stickerbomb](https://cloud.githubusercontent.com/assets/5970137/21624797/08bdc4ba-d1ce-11e6-9b88-b499f2e49a47.png)

Stickerbomb is an in-browser sticker-based image editor that allows your users to place stickers on a background, move them around, rotate, scale, mess with layers, etc. Remember [Blingee](http://www.blingee.com)?

Stickerbomb was originally created as a holiday card maker promotion for Missouri University of Science and Technology. It is MIT licensed for use in any project.

[Demo](http://rol.la/holiday_16)

[Demo 2](http://joshuawoehlke.com/stickerbomb-js-image-editor/)

## Usage

Include stickerbomb.js or stickerbomb.min.js on your site. **DO NOT** include stickerbomb.css or stickerbomb.scss--these files are only used to create the CSS string present in stickerbomb.js, which you will later edit with the styles properties.

Create a new show with the following command:

```javascript
var show = stickerbomb({
	target: '#selector_string',
	backdrops: [ 'images/backdrop1.png', 'images/backdrop2.png' ],
	stickers: {
		'Drawer Name 1': [
			{
				name: 'Sticker 1',
				src: 'images/sticker1.png',
				widthPercentage: 15
			},
			{
				name: 'Sticker 2',
				src: 'images/sticker2.png',
				widthPercentage: 20
			}
		],
		'Drawer Name 2': [
			{
				name: 'Sticker 3',
				src: 'images/sticker3.jpg',
				widthPercentage: 10
			},
			{
				// SVG sizing may not be consistent across browsers,
				// may require special post-processing for some,
				// and may require height as a function of width defined.
				// Recommend avoiding SVG unless needed
				name: 'Sticker 4',
				src: 'images/sticker4.svg',
				widthPercentage: 20,
				heightFxWidth: 1.8
			}
		]
	}
});
```

## Required properties

#### target
Type: *String*

Query selector for the target element that will hold the stickerbomb instance. Recommend a pre-sized and positioned div with an ID.

#### backdrops
Type: *Array* of *Strings*, Minimum length: 1

Paths to any images that should be selectable as backdrops. The first path in this array will be the default.

#### stickers
Type: *Object* of *Arrays*, Minimum properties: 1

The stickers property is an object containing "drawers" properties. The name of these drawers should be written in String format as shown in the example above, and their value should be an array of "stickers."

An unlimited number of sticker objects may be in each drawer array, and should generally have 3 properties:

- name (type: *String*)
- src (type: *String*, path to image)
- widthPercentage (type: *Number*, integer, Minimum: 1)

When stickerbomb loads, each of the "drawers" becomes an expandable button at the bottom of the interface that opens upwards to reveal its "stickers," which will be clickable images shown left-to-right.

You may have as many drawers and stickers as you like. If the length of the names of the drawers or the width of the images in the drawer are wider than the stickerbomb interface, scroll buttons will appear (left-right slide also works for touch-enabled devices).

## Options

#### aspectRatio
Type: *Number* or *String* (e.g. '16:9', '4:3'), Default: 1.55

The default aspect ratio fills one half of an 8.5x11 sheet of paper when printed due to this library's original purpose as a holiday card creator.


#### maxWidth
Type: *Number*, Default: 800

Useful for limiting the height of the editor so that it can be fully seen without scrolling. If this is not a concern, specify a higher number than would be necessary.

#### hideTools
Type: *Array* of *Strings*, Default: []

Hides specific tools (left bar) you would not like available. Default tools:

- move
- scale
- rotate
- mirror
- delete
- back
- forward

```javascript
var show = stickerbomb({
	target: '#selector_string',
	...
	hideTools: ['mirror', 'back', 'forward']
});
```

#### hideActions
Type: *Array* of *Strings*, Default: []

Hides specific actions (right bar) you would not like available. Default actions:

- share
- facebook
- twitter
- save
- print

```javascript
var show = stickerbomb({
	target: '#selector_string',
	...
	hideActions: ['save', 'print']
});
```

#### outputImageName
Type: *String*, Default: 'createdImage'

## Styles

Stickerbomb is skinnable using the `styles` property.

```javascript
var show = stickerbomb({
	target: '#selector_string',
	...
	styles: {
		leftBarButtons: '#FF9900',
		leftBarButtonsHover: '#CC6600',
		guideColors: [
			'#FF0000',
			'#CC0000',
			'#990000',
			'#660000'
		],
		guideOpacity: 0.5,
		guideNumberColor: '#FF0000',
		leftBarShadow: '5px 0px 5px rgba(0,0,0,0.5)'
		font: 'Bebas Neue, Arial, sans-serif'
	}
});
```

### Interface colors

The following properties of the styles object allow you to change the colors of the interface. All properties accept hex color strings.

- applicationBG
- leftBarBG
- leftBarButtons
- leftBarButtonsHover
- leftBarButtonsActive
- leftBarIcons
- leftBarIconsHover
- leftBarIconsActive
- rightBarBG
- rightBarButtons
- rightBarButtonsHover
- rightBarButtonsActive
- rightBarIcons
- rightBarIconsHover
- rightBarIconsActive
- tooltipText
- tooltipBG
- drawerBarBG
- drawerBarButtons
- drawerBarButtonsHover
- drawerBarButtonsActive
- drawerBarText
- drawerBarTextHover
- drawerBarTextActive
- drawerBG
- scrollButtons
- scrollButtonsBorders
- scrollButtonsIcons
- scrollButtonsHover
- scrollButtonsBordersHover
- scrollButtonsIconsHover

### Layer guides

Layer guides are the transparent squares that appear when a user clicks and holds, or when a tool is used that would require layer boundaries to be visible.

#### guideColors
Type: Array

Each new object will receive a random guide color to show its boundaries from the provided array of guide colors. This argument should be an array of hex color strings.

#### guideOpacity
Type: *Number*, Default: 0.8, Range: 0-1

#### guideNumberColor
Type: *String*, Default: '#FFFFFF'

### Additional properties

These properties accept CSS strings.

- leftBarShadow
- rightBarShadow
- drawerBarShadow
- font

## Advanced Options

#### actions
Type: *Object*

Custom actions can be added to the right bar using the following object format:

```javascript
var show = stickerbomb({
	target: '#selector_string',
	...
	actions: {
		actionName: {
			icon: 'fa-cog',
			tooltip: 'Special Custom Action',
			action: function(sb_instance) {
				// Your function here
				// console.log the passed sb_instance for ideas
			}
		}
	}
});
```

The passed sb_instance provides access to the canvas, all nodes of the interface, currently placed stickers and layers, and several utility functions such as the ability to re-render after changes and open/close popups. Please console.log the passed object for more information on what witchcraft is possible.


## Tips

- Use PNGs or JPGs; SVGs (as of Jan. 1 2017) are still handled weirdly enough that you'll have strange browser compatibility issues on things like IE10 and Safari. Nothing breaking, but scaling and placement weirdness aplenty.
- If you must use SVGs, specify a heightFxWidth (height as a function of width) for each sticker (e.g. 1.5), and ensure that the SVG itself has height and width properties. Some browser versions need the heightFxWidth, FireFox in general will need the height and width properties.
- An aspect ratio of 1.55 will allow you to print postcards. That's what this was originally created for. That also means the print function turns the image upside down by default (so it makes a pretty pretty card). Don't like it? Pull request!