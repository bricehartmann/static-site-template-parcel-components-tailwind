# Static Site Template - Parcel, PostHTML Components, Tailwind
This repository is a template for a static site using Parcel 2, PostHTML Components, and Tailwind.

## Stack
* [Parcel 2](https://parceljs.org/)
* [PostHTML Components](https://github.com/posthtml/posthtml-components)
* [TailwindCSS](https://tailwindcss.com/) via [PostCSS](https://github.com/postcss/postcss/)

# System requirements
* Node & NPM

## Getting started
1. Create a new repository on GitHub using this one as a template
2. Clone your new repository
3. Install dependencies with `npm install`

## Usage

### Local development
After changes, build the project and run a local HTTP server using:
```
npm run dev
```
Use Ctrl-C to exit the server.

Development mode does **NOT** optimize/minify files or bust the browser cache.

### Production
Build the project for production using:
```
npm run build
```
This will output the resulting files to the `dist/` directory.

### Copying unreferenced files upon build
Files that are not referenced through a page, such as `robots.txt`, must be copied to the `dist/` directory at build time.

This can be done automatically by adding the name of each file that should be copied from the `src/` directory to the `.local/copy-on-build.txt` text file.

### Pages
Pages are defined as `html` files within the `src/` directory or a subdirectory, excluding the `layouts` and `components` subdirectories.

The [parcel-optimizer-friendly-urls](https://github.com/vseventer/parcel-optimizer-friendly-urls) plugin is used to make "friendly" URLs, serving the default `index.html` document within a subdirectory.

For example, to make a page available at the URL `your-domain.com/work`, create an `index.html` file within the `src/work/` directory.

### File references
All files should be referenced with an absolute path from the root project directory.

As part of the build process, these references will be updated automatically.

For example:
* `/src/css/main.css`
* `/src/js/main.js`
* `/src/some-subdirectory/my-page.html`

### Layout and Component Basics

#### Defining layouts and components
To define a layout or component that can be reused, create an `html` file in the `src/layouts` or `src/components` directory.

#### Using layouts and components
Use a defined layout or component by prefixing the layout/component name with `x-` in an element tag.

```html
<x-some-component>
    My content here
</x-some-component>
```

#### Rendering content
Within a layout or component, use the `yield` element to render content defined where the layout or component is used.
```html
<yield></yield>
```

#### Layout and component attributes
All attributes specified when using a layout or component will overwrite the attributes defined in the layout/component definition.

An exception is the `class` attribute, which will be merged with the `class` attribute defined in the layout/component definition (if any).

For example, consider the following button link component:
```html
<a class="px-2 py-1">
    <yield></yield>
</a>
```

In addition to this component being used as such:
```html
<x-button-link href="/src/index.html" class="text-white bg-blue-500 hover:bg-blue-300">
    Go home
</x-button-link>
```

The above combination will result in the following when the project is built:
```html
<a href="/src/index.html" class="px-2 py-1 text-white bg-blue-500 hover:bg-blue-300">
    Go home
</a>
```

#### Named slots
Define named slots in layouts and components using the `slot` element. This is alternative to using `yield` for when multiple sections of content should be rendered.

The following is an example of rendering the content for the `main` slot.
```html
<slot:main></slot:main>
```

The following is an example of using a component and adding content to the `main` slot.
```html
<x-some-component>
    <fill:main>
        This is my slot content.
    </fill:main>
</x-some-component>
```

#### Props
Props may be defined in layouts and components using a `script` tag with an empty `props` attribute.

The script tag should define the prop names as keys to an object set on `module.exports`.

The following shows how to define two props, named `title` and `description`, on a layout.

```html
<script props>
    module.exports = {
        title: props.title,
        description: props.description,
    }
</script>
```

Props can be rendered within a layout or component using mustache syntax `{{ }}`.

For example, to use the `title` and `description` props:
```html
<title>{{ title }}</title>
<meta name="description" content="{{ description }}">
```
