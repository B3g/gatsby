# gatsby-plugin-manifest

The web app manifest(part of the [PWA](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps) specification) enabled by this plugin allows users to add your site to their home screen(on most mobile browsers —
[see here](http://caniuse.com/#feat=web-app-manifest)). The manifest provides configuration and icons to the phone.

This plugin provides several features above and beyond the manifest to make your life easier, they as follows:

- Auto generation of required icon sizes from a single source
- [Favicon support](https://www.w3.org/2005/10/howto-favicon)
- Legacy icon support (iOS)[^1]
- [Cache busting](https://www.keycdn.com/support/what-is-cache-busting)

Each of these features has extensive configuration available so you're always in control.

## Table of Contents

1. [Spec Resources](#spec-resources)
2. [Install](#install)
3. [How to Use](#third-example)

## Install

`npm install --save gatsby-plugin-manifest`

If you're using this plugin together with [`gatsby-plugin-offline`](https://www.gatsbyjs.org/packages/gatsby-plugin-offline) (recommended),
this plugin should be listed _before_ the offline plugin so that it can cache
the created `manifest.webmanifest`.

## How to use

### Configure plugin and add Web App Manifest settings (Required)

```js
// in gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `GatsbyJS`,
        short_name: `GatsbyJS`,
        start_url: `/`,
        background_color: `#f7f0eb`,
        theme_color: `#a2466c`,
        display: `standalone`,
      },
    },
  ],
}
```

For more information on configuring your web app [see here](https://developers.google.com/web/fundamentals/web-app-manifest/).

### Configure icons and their generations (Required)

There are three modes in which icon generation can function: automatic, hybrid, and manual(disabled). These modes can affect other configurations defaults.

- Automatic - Generate a pre-configured set of icons from a single source icon.

  - Favicon - yes
  - Legacy Icon Support - yes
  - Cache busting - yes

- Hybrid - Generate a manually configured set of icons from a single source icon.
  - Favicon - yes
  - Legacy Icon Support - yes???
  - Cache busting - yes
- Manual - Don't generate or pre-configure any icons.
  - Favicon - no
  - Legacy Icon Support - no
  - Cache busting - no

#### Automatic mode configuration

Add the following line to the plugin options

```js
  icon: `src/images/icon.png`, // This path is relative to the root of the site.
```

When in automatic mode the following json array is injected into the manifest configuration you provide and the icons are generated from it. The source icon you provide should be at least as big as the largest icon being generated.

```json
[
  {
    "src": "icons/icon-48x48.png",
    "sizes": "48x48",
    "type": "image/png"
  },
  {
    "src": "icons/icon-72x72.png",
    "sizes": "72x72",
    "type": "image/png"
  },
  {
    "src": "icons/icon-96x96.png",
    "sizes": "96x96",
    "type": "image/png"
  },
  {
    "src": "icons/icon-144x144.png",
    "sizes": "144x144",
    "type": "image/png"
  },
  {
    "src": "icons/icon-192x192.png",
    "sizes": "192x192",
    "type": "image/png"
  },
  {
    "src": "icons/icon-256x256.png",
    "sizes": "256x256",
    "type": "image/png"
  },
  {
    "src": "icons/icon-384x384.png",
    "sizes": "384x384",
    "type": "image/png"
  },
  {
    "src": "icons/icon-512x512.png",
    "sizes": "512x512",
    "type": "image/png"
  }
]
```

The automatic mode is the easiest option for most people.

#### Hybrid mode configuration

Add the following line to the plugin options

```js
icon: `src/images/icon.png`, // This path is relative to the root of the site.
icons: [
  {
    src: `/favicons/android-chrome-192x192.png`,
    sizes: `192x192`,
    type: `image/png`,
  },
  {
    src: `/favicons/android-chrome-512x512.png`,
    sizes: `512x512`,
    type: `image/png`,
  },
], // Add or remove icon sizes as desired
```

If you want to include more or fewer sizes, then the hybrid option is for you. Like automatic mode, you should include a high resolution icon to generate smaller icons from. But unlike automatic mode, you provide the `icons` array config and icons are generated based on the sizes defined in your config. Here's an example `gatsby-config.js`:

The hybrid option allows the most flexibility while still not requiring you to create all icon sizes manually.

#### Manual mode configuration

Add the following line to the plugin options

```js
icons: [
  {
    src: `/favicons/android-chrome-192x192.png`,
    sizes: `192x192`,
    type: `image/png`,
  },
  {
    src: `/favicons/android-chrome-512x512.png`,
    sizes: `512x512`,
    type: `image/png`,
  },
], // Add or remove icon sizes as desired

```

In the manual mode, you are responsible for defining the entire web app manifest and providing the defined icons in the [static](https://www.gatsbyjs.org/docs/static-folder/) folder. Only icons you provide will be available. There is no automatic resizing done for you.

### Feature configuration (Optional)

### Disable Legacy Icons

iOS 11.3 added support for the web app manifest spec. Previous iOS versions won't recognize the icons defined in the webmanifest and the creation of `apple-touch-icon` links in `<head>` is needed. This plugin creates them by default. If you don't want those icons to be generated you can set the `legacy` option to `false` in plugin configuration:

```js
// in gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `GatsbyJS`,
        short_name: `GatsbyJS`,
        start_url: `/`,
        background_color: `#f7f0eb`,
        theme_color: `#a2466c`,
        display: `standalone`,
        icon: `src/images/icon.png`,
        legacy: false, // this will not add apple-touch-icon links to <head>
      },
    },
  ],
}
```

### Disable Favicon

Excludes `<link rel="shortcut icon" href="/favicon.png" />` link tag to html output. You can set `include_favicon` plugin option to `false` to opt-out of this behavior.

```js
// in gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `GatsbyJS`,
        short_name: `GatsbyJS`,
        start_url: `/`,
        background_color: `#f7f0eb`,
        theme_color: `#a2466c`,
        display: `standalone`,
        icon: `src/images/icon.png`, // This path is relative to the root of the site.
        include_favicon: false, // This will exclude favicon link tag
      },
    },
  ],
}
```

## Disabling or changing "[Cache Busting](https://www.keycdn.com/support/what-is-cache-busting)"

Cache Busting allows your updated icon to be quickly/easily visible to your sites visitors. HTTP caches could otherwise keep an old icon around for days and weeks. Cache busting can only done in 'automatic' and 'hybrid' modes.

Cache busting works by calculating a unique "digest" of the provided icon and modifying links or file names of generated images with that unique digest. If you ever update your icon, the digest will change and caches will be busted.

**Options:**

- **\`query\`** - This is the default mode. File names are unmodified but a URL query is appended to all links. e.g. `icons/icon-48x48.png?digest=abc123`

- **\`name\`** - Changes the cache busting mode to be done by file name. File names and links are modified with the icon digest. e.g. `icons/icon-48x48-abc123.png` (only needed if your CDN does not support URL query based cache busting)

- **\`none\`** - Disables cache busting. File names and links remain unmodified.

```js
// in gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `GatsbyJS`,
        short_name: `GatsbyJS`,
        start_url: `/`,
        background_color: `#f7f0eb`,
        theme_color: `#a2466c`,
        display: `standalone`,
        icon: `src/images/icon.png`,
        cache_busting_mode: `none`, // `query`(default), `name`, or `none`
      },
    },
  ],
}
```

### Removing `theme-color` meta tag

By default a `<meta content={theme_color} name="theme-color" />` tag is inserted into the html output. This can be problematic if you want to programmatically control that tag (e.g. when implementing light/dark themes in your project). You can set `theme_color_in_head` plugin option to `false` to opt-out of this behavior.

```js
// in gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `GatsbyJS`,
        short_name: `GatsbyJS`,
        start_url: `/`,
        background_color: `#f7f0eb`,
        theme_color: `#a2466c`,
        display: `standalone`,
        icon: `src/images/icon.png`,
        theme_color_in_head: false, // This will avoid adding theme-color meta tag.
      },
    },
  ],
}
```

## Enable CORS using `crossorigin` attribute

Add a `crossorigin` attribute to the manifest `<link rel="manifest" crossorigin="use-credentials" href="/manifest.webmanifest" />` link tag.

You can set `crossOrigin` plugin option to `'use-credentials'` to enable sharing resources via cookies. Any invalid keyword or empty string will fallback to `'anonymous'`.

You can find more information about `crossorigin` on [MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/CORS_settings_attributes).

```js
// in gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `GatsbyJS`,
        short_name: `GatsbyJS`,
        start_url: `/`,
        background_color: `#f7f0eb`,
        theme_color: `#a2466c`,
        display: `standalone`,
        icon: `src/images/icon.png`,
        crossOrigin: `use-credentials`, // `use-credentials` or `anonymous`
      },
    },
  ],
}
```

## Web app manifest resources

This article from the Chrome DevRel team is a good intro to the web app
manifest—https://developers.google.com/web/fundamentals/engage-and-retain/web-app-manifest/

For more information see the w3 spec https://www.w3.org/TR/appmanifest/ or Mozilla Docs https://developer.mozilla.org/en-US/docs/Web/Manifest.

[^1]: Currently this is only for older versions of [iOS Safari](https://developer.apple.com/library/archive/documentation/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html). IE is the only other browser that doesn't support the web app manifest, and it's market share is so small no one has contributed support.
