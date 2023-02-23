# Tailwind

Tailwind is a CSS compiler framework. It is very popular and has a lot to recommend it. It may not be a fit for every project. 

You should all give it a try and test it out. You may grow to like it! 

Tailwind uses atomic or utility classes. These are classes that contain set one or two CSS properties. For example atomic classes might be "rounded" which rounds all corners, and "p-4" which sets 4 units of padding. In conparison to a monolithic class that sets the both of these properties and more using a single class name. 

Tailwind uses these classes to describe CSS styles in a sort of shorthand. 

Tailwind uses a process that scans your HTML for class names and creates a stylesheet from what it finds. Essentially creating the stylesheet from the class names you have used. 

Pros and cons of Tailwind: 

- Uses class names that bloat your HTML
- Write less code than you might write in standard CSS
- You don't have to make up class names and keep track of these
- CSS is generated so there is no unused CSS
- Takes a little bit of setup

## Getting started 

Create a folder to work. This example will create a simple HTML file that uses Tailwind. 

Check out the docs here: https://tailwindcss.com/docs/installation

Install Tailwind:

```
npm install -D tailwindcss
npx tailwindcss init
```

Open `tailwind.config.js` and edit `content` to match:

```JS
/** @type {import('tailwindcss').Config} */
module.exports = {
	content: ["./src/**/*.{html,js}"],
	theme: {
		extend: {},
	},
	plugins: [],
}
```

Add: `src/input.css`

Add the following to input CSS: 

```CSS
@tailwind base;
@tailwind components;
@tailwind utilities;
```

These lines import the base styles from Tailwind. 

Add an HTML file with the boilerplate HTML: `dist/index.html`. Add this line to load your CSS in the head of the document: 

```HTML
<link href="output.css" rel="stylesheet">
```

Run:

```
npx tailwindcss -i ./src/input.css -o ./dist/output.css --watch
```
Notice this line is reading `src/input.css` and outputting `dist/output.css`. 

The `--watch` flag compiles and rewrites the output.css every time you save your other files. 

If you are using Liveserver in VS Code you go to the liveserver settings and and check the box: Full Reload. Live server doesn't refresh CSS when reloading this will force it to also reload CSS, so that you see changes from Tailwind. 

Install the Tailwind extension for VS Code. Search for it in extensions. This will give you code hints and make it a lot faster and easier to work with. 

Style the body tag. Tailwind works via class names. Each class represents one or more CSS styles. The name also includes the value. 


Try this: 

```HTML
<body class="bg-slate-400 text-slate-200">
```

- `bg-slate-700` sets the background color to slate gray. The number sets the value. Low numbers are lighter and high numbers are darker. Try bg-slate-100 and bg-slate-900
- `text-slate-200` sets the color of the text. 

If you have the VS code extension installed you will see a color preview with the code hints. 

Check out the colors here: https://tailwindcss.com/docs/customizing-colors#default-color-palette

Add this: 

```HTML
<h1 class="text-3xl font-thin">
	Hello, world!
</h1>
```

- `text-3kl` sets the font size to 3xl large
- `font-thin` sets the font weight to thin

Looks these up here: https://tailwindcss.com/docs/font-size#setting-the-font-size

https://tailwindcss.com/docs/font-weight#setting-the-font-weight

That was pretty easy so far! Notice that you are describing what you want in a general way while the framework applies a consistent style to everything. 

Put the text in the middle of the page. To do this you need to apply styles that do the following: 

- make the body tag 100vh
- set body display grid
- put the content in the center of the grid cell

Add the following classes to the body tag: 

```HTML
<body class="bg-slate-700 text-slate-200 grid place-content-center min-h-screen">
```

- `grid` sets display grid
- `place-content-center` places content in the center of a grid cell
- `min-h-screen` sets min-height to the height of the screen

That was pretty easy and code is self-documenting when you understand the language it is using. Many of the class names are the same or relate to the CSS styles that are generated. 

Look these styles up in the docs: https://tailwindcss.com/docs/installation

Find the search bar and search for styles you might use: 

- margin
- padding
- background-color

Make a card. Add the following html: 

```HTML
<div class="bg-white rounded-xl shadow-lg">
	<div>
	<div class="">ChitChat</div>
		<p class="">You have a new message!</p>
	</div>
</div>
```

Add a background, some rounded corners, and a shadow.

```HTML
<div class="bg-white rounded-xl shadow-lg p-6">
	<div>
	<div class="">ChitChat</div>
		<p class="">You have a new message!</p>
	</div>
</div>
```

Add some padding `p-6`. Padding 6 units. You can use `p-?` with any value.

Style the text. 

```HTML
<div class="bg-white rounded-xl shadow-lg p-6">
	<div>
	<div class="text-xl font-medium text-black">ChitChat</div>
		<p class="">You have a new message!</p>
	</div>
</div>
```

- `text-xl` font size xl 
- `font-medium` font-weight medium
- `text-black` color black

Color the paragraph: `text-slate-500`

Create a rounded div. 

```HTML
<div class="bg-white rounded-xl shadow-lg p-6">
	<div class="w-20 h-20 bg-teal-400"></div>
	<div>
	<div class="text-xl font-medium text-black">ChitChat</div>
		<p class="">You have a new message!</p>
	</div>
</div>
```

This creates a div with a width and height of 20 and a background color of teal. 

w = width and h = height, p = padding and m = margin. 

I noticed I could not use any value for width and height, but many values worked. Check this: https://tailwindcss.com/docs/width

Round the corners by adding: `rounded-full`

Let's use flex to arrange the two child divs. 

```HTML
<div class="bg-white rounded-xl shadow-lg p-6 flex items-center space-x-4">
	...
</div>
```

- `flex` this is now a flex container
- `items-center` like align-items center (applies to the cross axis!)
- `space-x-4` adds space on the x-axis the number (4) is the units

Those units are a little mysterious. Check the docs to see the scale: https://tailwindcss.com/docs/customizing-spacing

- '1': '8px',
- '2': '12px',
- '3': '16px',
- '4': '24px',
- '5': '32px',
- '6': '48px',

Add another div inside the teal div. 

```HTML
...
<div class="w-20 h-20 bg-teal-400 rounded-full">
	<div class=""></div>
</div>
...
```

Add the following to the inner div: 

- `w-10` width of 10 units (half the width of the parent)
- `h-10` height of 10 units (half the height of the parent)
- `bg-red-400` background color red 400 value
- `rounded-full` border-radius 50%

Put the red circle in the center with: 

```HTML
...
<div class="w-20 h-20 bg-teal-400 rounded-full flex items-center justify-center">
	<div class="w-10 h-10 bg-red-400 rounded-full"></div>
</div>
...
```

- `flex` display: flex
- `justify-center` justify-content: center
- `items-center` align-items: center

The tags are getting pretty long but the amount of code we write is shorter than what would have been written in CSS. The code reads pretty well. 

## Try Tailwind with React

Check out the docs: https://tailwindcss.com/docs/guides/create-react-app

Open a React project, or create a new project. 

```
npm install -D tailwindcss
npx tailwindcss init
```

Edit the `tailwind.config.js`

```JS
/** @type {import('tailwindcss').Config} */
module.exports = {
	content: [
		"./src/**/*.{js,jsx,ts,tsx}",
	],
	theme: {
		extend: {},
	},
	plugins: [],
}
```

Edit `src/index.css`. Delete everything and replace with: 

```
@tailwind base;
@tailwind components;
@tailwind utilities;
```

You should delete your other stylesheets or move them out of the src directory temporarily. 

```
npm start
```

Now do all of the things you did earlier but this time in your react components. For example: 

```JS
<h1 className="text-3xl font-bold underline">
	Hello, world!
</h1>
```

Use the code hints and the docs to help you out!
