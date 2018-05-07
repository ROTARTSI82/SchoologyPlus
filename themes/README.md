
# Custom Themes for Schoology Plus
### Introduction
Schoology Plus allows users to create, share, and apply custom themes for the Schoology interface. Themes are defined as a JSON object and can be installed to your browser from Schoology Plus settings. This can be done by pasting the JSON into the `Install from JSON` box found under the `Install and Manage Themes...` item in the `Theme` dropdown menu in Schoology Plus settings. 

![Example: installing a custom theme](https://i.imgur.com/x5QlaPZ.gif)

### Format
A Schoology Plus theme has the following format and components (each component will be explained in detail below):
```json
{
	"name": "My Theme",
	"hue": 359,
	"colors": [
		"red", 
		"rgb(0,0,255)", 
		"hsl(150,50%,50%)",
		"#000000"
	],
	"logo": "schoology",
	"cursor": null,
	"icons": {
		"BIO(LOGY)? ": "https://example.com/my-biology-image.png",
		"MATH|ALGEBRA": "https://example.com/my-math-image.png"
	}
}
```
### Components
|Property| Type | Optional? |
|--|--|--|
|[`name`](#name)|`string`|false
|[`hue`](#hue)|`number`|true
|[`colors`](#colors)|`string[]`|true
|[`logo`](#logo)|`URL \| string`|true
|[`cursor`](#cursor)|`URL`|true
|[`icons`](#icons)|`Dictionary<RegExp, URL>`|true

#### `name`
|Property||
|--|--|
|Key|`name`
|Value Type|`string`|
|Optional|false
|Default Value|N/A
|Description|The name of the theme. Used to identify the theme in the selection dropdown menu. Two themes with the same name cannot be installed at the same time.
|Value Restrictions|N/A
|Special Notes|N/A
|Example|`"name": "My Theme"`
#### `hue`
|Property||
|--|--|
|Key|`hue`
|Value Type|`number`|
|Optional|true
|Default Value|210 (Default Schoology Plus color theme)
|Description|The theme's HSL color hue, used to color the interface by modifying the saturation and lightness values. The default Schoology Plus theme uses hue 210, and this value will be used by default if no color definitions are present.
|Value Restrictions|Hues are integers between 0 and 359, however decimal numbers still work and numbers over 359 are subject to a modus of 360.
|Special Notes|Overridden by the [`colors`](#colors) property if specified
|Example|`"hue": 300`
#### `colors`
|Property||
|--|--|
|Key|`colors`
|Value Type|`string[]`|
|Optional|true
|Default Value|`undefined` (Falls back to [`hue`](#hue) if specified, else default blue color theme is used)
|Description|Allows themes to directly change the four interface colors rather than use a hue with varying saturation and lightness values.
|Value Restrictions|Array must have length 4 and strings must be [valid CSS colors](https://www.w3schools.com/colors/default.asp) in any valid format
|Special Notes|<table><tr><th>Index</th><th>Description</th><tr><td>0</td><td>The main interface color used in the navigation bar and as the primary color for buttons and other UI elements. **Should be dark enough to read white text.**</td></tr><tr><td>1</td><td>The background of the  settings dropdown button on the navbar next to your name.</td></tr><tr><td>2</td><td>The background color of buttons and other interactive elements when you hover over them, and the border color of the settings dropdown button on the navbar next to your name. **Should be dark enough to read white text.**</td></tr><tr><td>3</td><td>The border color of buttons.</td></tr></table>Overrides [`hue`](#hue) if set.
|Example|`"colors": ["#011e3e","#06448E","#b70014","#b70014"]`
#### `logo`
|Property||
|--|--|
|Key|`logo`
|Value Type|`URL | string`|
|Optional|true
|Default Value|![Schoology logo](https://i.imgur.com/y64kiCY.png)
|Description|An image URL to be used as the logo on the left side of the navbar.
|Value Restrictions|A direct link to an image (ideally sized **`160x36`**) -OR- `"schoology"` or `"lausd"`
|Special Notes|<table><tr><th>Value</th><th>Preview</th></tr><tr><td>`"schoology"`</td><td>![Schoology logo](https://i.imgur.com/y64kiCY.png)</td></tr><tr><td>`"lausd"`</td><td>![LAUSD logo](https://imgur.com/Mm7FXhD.png)</td></tr></table>**NOTE**: The Schoology logo has a transparent background, however the LAUSD logo background is orange as shown.
|Example|`"logo": "https://example.com/my-correctly-sized-image.png"`<br/>`"logo": "schoology"`
#### `cursor`
|Property||
|--|--|
|Key|`cursor`
|Value Type|`URL`|
|Optional|true
|Default Value|System default cursor
|Description|An image url to be used as the standard mouse cursor (i.e. not when hovering over links)
|Value Restrictions|A direct image link. Chrome and Firefox restrict cursor images to a **maximum size of `128x128`**, however it is recommended to use a significantly smaller image size.
|Special Notes|This only replaces the default cursor (CSS value `default`), not the cursor shown when hovering over links or performing other special actions.
|Example|`"cursor": "https://imgur.com/5QVrDqX.png"`
#### `icons`
|Property||
|--|--|
|Key|`icons`
|Value Type|`Dictionary<RegExp,URL>`|
|Optional|true
|Default Value|Schoology Plus default course icon set
|Description|A dictionary of regular expressions to image URLs to be used as icons for courses.
|Value Restrictions|A JSON object where all keys are valid regular expressions and all values are direct image links. Images should be square and at least `32x32` in size.
|Special Notes|Course names are checked against regular expressions in object order, meaning the first keys on the object are checked first *in a non-case-sensitive manor*. If no regular expression matches a specific course, Schoology Plus will fallback to the default Schoology Plus icon set. If you want to prevent this behavior, add an entry such as `".": "https://example.com/my-image.png"` that will match all course titles.
|Example|`"icons": {"FRENCH": "https://cdn.countryflags.com/thumbs/france/flag-round-250.png",".": "https://image.flaticon.com/icons/svg/183/183759.svg"}`<br/><br/>[See here](https://github.com/aopell/SchoologyPlus/blob/master/js/icons.js) for another example