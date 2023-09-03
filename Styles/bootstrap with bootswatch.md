### Using Bootstrap in Spring Application (with bootswatch)

- step 1: go to [Bootswatch](https://bootswatch.com/)
- step 2: pick a theme you like
- step 3: download the theme.css file
- step 4: add the theme.css file (in spring application) into
```
-src(📂)
    - main (📂)
        - resources (📂)
            - static (📂)
                - css (📂)
                    - theme.css(📃)
```
- step 5: add the theme.css into your .jsp file with this link:
```html
<link rel="stylesheet" href="/css/theme.css"/>
```
- step 6: enjoy bootstrap custom theme