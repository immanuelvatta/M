# Tailwind (daisy ui) for Spring APP

- add the following to your .jsp file to import tailwind
```jsp
<head>
    <link rel="stylesheet" href="style.css">
    <script src="https://cdn.tailwindcss.com"></script>
</head>
```

### Install daisyui
- To install DaisyUI enter the following command
```bash
npm i -D daisyui@latest
```

### Create Tailwind config file
- file should be in the project file (same level as pom.xml)
    - file name should be `tailwind.config.js`
      - inside the `tailwind.config.js` file write this
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
    content: ["./src/main/webapp/WEB-INF/**/*.{html,js,jsp}"],
    theme: {
        extend: {},
    },
    daisyui: {
        themes: ["lofi", "dracula"],
        // can populate more themes if you like
    },
    plugins: [require("daisyui")],
}
```
-   you can add more themes or plugins
### Create static css files
- ##### main.css and style.css
  - In your main.css file add the following
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```
  - The contents in your style.css will get generated once you run this command 
  - 
### tailwind script after you modify .jsp

#### In your .jsp file add classes to your tags (with proper daisyui class names)

-   example:
```html
<p class="text-yellow-400">Lorem ipsum dolor sit amet consectets.</p>
```
- after you save your .jsp file, you need to run this command

```bash
npx tailwindcss -i src/main/resources/static/main.css -o src/main/resources/static/style.css
```
Add more classes to mix and match your page style