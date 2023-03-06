This sample block is a reference to help you start building your own blocks and use them in YAWB.io

[YAWB.io](https://app.yawb.io) stands for Yet Another Website Builder and it’s the most flexible and extendable website building platform there is while being easy to use and user-friendly.

The power of [YAWB.io](https://app.yawb.io) is its **Blocks System**. You can drag and drop blocks into the pages of your website to build any layout you desire. What makes YAWB.io unique is that contribute and create new blocks. The blocks can be imported and shared.

This document explains how to create your own custom blocks.

A Block can consist of a few files:

- settings.json : Settings describing what the block is about, it’s name, handle and the files it needs to work. [Mandatory File]
- schema.json : Defines the properties and customizations that the block allows. This file is used by [YAWB.io](https://app.yawb.io) to show the customization fields for the block. [Mandatory File]
- Entry file : a Liquid template file that contains the HTML of the block. It’s a liquid file meaning that you can inject variables and dynamic content into the template. [Mandatory File]
- Stylesheet : a CSS file for styling the block. [YAWB.io](https://app.yawb.io) supports using liquid code directly in the stylesheet.
- Script : a Javascript file for adding functionality to the block. [YAWB.io](https://app.yawb.io) supports using liquid code directly in the script file.
- Placeholder file : An alternative to the entry file that should be used instead when viewing the block in [YAWB.io](https://app.yawb.io)’s editing mode. When viewing a block in YAWB.io’s editing mode scripts and some HTML tags and attributes are stripped which can break the functionality of the block. You can use the placeholder file as an alternative to show a preview of the block in editing mode.

In this document, we also discuss the grid system that [YAWB.io](https://app.yawb.io) uses.

We highly recommend viewing the code of the built-in blocks to better understand how to use the different features and options specified here.

# settings.json

```javascript
{
	"name": "Button", // Displayed in the blocks grid [required]
	"handle": "button", // A unique handle for the block [required]
	"author": "yawb", // Must be lowercase and only contain letters, numbers an the dash character [required]
	"category": "basic", // values: basic, media, advanced, header, footer [required]
	"description": "",
	"entry": "block.liquid", // Name of the html file of the block [required]
	"placeholder": "placeholder.liquid", // Name of the html file of the block for preview mode [optional]
	"stylesheet": "style.css", // Name of the CSS stylesheet if necessary [optional]
	"script": "script.js", // Name of the JS script if necessary [optional]
	"icon": "smart_button" // Block icon (as definied in the Material Symbols Icon pack: https://fonts.google.com/icons) [required]
}
```

[YAWB.io](https://app.yawb.io) uses JSON5 which means that it parses JSON files more leniently so you can have comments in the JSON file.

# schema.json

```javascript
{
	"resizable": true, // Whether the height of the block can be resizable [default: false]
	"manageNarrowWidth": false, // Whether the narrow width feature is managed directly by the block [default: false]
	"elements": [ // [optional]
		// See next sections
	],
	"properties":[
		// See next sections
	]
}
```

## Schema Fields (properties)

The following fields are supported for the *properties* property.

```javascript
[
	{ // Text Field
		"name": "Text", // Title that displays above the field in the editor [required]
		"variable": "text", // Variable that this field sets [required]
		"type": "text", // Type of field [required]
		"maxLength": 15, // Maximum number of characters allowed [optional]
		"multiline": false, // Whether this is a multiline text field or not [default: false]
		"default": "Lorem ipsum", // Default value [default: ""]
		"cssVariable": true, // Whether to generate a CSS variable for this field (for easy CSS integration) [default: false]
		"showWhen": { // Conditionally show this field [optional]
			"variable": "number", // Variable to check [required]
			"comparator": ">", // values: ==, !=, >, <, >=, <= [default: ==]
			"value": 0 // Value that the variable needs to satisfy for this field to show. Either a string or a number. [required]
		}
	},
	{ // Instruction field (notice box)
		"name"			: "Type a text value in the field above.", // 
		"type"			: "instruction",
		"level"			: "error" // value: error, info, warning, success [default: info]
		// Other optional fields: showWhen
	},
	{ // Number Input
		"name"			: "Type a number",
		"variable"		: "number",
		"type"			: "number",
		"default"		: 10, // [default: 0]
		"min"		: 0, // Minimum value [optional]
		"max"		: 100,// Maximum value [optional]
		"unit"			: "em", // [optional]
		"integer"			: true // Whether to only allow integer values
		// Other optional fields: cssVariable, showWhen
	},
	{ // Dropdown Menu
		"name": "Dropdown Menu",
		"variable": "option",
		"type": "dropdown",
		"options": [ // List of possible options [required]
			{
				"name": "Cover", // Name that displays in the dropdown
				"value": "cover" // Value when selected
			},
			{
				"name": "Fill",
				"value": "fill"
			}
		],
		"default": "cover" // One of the possible values [required]
		// Other optional fields: cssVariable, showWhen
	},
	{ // Section
		"name": "Collection of fields",
		"type": "section",
		"collapsed": false, // Whether the section is collapsed by default
		"fields"		: [ // Fields [required]
			{
				"name": "Text",
				"variable": "anothertext",
				"type": "text"
			},
			{
				"name": "Type a number",
				"variable": "sectionnumber",
				"type": "number",
				"default": 10
			}
		]
		// Other optional fields: showWhen
	},
	{ // Image Uploader
		"name": "Image",
		"variable": "img",
		"type": "image",
		"default": "https://..." // Default image URL [optional]
		// Other optional fields: cssVariable, showWhen
	},
	{ // Image Gallery field
		"name": "Upload many images",
		"variable": "imgs",
		"type": "image-gallery",
		"maxImages": 4, // Number of images to allow [optional]
		"default": ["https://..."] // Array of default images [optional]
		// Other optional fields: showWhen
	},
	{ // URL Text input
		"name": "Url",
		"variable": "url",
		"type": "url"
		// Other optional fields: default, cssVariable, showWhen
	},
	{ // Number Slider
		"name": "Slide Value",
		"variable": "slider",
		"type": "slider",
		"default": 1, // [required]
		"min": 0, // [required]
		"max": 1 // [required]
		// Other optional fields: integer, unit, cssVariable, showWhen
	},
	{ // Color picker
		"name": "Color",
		"variable": "color",
		"type": "color",
		"default": "#FF000055", // HEX or HEXA color value (HEXA is HEX with alpha appended to it) OR preset color: =primaryColor, =primaryColorAccent, =primaryColorContrast... [default: #000000]
		"alpha": true // Whether to allow transparency [default: true]
		// Other optional fields: cssVariable, showWhen
	},
	{ // Font picker
		"name": "Font",
		"variable": "font",
		"type": "font",
		"default": {
			"family": "Cairo", // A Google Fonts name
			"size": "1em", // supported units: em, px, 
			"weight": 600, // from 100 to 1000
			"style": "italic" // value: normal, italic [default: normal]
		}, // Should be a font name string if all disableWeight, disableSize and disableStyle are true

		"disableWeight": false, // Whether or not to disable the weight field [default: false]
		"disableSize": false, // Whether or not to disable the size field [default: false]
		"disableStyle": false // Whether or not to disable the style field [default: false]
		// Other optional fields: cssVariable, showWhen
	}
]
```

### showWhen

Fields have an extra property that you can use, the showWhen property. It allows you to conditionally show the field when the condition is respected.

The condition follows this format:

```javascript
{
	"variable": "size",
  "comparator": ">", // Possible comparators: ==, !=, >, <, >=, <= [default: ==]
	"value": 5// String or number
}
```

The when using the == comparator, the value property can be an array in which case the condition is true when any of the values match the condition.

You can have multiple conditions by putting them in an Array:

```javascript
[
	{ // A
		"variable": "size",
	  "comparator": ">",
		"value": 5
	},
	{ // B
		"variable": "size",
	  "comparator": "<",
		"value": 10
	}
]
```

Translates to show the field when the size value is above 5 and below 10.

A && B

You can use the *OR* operator by putting the condition in a second Array:

```javascript
[
	[
		{ // A
			"variable": "size",
		  "comparator": ">",
			"value": 5
		},
		{ // B
			"variable": "size",
		  "comparator": "<",
			"value": 10
		}
	],
	[
		{ // C
			"variable": "size",
		  "comparator": "==",
			"value": 2
		}
	]
}
```

Translates to show the field when the size value is above 5 and below 10 or if the size is 2.

(A && B) || C

## Schema Elements

Schema elements are a way to allow the user to customize the styling of the elements in the block. You simply need to *tag* the HTML elements using the *yawb-element* attribute (space separated list) and define the elements’ customizations in the schema.

```javascript
{
	"name": "Block of text", // Name of the element
	"handle": "text", // Handle of the element that is used in the yawb-element HTML attribute
	"customizations": [ // Array of CSS properties that are allowed to be customized. Supported properties:
		"color",
		"background-color",
		"font-family",
		"font-size",
		"font-weight",
		"font-style",
		"text-transform",
		"text-decoration",
		"letter-spacing",
		"line-height",
		"text-align",
		"padding",
		"margin",
		"opacity",
		"border",
		"border-radius",
		"transform-scale",
		"filter",
		"transition"
	], // Instead of an Array, you can also use one of the presets: "all", "text", "box", "basic", "image"
	"hoverCustomizations": true, // Whether to allow customizing the hover state [default: false]
	"activeCustomizations": false, // Whether to allow customizing the active (clicked) state [default: false]
	"defaults": { // Default style for this element [optional]
		"color"				: "#000000",
		"background-color"	: "#FFFFFF33",
		"font-family"		: "Cairo",
		"font-size"			: 1, // in em units
		"font-weight"		: 400, // 100 to 1000
		"font-style"		: "italic", // normal, italic 
		"text-transform"	: "uppercase", // none, uppercase, lowercase
		"text-decoration"	: "underline", // none, underline
		"letter-spacing"	: 0, // -0.2 to 2
		"line-height"		: 0, // 0 to 2
		"text-align"		: "center", // left, center, right, justify
		"padding"			: [5, 0, 5, 0], // px: [Top, Right, Bottom, Left] or Number
		"margin"			: 10, // px: [Top, Right, Bottom, Left] or Number
		"opacity"			: 100, // 0 to 100
		"border-width"		: 2, // px: [Top, Right, Bottom, Left] or Number
		"border-color"		: "#000000",
		"border-style"		: "solid", // solid, dashed, dotted
		"border-radius"		: 5, // 0 to 200
		"transform"			: {
			"scale"		: 100,// 0 to 300
		},
		"filter"			: 
			"blur"		: 0,// 0 to 20
			"grayscale"	: 0,// 0 to 100
			"invert"	: 0,// 0 to 100
		"transition"		: 0.25 // seconds: 0 to 2
	},
	"defaultsHover": {}, // same as defaults but without the transition property [optional]
	"defaultsActive": {} // same as defaults but without the transition property [optional]
}
```

## Entry file

An HTML+Liquid file that contains the HTML of the block.

Liquid is a template engine that has a syntax similar to Handlebars or even Mustache. It allows you to do things like:

```html
<div>{{ content | uppercase }}</div>
```

This will inject the content variable (or block property in the case of [YAWB.io](https://app.yawb.io)) and apply the uppercase filter to it.

Several filters are available. [YAWB.io](https://app.yawb.io) supports the filters provided by [LiquidJS](https://liquidjs.com/filters/overview.html):

And in addition defines a few additional filters:

### image_url: [size]

Takes an image property as an input and outputs the URL of the image. You can specify the image's width using the size argument.
```html
{{ image | image_url }}
{{ image | image_url: 500 }}
```
Generates:
```html
https://yawb-user-images.s3.amazonaws.com/user-images/63811c56e47bca4770d96655/photo-1667122169237-f9826de1786b-e849b225-c536-46de-a7be-4ca26e98d56f-638edb809a1b2f8ff35ab7c.jpg
https://yawb-user-images.s3.amazonaws.com/user-images/63811c56e47bca4770d96655/photo-1667122169237-f9826de1786b-e849b225-c536-46de-a7be-4ca26e98d56f-638edb809a1b2f8ff35ab7c-500.jpg
```

### image_srcset

Takes an image property as an input and outputs the src attributes to be used in an img tag.

Example:

```html
<img {{ image | image_srcset }} alt="{{ image.alt }}" />
```

Would output:

```html
<img srcset="https://yawb-user-images.s3.amazonaws.com/user-images/63811c56e47bca4770d96655/photo-1667122169237-f9826de1786b-e849b225-c536-46de-a7be-4ca26e98d56f-638edb809a1b2f8ff35ab7c-64.jpg 64w, https://yawb-user-images.s3.amazonaws.com/user-images/63811c56e47bca4770d96655/photo-1667122169237-f9826de1786b-e849b225-c536-46de-a7be-4ca26e98d56f-638edb809a1b2f8ff35ab7c-200.jpg 200w, https://yawb-user-images.s3.amazonaws.com/user-images/63811c56e47bca4770d96655/photo-1667122169237-f9826de1786b-e849b225-c536-46de-a7be-4ca26e98d56f-638edb809a1b2f8ff35ab7c-500.jpg 500w, https://yawb-user-images.s3.amazonaws.com/user-images/63811c56e47bca4770d96655/photo-1667122169237-f9826de1786b-e849b225-c536-46de-a7be-4ca26e98d56f-638edb809a1b2f8ff35ab7c.jpg 1080w" sizes="(max-width: 128px) 50vw, (max-width: 400px) 50vw, (max-width: 1000px) 50vw, 50vw" loading="lazy" alt="Alternative text" />
```

This ensures that the best image size loads depending on the screen size, the image's intrinsic dimensions and the size it takes on the page. It also adds the lazy loading attribute to only load the image when it's needed.

Note that image_srcset takes an extra argument: proportion. It allows you to indicate what is the maximum proportion the image will take relative to the screen's width. If you have two columns of images, you can write `<img {{ image | image_srcset: 0.5 }} alt="{{ image.alt }}" />` since each image will take a maximum of half of the screen.

### css_var
For use in CSS. It allows you to link a property to a variable of your block.
```css
#_BLOCK_ .element {
	color: {{ "colorfield" | css_var }};
}
```
Generates:
```css
#_BLOCK_ .element {
	color: var(--colorfield);
}
```

### isActiveUrl
Returns a boolean when the URL that is passed to it is the URL of the current page. It's a useful helper that can be used when building navigation links to highlight the current page.
```html
{% for link in menu %}
	{% assign activeUrl = link.url | isActiveUrl %}
	<a href="{{ link.url }}" class="{% if activeUrl %}current-page{% endif %}">{{ link.name }}</a>
{% endfor %}
```


### relative_time
Takes a date value and generate a relative time value from it.
```html
<span>{{ date | relative_time }}</span>
```
Could generate:
```html
<span>23 days ago</span>
```
or (depending on the date):
```html
<span>5 minutes ago</span>
```


### date
Formats the date value. Takes an optional argument `showTime` show the time in the formatted date as well.
```html
<span>{{ datevalue | date: true }}</span>
```

### if: condition[, comparator, value] [, otherwise]
Conditionaly shows a value:
```html
{{ "Hello world" | if: value }}
```
Will display "Hello world" when the value variable is truthy.

```html
{{ "Hello world" | if: value, "<", 20, "Foo" }}
```
Will display "Hello world" when the value variable is lower than 20 otherwise it displays "Foo".

### notionPagesSort

Given a list of notion pages such as `_notionPage.subPages`, this filter is able to sort them following a sorting criterion given as an argument. The following sorting modes are available: regular, reverse, dateModified, reverseDateModified, dateCreated, dateCreated.
```html
{% assign pages = _notionPage.subPages | notionPagesSort: "dateModified" %}
```

### notionPagesSort

Given a notion page's blocks array such as `_notionPage.blocks`, this filter converts the blocks into their HTML equivalent. This is a helper to easily display the content of a Notion page.
It also has an optional argument, `hideSubPages` that allows you to render Notion's `child_page` blocks or not.
```html
{{ _notionPage.blocks | notionBlocksToHtml }}
```

### singlelineText

Parses a single-line text value. It replaces whitespace when necessary into &nbsp; entities to ensure that the output reflects what the user has entered.

### multilineText

In addition to what the singlelineText filter does, this filter also replaces new lines into <br> tags to reflect new lines in the HTML output.

### Escaping by default

[YAWB.io](https://app.yawb.io) escapes the output by default to prevent XSS injections. In case you want to inject HTML without escaping, you can use the raw filter like this:

```html
{{ html | raw }}
```

## Variables
You can refer to the values of the block properties that are defined in the schema file directly in the liquid code like this:
```html
{{ variablename }}
```
Given that the property is defined in the schema file:
```javascript
{
	"properties":[
		{
			{
				"name": "Text",
				"variable": "variablename",
				"type": "text"
			}
		}
	]
}
```

You can also access a special variable `_blockSettings` that gives you access to some information about the current block and the page itself:
```javascript
_blockSettings = {
	placeholderMode			: false, // Whether the block is being rendered in the preview frame or in the live site
	id						: '', // ID of the current block
	depth					: 0, // Depth of the block. 1 when the block is in a row of blocks and 0 otherwise
	fullWidth				: true, // Whether the block is full-width or narrow-width.
	span					: 6, // Number of columns the block takes when it's in a row of blocks. You need to divide by 12 to get the proportion of the width this block takes
	height					: false, // Height of the block. A number if it's defined (pixels) or false when the block takes it's natural height
	hideDesktop				: false, // Whether this block should be hidden for desktop devices
	hideTablet				: false, // Whether this block should be hidden for tablet devices
	hideMobile				: false, // Whether this block should be hidden for mobile devices
	narrowWidthDimension	: 1200 // The value of the narrowWidth dimension setting of the site
}
```


yawb.io also provides the `_notionPage` variable for Notion pages of your websites. This variable is structured as follows:
```javascript
_notionPage = {
	blocks	: [
		// Array of blocks in the page
		// This is a simplified version of the blocks returned by the Notion API
		// https://developers.notion.com/reference/block
		// You can refer to the official API for a list of blocks types and structure
		{
			id: '170cb5c6-28db-4cb1-a7d2-8b4bc580ddb8',
			type: 'heading_1',
			hasChildren: false,
			content: 'Page title',
			toggleable: false
		},
		{
			id: '6cf2ae45-9dbc-4a0e-8dd1-3f15abd844cd',
			type: 'paragraph',
			hasChildren: false,
			content: 'Hello <b>world.</b>'
		}
		// ...
	],
	page		: { // Information about the Notion page
		id: '4368f3dc-c421-431c-a740-c4932c3c0595',	// Notion page ID
		title: 'yawb.io + Notion',// Title
		url: 'yawb-io-notion-Q2jz3MQhQxynQMSTLDwFlQ',// Handle (url) of the page
		cover: false,// Cover image of the page
		dateCreated: '2023-03-05T18:44:00.000Z',
		dateModified: '2023-03-06T04:22:00.000Z',
		parent: 'e440d5fc-0b3c-42e3-a9af-e901d2d2aaa7' // Parent ID of the Notion page
	},
	subPages	: [
		// List of pages that are direct children of the current page.
	]
}
```

## Editable content

When you inject text content to the element, you can mark that HTML element as editable using the yawb-editable attribute and type the name of the variable as its value. That way the user can edit that text directly in the preview section.

```javascript
<div yawb-editable="content">{{ content | multilineText }}</div>
```

## Customizable Elements

You can mark an element as customizable by adding the *yawb-element* attribute and specify which element this is (must match the handle of the element you specified in the schema file):

```javascript
<div yawb-element="customizable">Hello world</div>
```

There are some global elements that you can use so that the same styling can be reused across multiple blocks:

- _button
- _button-small
- _button-large
- _button-extralarge
- _paragraph
- _markdown
- _h1 - _h6 : Heading elements

You can combine multiple styles by separating the element handles using a space character:

```html
<span yawb-element="button _button _button-large">My button</span>
```

In this example, the button is associated with three styles:

- button : A custom style that is defined for this particular block
- _button : A global style for buttons, shared across many block types
- _button-large : A global style for large buttons, shared across many block types

# Stylesheet

A CSS + Liquid file to style your block. There are two important things to understand when making your CSS code:

1. Always prepend CSS selectors with #_BLOCK_ 

This keyword will automatically be replaced by [YAWB.io](https://app.yawb.io) to contain your styling to your block only. This ensures that block styles do not interfere with one another. If your block is using a .image class, it won’t interfere with other blocks using the same class.

1. You can refer to the block properties that are marked as “cssVariable” using the css_var filter like this:

```css
#_BLOCK_ .image {
	color: {{ "color" | css_var }};
	font-family: {{ "customfont.family" | css_var }};
	font-size: {{ "customfont.size" | css_var }};
}
```

Notice that font properties are objects with a few fields that you can access: font.family, font.size, font.weight, font.style.

For number and slider inputs, the unit is appended to the value. You can get the number without the unit like this:

```css
opacity: {{ "opacity_withoutUnit" | css_var }};
```

# Script

The script file is similar to the stylesheet. In scripts too, you must always prepend the selectors with the #_BLOCK_ keyword.

This means that your code should look like this:

```javascript
document.querySelector('#_BLOCK_ .button').addEventListener('click', ()=>alert('Clicked!'));
```

## YAWB Helpers
You can use the global YAWB object in your javascript code to access some helper functions:
```javascript
// A helper function that takes a form DOM element as an input and returns an object with name-value pairs for each input present in the form.
var formData = YAWB.serializeForm(formElement, {
	asArray		: true, // Return the data as an array of fields: {key, value}, otherwise returns the data as an object
	validate	: true, // Validate the form fields while retrieving the values. If a field doesn't validate, the function returns false
});

// Asynchronous helper that allows you to send an email to the website owner. This can be used for a contact form as an example.
var {success, message} = await YAWB.sendEmail([
	{
		key		: "name",
		value	: "John Doe"
	},
	{
		key		: "message",
		value	: "Hey there"
	},
]);



// Liquid filters:
// These helpers provide the same functionality as they liquid filter equivalents:
YAWB.escapeHtml(...);
YAWB.trim(...);
YAWB.image_srcset(...);
YAWB.image_url(...);
YAWB.css_var(...);
YAWB.uppercase(...);
YAWB.lowercase(...);
YAWB.relative_time(...);
YAWB.money(...);
YAWB.date(...);
```

# Placeholder

The placeholder file has the same specifications as the entry file. The difference is that it is intended to be used in the editing mode of [YAWB.io](https://app.yawb.io) and should show a preview of the block. **You must not use any scripts in the placeholder file.**

If your block doesn’t use scripts to begin with, you can omit the placeholder file as it may not be necessary.

# YAWB.io’s grid system

Blocks in [YAWB.io](https://app.yawb.io) are placed on top of each other consecutively.

If the resizable property in the schema allows it, the height of the block can be resized directly in the editing mode. This is useful for images or hero sliders.

The user can also customize the width of the block by setting it to be full-width or narrow-width.

Blocks can also be put side by side. When placed horizontally, each block takes a set number of columns. The whole width is divided in 12 equal columns. In the horizontal mode, the full-width/narrow-width property no longer applies to the individual blocks but to the whole row of blocks.