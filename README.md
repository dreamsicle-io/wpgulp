# WP Theme Assets 

A simple, zero configuration Gulp build &amp; lint setup for developing modern WordPress themes.

This package is built entirely on Gulp 4, and will lint, build and optimize `.js` and `.scss` assets, generate `.pot` language localization files, and optimize images. This package will also create the required `style.css` file and its formatted header comment, as well as the required, repository-friendly `README.md` file with all it's content; all generated by the theme root's `package.json` data. 

The package also includes a php class named `WP_Theme_Assets` in `includes/class-wp-theme-assets.php`. This class beautifully handles all script and stylesheet enqueues of built assets automatically. 

## Getting Started

In order to give complete control of the entire setup, this package comes "ejected," and is not abstracted into an npm module. This means that the theme has full control over all dependencies, scripts, Gulp tasks, and linting configurations. This, however, means that using this tool requires downloading it and dropping some files into a WordPress theme's root directory. 

### 1. Download

Download the latest release as a `.zip` file and extract the contents locally.

### 2. Include Files

Place all the files and directories inside the `/package` directory into the theme's root.

> **Note about includes:** If the theme already has an `/inc` or an `/includes` directory, add `class-wp-theme-assets.php` there.

> **Note about licenses:** If the theme already has a `LICENSE` file, ignore the `LICENSE` from this package. If the theme's license is not GPL-3.0, make sure the correct license is listed in `package.json`.

### 3. Install Dependencies

Once the files have been included in the theme, it's time to install the dependencies.

**From the command line, enter the directory that contains the theme being developed (the same directory where this package's files were dropped).**

```shell
cd path/to/theme
```

**Run the install command:**

```shell
npm install
```

> **Note:** This will install all dependencies and log the command's progress. On some systems, logs may show "Skipping optional dependency" messages; these may be ignored.

### 4. Create the first build

Now that all files have been included, and all dependencies have been installed, it is time to create the first build.

**From the theme's root, run the build command:**

```shell
npm run build
```

> **Note:** This will create a directory at `assets/dist` and build all `.css` and `.js` files, as well as optimize all images. This will also create a `languages` directory and place a built `.pot` file inside of it. Lastly, this will build the required `style.css` and `README.md` files in the theme root.

### 5. Initialize WP_Theme_Assets Class

The included `WP_Theme_Assets` class, found at `includes/class-wp-theme-assets.php`, beautifully handles all script and stylesheet enqueues of built assets automatically. It also loads the theme textdomain and registers the languages directory.

**Place the following snippet in the theme's `functions.php` file:**

```php
require get_template_directory_uri() . '/includes/class-wp-theme-assets.php';

function wp_theme_assets_init() {
    WP_Theme_Assets::init();
}

add_action( 'after_setup_theme', 'wp_theme_assets_init', 10 );
```

## Development Commands

The included development commands, found on the `scripts` key in the `package.json` file, control all top level commands needed for this setup to function properly.

### Install

This command will install all dependencies listed on the `devDependencies` key in the `package.json` file. This will generate a `package-lock.json` file and a `node_modules` directory in the theme root, both of which are ignored in the `.gitignore` file by default.

```shell
npm install
```

### Start

This command will start the `default` task, which runs the `lint`, `build`, and `watch` tasks in series.

```shell
npm start
```

### Build

This command will start the `build` task, which runs the `build:package`, `build:pot`, `build:sass`, `build:js`, and `build:images` tasks in series.

```shell
npm run build
```

### Watch

This command will start the `watch` task, which watches for changes in source files and reruns relevant building and linting tasks when changes are detected.

```shell
npm run watch
```

### Clean

This command will start the `clean` task, which will delete all built assets including the `assets/dist` directory, as well as `languages/{textdomain}.pot`, `REAMDE.md`, and `style.css`.

```shell
npm run clean
```

### Lint

This command will start the `lint` task, which will lint SASS files in `src/sass` according to `.sasslintrc` file; and JS files in `src/js` according to the `.eslintrc` file.

```shell
npm run lint
```

## File structure 

This gulp setup expects that the project root's `package.json` is setup properly, and that an opinionated file structure is followed. The localization tasks are handled by the built in WordPress localization functions in all theme `.php` files, but the source assets should be structured accordingly.

### Source Files 

``` 
root 
―――― /assets 
―――― ―――― /src 
―――― ―――― ―――― /js 
―――― ―――― ―――― ―――― /modules 
―――― ―――― ―――― ―――― site.js 
―――― ―――― ―――― ―――― admin.js 
―――― ―――― ―――― ―――― customizer-preview.js 
―――― ―――― ―――― ―――― customizer-controls.js 
―――― ―――― ―――― /sass 
―――― ―――― ―――― ―――― /modules 
―――― ―――― ―――― ―――― site.scss 
―――― ―――― ―――― ―――― admin.scss 
―――― ―――― ―――― ―――― editor.scss 
―――― ―――― ―――― ―――― customizer-preview.scss 
―――― ―――― ―――― ―――― customizer-controls.scss 
―――― ―――― ―――― /images 
―――― ―――― ―――― ―――― test-icon.svg 
``` 

### Build Files 

``` 
root 
―――― /assets 
―――― ―――― /dist 
―――― ―――― ―――― /js 
―――― ―――― ―――― ―――― site.min.js 
―――― ―――― ―――― ―――― site.min.js.map 
―――― ―――― ―――― ―――― admin.min.js 
―――― ―――― ―――― ―――― admin.min.js.map 
―――― ―――― ―――― ―――― customizer-preview.min.js 
―――― ―――― ―――― ―――― customizer-preview.min.js.map 
―――― ―――― ―――― ―――― customizer-controls.min.js 
―――― ―――― ―――― ―――― customizer-controls.min.js.map 
―――― ―――― ―――― /css 
―――― ―――― ―――― ―――― site.min.css 
―――― ―――― ―――― ―――― site.min.css.map 
―――― ―――― ―――― ―――― admin.min.css 
―――― ―――― ―――― ―――― admin.min.css.map 
―――― ―――― ―――― ―――― editor.min.css 
―――― ―――― ―――― ―――― editor.min.css.map 
―――― ―――― ―――― ―――― customizer-preview.min.css 
―――― ―――― ―――― ―――― customizer-preview.min.css.map 
―――― ―――― ―――― ―――― customizer-controls.min.css 
―――― ―――― ―――― ―――― customizer-controls.min.css.map 
―――― ―――― ―――― /images 
―――― ―――― ―――― ―――― test-icon.svg (optimized but not renamed) 
―――― /languages 
―――― ―――― {textdomain}.pot 
―――― README.md 
―――― style.css 
```

## Testing Build

This package's `assets/src` directory comes prepopulated with all expected `.js` and `.scss` files. These files themselves are also prepopulated with small snippets intended to test the various build tasks and ensure they are all working properly. 

**Test assets included by default:** 

* A test `test-module.scss` module to test SASS processing.
* A test `test-module.js` module to test JavaScript processing. 
* A `WP_Theme_Assets::test_translations()` method to test language processing.
