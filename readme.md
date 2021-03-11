# How to sub-theme Olivero.
Technically Olivero does not support sub-theming, in this document I'll walk you through copying a Olivero into a new theme and making changes to the CSS and JavaScript.

## Why no sub-theming of Olivero?
Olivero isn't quite stable in Drupal 9. We're still making lots of changes to the markup, which can screw up any CSS overrides that you have in place. Even after reaching stable, we likely won't support sub-theming immediately because of various non-critical technical debt issues that we want to fix.

## Steps to copy Olivero into a new theme.
Instead of sub-theming, we're going to copy the core theme into a new theme.

You can do this using manual steps or by using the included script.

<details>
 <summary><p style="font-size: 24px">Manually copy theme</p></summary>

   ## Copy the theme directory.

   1. Copy the `/core/themes/olivero` directory into the `/themes/` directory.
   2. Rename the files in the new theme.
      1. Change the directory name from `olivero` to the new theme name (in these example, we'll use `coco`). So rename the `olivero` directory to `coco` (Coco is my dogs name).
      2. Rename the `olivero.info.yml` file to `coco.info.yml`
      3. Rename `olivero.breakpoints.yml` file to `coco.breakpoints.yml`
      4. Rename `olivero.libraries.yml` file to `coco.libraries.yml`
      5. Rename `olivero.theme` file to `coco.theme`
      6. Rename all of the `olivero` config within the theme's `config` directory to `coco`. For example, rename `block.block.olivero_account_menu.yml` to `block.block.coco_account_menu.yml`. There are a number of files in there to rename.
      7. Rename `/src/OliveroPreRender.php` to `/src/CocoPreRender.php`.
   3. Do a global search and replace for the name. When you search and replace, be case-sensitive
      1. Search and replace `Olivero` with `Coco`.
      2. Search and replace `olivero` with `coco`.


   ## Copy the CSS and JavaScript compilation scripts.
   Copy all of the files this repository into the new theme. These scripts will enable you to make change in the source files and have them compile down to regular CSS and JS.

   The most important files are:

   * `package.json` file
   * `yarn.lock` file
   * `scripts/` directory - this contains the CSS and JS compilation scripts.

</details>

<details>
 <summary><p style="font-size: 24px">Use the provided script to copy the theme</p></summary>
1. Copy this repository into the Drupal's `/themes/` directory.
2. Rename this directory into the your themes name.
3. Use the terminal to `cd` into the theme's directory.
4. run `sh ./build.sh` to start the process to generate the theme.
5. Enter the name of the theme when prompted (example: `Mytheme`).
 </details>

## Enable the new theme.
You should now see the new `Coco` theme listed under `appearance > themes`.
1. Set a theme other than Olivero to be the default theme.
2. Uninstall the Olivero theme.
3. Install `Coco` and set it to be the default.

Enable it. It should look exactly like the default core Olivero theme.

## Install Node dependencies.
First make sure you have [Node](https://nodejs.org/en/download/), and [Yarn](https://classic.yarnpkg.com/en/docs/install/) installed. Then run `yarn install` to install the dependencies.

## Make a change to the CSS styles.
Within the theme you'll notice both regular `*.css` files and also `*.pcss.css` files. The files that you want to modify are the `*.pcss.css` files. These PostCSS files will be made into browser-compatible CSS by running the compilation scripts.

1. Open up the `/css/base/variables.pcss.css`.
2. Change some of the blue color's hex values down near line 132.
3. To generate the CSS run `yarn build:css`.

Note you can also watch CSS by running `yarn watch:css`.

See the scripts section within the `package.json` file for more commands.

## Make a change to the JavaScript.
Similar to CSS, there are two JavaScript files for each file:

* `*.es6.js` - These are the files that you will modify. They are written using modern JavaScript.
* `*.js` - These are the IE11 compatible JavaScript files generated by running the compilation scripts. Do not directly modify these files.

You compile changes to the JavaScript files by running `yarn watch:js` and `yarn build:js`.

## How to remove IE11 (and Opera Mini support).
Internet Explorer 11 and Opera Mini do not support CSS Variables (aka CSS Custom properties). This results in making new features (such as dark mode) very hard to implement. To remove support for these browsers.

1. Remove `"last 1 Explorer version",` from line 95 in `package.json`.
2. Remove `"last 1 OperaMini version",` from line 99 in `package.json`.
3. Search the codebase and remove instances of importing the `variables.pcss.css` file. Examples:
   1. `@import "../base/variables.pcss.css";` from `css/components/action-links.pcss.css`
   2. `@import "../../base/variables.pcss.css";` from `css/components/navigation/nav-button-mobile.pcss.css`
4. Run `yarn build` to regenerate the compiled CSS and JavaScript.
