<!-- MarkdownTOC autolink="true" style="ordered" autoanchor="true" -->

1. [Tailwind CSS Demo](#tailwind-css-demo)
  1. [Author](#author)
  1. [Local installation](#local-installation)
  1. [Branches](#branches)
  1. [Tailwind](#tailwind)
  1. [DIY](#diy)
    1. [Get the Tailwind Preset Installed](#get-the-tailwind-preset-installed)
    1. [Alter Mix Config](#alter-mix-config)
    1. [MDI Icons](#mdi-icons)
    1. [Configure some basic Tailwind Options - Color](#configure-some-basic-tailwind-options---color)
    1. [Configure some basic Tailwind Options - Margin & Padding](#configure-some-basic-tailwind-options---margin--padding)
  1. [Re-Build Your Assets](#re-build-your-assets)
  1. [Done!](#done)

<!-- /MarkdownTOC -->

<a id="tailwind-css-demo"></a>
# Tailwind CSS Demo

This repo is for a lightning talk for the Omaha Laravel Meetup group.

The purpose of the demo is to outline basic installation and usage of TailwindCSS in a Laravel project.

<a id="author"></a>
## Author

**Peter DeMarco**

- Twitter: @BeardAboutIt
- Github: /PeterDKC
- Currently: @LinkedIn

<a id="local-installation"></a>
## Local installation

    git clone git@github.com:PeterDKC/tailwind-demo.git
    cd tailwin-demo
    composer install
    npm install

set your dev database details in `.env`

    php artisan sprocket:make-db
    php artisan trees:seed

You should now have your databases installed, migrated, and 10 faked Trees in the database.

Set up your local development site however you usually do (e.g. valet, apache, or php artisan serve) and resolve any errors that appear in the browser.

You should see a list of Trees with create, edit, and delete buttons with browser default styling.

<a id="branches"></a>
## Branches

**master**: This branch contains a default Laravel application with the Tree model, controller, migration, and seeder. It has a set of views to view, create, edit, and delete Tree resources in the database. It has no styles applied to any of the frontend. It does include Vue.js and the MDI font set.

Once you've following the above steps you can branch off of master with:

`git checkout -b my-awesome-branch-name`

See git guides / documentation for futher options from here.

**demo-complete**: This branch contains a completed set of views with Tailwind styles applied to represent a completed site. It may / may not correspond to what was done during the live demo, but instead is a reference for what might come out of the demo.

<a id="tailwind"></a>
## Tailwind

**Installation**: https://github.com/laravel-frontend-presets/tailwindcss

**Documentation**: https://tailwindcss.com/docs/what-is-tailwind/

<a id="diy"></a>
## DIY

Below are the set of steps to get Tailwind up and running and start applying classes to the application.

<a id="get-the-tailwind-preset-installed"></a>
### Get the Tailwind Preset Installed

    git checkout -b my-branch-name
    composer install laravel-frontend-presets/tailwindcss
    php artisan preset tailwindcss
    npm install && npm run dev && npm run dev

<a id="alter-mix-config"></a>
### Alter Mix Config

Open `webpack.mix.js` and make the following changes to the last few lines of the file:

**Before:**

*(snipped)...*
```
   .tailwind()
   .purgeCss();

if (mix.inProduction()) {
  mix.version();
}
```

**After:**

*(snipped)...*
```
   .tailwind()
   .purgeCss()
   .version()
   .extract(['vue', 'axios']);
```

This instructs Webpack to always version ( cache-bust ) our frontend assets, and to make longer-lived files for seldom-changed libraries ( Vue and Axios ). The `extract()` directive is optional but is a good habit to get into.

**Note** that this requires mixing 3 javascript files into the application instead of just one:

*from `resources/views/layouts/app.blade.php` in this application:*

```
...
    <!-- Scripts -->
    <script src="{{ mix('js/manifest.js') }}"></script>
    <script src="{{ mix('js/vendor.js') }}"></script>
    <script src="{{ mix('js/app.js') }}"></script>
...
```

<a id="mdi-icons"></a>
### MDI Icons

Add the following two lines to the top of `resources/assets/css/app.css`:

```
$icon-font-path: '~mdi/fonts';
@import '~mdi/css/materialdesignicons.css';
```

This will instruct Webpack to build out the MDI icon files and package them with the application.

<a id="configure-some-basic-tailwind-options---color"></a>
### Configure some basic Tailwind Options - Color

Tailwind is fully configurable. Its default set of styles is laid out in a large JavaScript array in `tailwind.js` in your app root.

Find the `colors` section, and then the list of `green` colors. Replace them with the following:

```
  "green-darkest": "#174F1C",
  "green-darker": "#227729",
  "green-dark": "#2E9E37",
  green: "#39C645",
  "green-light": "#61D16A",
  "green-lighter": "#88DD8F",
  "green-lightest": "#B0E8B5",
```

Find the `colors` > `brand` section ( at the bottom of the color declarations ). Replace `orange` with `green` in these declarations to match our Tree theme. Example:

**Before:**

```
  get ["brand-darkest"]() {
    return this["orange-darkest"];
  },
```

**After:**

```
  get ["brand-darkest"]() {
    return this["green-darkest"];
  },
```

<a id="configure-some-basic-tailwind-options---margin--padding"></a>
### Configure some basic Tailwind Options - Margin & Padding

The list of basic Padding and Margin options contains a good default set. However you usually will want to fill in some of the skipped "steps" in these.

**Padding**

In `tailwind.js`, find the `padding` section. You'll see that steps 0-4, 6, & 8 come out of the box, stepping up `.25rem` per step. Fill in any additional options. Example:

```
  padding: {
    px: "1px",
    "0": "0",
    "1": "0.25rem",
    "2": "0.5rem",
    "3": "0.75rem",
    "4": "1rem",
    "5": "1.25rem",
    "6": "1.5rem",
    "7": "1.75rem",
    "9": "2.25rem",
    "10": "2.5rem",
    "12": "3rem",
    "16": "4rem",
    "20": "5rem",
    "24": "6rem",
  },
```

**Margin**

You'll find an identical set of out-of-the-box options in the `margin` section. Make any desired alterations.

<a id="re-build-your-assets"></a>
## Re-Build Your Assets

Don't forget that after any changes to `webpack.mix.js` or `tailwind.js`, you'll need to reubuild your frontend assets with Webpack! Changes to `taildwind.js` should be picked up automatically by `npm run watch`, but for `webpack.mix.js` alterations you probably have to stop the watch script and re-run manually.

  npm run dev

or

  npm run watch

<a id="done"></a>
## Done!

You're ready to start applying styles into the site.
