# Craft CMS Starter Project

This is an opinionated Craft CMS starter project using DDev for local development, Tailwind CSS, Alpine Js, and Laravel Mix.

to install this package run `composer create-project createsean/craft-starter .`

---

## Local Development

Set up your local development, if you are using DDev for local development then everything should just work for you. If you are **NOT** using DDEV for local development skip this part and set up local development however you normally do (Valet, Mamp, etc), be sure to import the seed database `db.sql.gz`

1. open .ddev/config.yaml and update line 15 to use the port you want. Must be unique to all ddev sites on your local computer
2. open .ddev/config.yaml and update php version (line 4) and mysql_version (line 11) if needed
3. update dotenv variables, especially SITE_NAME, PRIMARY_SITE_URL, SITE_PATH, ASSET_BASE_URL and fill in the missing details
4. Run `ddev start` and the site should start up.
5. run `ddev import-db --src=db.sql.gz` this will import the seeder database with settings, channels, etc.
6. run `ddev launch access` will open the Craft CP
7. To access the db from your host machine run `ddev describe` and you'll get the connection details needed

Login: `cc_admin`
Password: `letmein`

8. after logging in be sure to **update your password**

## To Do

* add more default templating i.e. main-nav with mobile hamburger

## Build Process

Images and svg files should be copied to src/img and src/img/svg. When running `npm run production` these will then be optimized and copied to /public/assets/images and /public/assets/images/svg respectively (if you don't want to run production, copy files to both locations)

You will need [NodeJS](https://nodejs.org/en/) version 14+. YOu can either update to 14+ or if you need multiple versions of node install the Node Version Manager [Windows](https://github.com/nvm-sh/nvm) / [Mac](https://github.com/coreybutle/nvm-windows).

1. run `npm install` or `npm i`

Add any scripts or css you need by running `npm install <package-name> --save-dev`
You can then have the required javascript or css files combined and minimized by adding paths to the correct files in `webpack.mix.js` on line 64-70(js) or line 74-78(css). when you run `npm run watch` everything will be combined and output to `/public/assets/js` or `public/assets/css`

2. in `webpack.mix.js` replace all instances of `https://craft-starter.ddev.site` with your local domain
3. run `npm run watch` to have laravel mix compile tailwind, set up browser sync. and combine scripts.

### Production

when you are ready to deploy your code run `npm run production` to optimize images in `/src/img/` optimized images will be output in `/public/assets/images`

this will also run the critical css task which you can configure at line 118 by adding in an array of urls and templates

```javascript
  urls: [{
      url: '',
      template: 'home'
    },
    {
      url: 'contact',
      template: 'singles/contact'
    },
  ],

```

---

## Tailwind

There is an empty tailwind.config.js file at the root of the repository. Add config settings as necessary but I do have some conventions that I use on all projects

For colors use `colornameBrand` where colorname is `red`, `blue`, or whatever the color is. and for font family use the name of the font. Below you can seen an example taken from another project in the extend key.


```javascript
colors: {
  redBrand: {
    light: '#fce9e8',
    default: '#de242b',
    dark: '#990e3d',
  },
  grayBrand: {
    light: '#f2f2f2',
    default: '#637a84',
  },
  textBrand: {
    light:'#334960',
    default: '#3a4250',
  },
  blackBrand: '#041e26',
},
fontFamily: {
  karla: ['Karla', 'sans-serif'],
  montserrat: ['Montserrat', 'serif'],
},
```


## Craft Plugins

The following plugins are installed and ready to be used on the site. I prefer to keep plugin usage to a **minimum** so do **not** install a plugin unless absolutely necessary. If it can be done natively, it should be done natively.

1. Environment Label - adds a color bar across the control panel indicating current environment
2. Imager X - image transforms
3. Minify - minifies html/css/js on production
4. Redactor - Rich Text Editor
5. Retcon - extra twig filters
6. SEOmatic - used for all SEO.
7. Template Comments - adds template comments in local dev to make finding templates easy
8. User Manual - in CP user manual


## Templates

Create the static templates in templates/static and then link to each template in the index.twig file.

* You can see an example template set up already.
* add any partials to the templates/_includes

## Image Macro

**lazyLoaded Image/bgImages, optimized with Imager-x**

There is a macro available to resize images and use focal points. You can do that bu following these instructions:


1. import marco in your template:
  ```{% import '_macros' as macros %}```
2. set image to run through macro e.g.:
  ```{% set image = entry.image.one() %}```
3. set options in template, or pass without options for defaults:
  ```
  {% set options = {
    sizes: [
      { width: 640, height: 640 },
      { width: 400, height:400 },
    ],
    alt: block.image1Caption ?? image.title ,
    class: '',
    lazy: true,
    mode: 'crop',
    quality: 80,
  } %}
  ```

 4. execute macro in template - this outputs the image tag:
  ```{{ macros.LazyFocusImager(image, options) }}```