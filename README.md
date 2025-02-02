![](https://repository-images.githubusercontent.com/337066468/8a3e8e34-3d48-4f0f-bb07-b02e4391f53c)

<p align='center'>
<a href='https://www.npmjs.com/package/nuxt-webpack-optimisations'>
<img src='https://img.shields.io/npm/v/nuxt-webpack-optimisations?color=0EA5E9&label='>
</a>
<a href="https://www.npmjs.com/package/nuxt-webpack-optimisations" target="__blank"><img alt="NPM Downloads" src="https://img.shields.io/npm/dm/nuxt-webpack-optimisations?color=0EA5E9&label="></a>
<a href='https://github.com/harlan-zw/nuxt-webpack-optimisations/actions/workflows/test.yml'>
<img src='https://github.com/harlan-zw/nuxt-webpack-optimisations/actions/workflows/test.yml/badge.svg' >
</a>
</p>

<p align='center'>Webpack Build Time Optimisations for Nuxt.js! ⚡️<br>
<sup><em>Instantly speed up your Nuxt.js webpack build time.</em></sup>
</p>

<p align="center">
<table>
<tbody>
<td align="center">
<img width="2000" height="0" /><br>
<i>Status:</i> <b>Stable v2 ✅ , bridge ✅, v3 ⚠️</b><br>
<sub>Made possible by my <a href="https://github.com/sponsors/harlan-zw">Sponsor Program 💖</a><br> Follow me <a href="https://twitter.com/harlan_zw">@harlan_zw</a> 🐦</sub><br>
<img width="2000" height="0" />
</td>
</tbody>
</table>
</p>

> Previously: "nuxt-build-optimisations", see <a href="https://github.com/harlan-zw/nuxt-webpack-optimisations/releases/tag/2.0.0">migration notes</a>.

### Can't use Vite with Nuxt yet?

> i.e  [Nuxt Vite](https://vite.nuxtjs.org/) or  [Nuxt 3](https://v3.nuxtjs.org/)

Truly sad... But I do have some good news. While you won't be able to achieve
instant app starts anytime soon, `nuxt-webpack-optimisations` can get things snappy again.

## Webpack Optimisations

`nuxt-webpack-optimisations` is a collection of webpack config changes that will let you speed up your build times and audit them.

By making smarter and riskier assumptions on how you want to run your environment in development, this module has been benchmarked
to reduce your build time by **~50% when cold ☃** , **~95%+ when hot 🔥** (using hardsource).

### How risky are we talking

The riskier optimisations are enabled only on development and relate to over caching, which is always easy to fix with a good old `rm -rf node_modules/.cache` 💩.

✔️ This module has been tested to cause no issues in production environments.

## Features

Features are enabled by their risk profile. The risk profile is the likelihood of issues coming up.

**Tools**

- [speed-measure-webpack-plugin](https://github.com/stephencookdev/speed-measure-webpack-plugin)


**Always**
- Nuxt config [build.cache](https://nuxtjs.org/docs/2.x/configuration-glossary/configuration-build#cache) enabled
- Nuxt config [build.parallel](https://nuxtjs.org/docs/2.x/configuration-glossary/configuration-build#parallel) enabled  - _requires `risky: true`_
- webpacks [best practices](https://webpack.js.org/guides/build-performance/)  for performance

**Dev**
- [esbuild](https://esbuild.github.io/) replaces `babel-loader`
- [esbuild](https://esbuild.github.io/) replaces `ts-loader` 
- [postcss-preset-env](https://github.com/csstools/postcss-preset-env) is disabled
- `file-loader` replaces `url-loader`
- Nuxt config [build.hardsource](https://nuxtjs.org/docs/2.x/configuration-glossary/configuration-build#hardsource) enabled - _requires `risky: true`_

**Production**
- [esbuild](https://esbuild.github.io/) replaces [Terser](https://github.com/terser/terser) for minification 


### Compatibility

- ✔️ Nuxt v2
- ✔️ Nuxt bridge
- ⚠ Nuxt v3 _Note: Vite needs to be disabled. You probably don't need this module._


## Setup

Install the module.

```bash
yarn add nuxt-webpack-optimisations
# npm i nuxt-webpack-optimisations
```

Within your `nuxt.config.ts` or `nuxt.config.js`
```js
buildModules: [
  'nuxt-webpack-optimisations',
],
```

### Typescript

For Nuxt config typescript support, add the module within your `tsconfig.json`.

```json

{
  "compilerOptions": {
    "types": [
      "nuxt-webpack-optimisations"
    ]
  }
}
```


---

## Usage

All non-risky features are enabled by default, only `hardsource` and `parallel` are disabled.

If you'd like to get more performance than the default you can try

```js
// nuxt.config.ts
export default {
  webpackOptimisations: {
    // hard source is the riskiest, if you have issues don't enable it
    hardSourcePlugin: process.env.NODE_ENV === 'development',
    parallelPlugin: process.env.NODE_ENV === 'development',
  }
}
```

Note: It's recommended to avoid running risky in non-development environments. Caching in CI environments can lead to issues.

### Something isn't working

A lot of the speed improvements are from heavy caching, if you have any issues the first thing you should
do is clear your cache.

```shell
# Linux / Mac
rm -rf node_modules/.cache

# windows
rd /s  "node_modules/.cache"
```

If you'd like to see what features are running you can enable the debug mode.

```js
// nuxt.config.ts
export default {
  webpackOptimisations: {
    debug: true
  }
}
```


## Configuration


### Features

*Type:*  `object`

*Default:* Non-risky features enabled.

You can disable features if you'd like to skip optimisations.


```js
export default {
  webpackOptimisations: {
    features: {
      // Note: just an example of keys, these are all keys and their default
      postcssNoPolyfills: true,
      esbuildLoader: true,
      esbuildMinifier: true,
      imageFileLoader: true,
      webpackOptimisations: true,
      cacheLoader: true,
      hardSourcePlugin: false,
      parallelPlugin: false,
    }
  }
}
```

### esbuildLoaderOptions

*Type:*  `object`

*Default:*
```javascript
export default {
  client: {
    target: 'es2015',
  },
  server: {
    target: 'node14',
  },
  modern: {
    target: 'es2015',
  },
}
```

See [esbuild-loader](https://github.com/privatenumber/esbuild-loader).

### esbuildMinifyOptions

*Type:*  `object`

*Default:*
```javascript
export default {
  client: {
    target: 'es2015',
  },
  server: {
    target: 'node14',
  },
  modern: {
    target: 'es2015',
  },
}
```

See [esbuild-loader](https://github.com/privatenumber/esbuild-loader).

### Measure

*Type:* `boolean` or `object`

*Default:* `false`

When measure is enabled with true (options or environment variable), it will use the `speed-measure-webpack-plugin`.

If the measure option is an object it is assumed to be [speed-measure-webpack-plugin options](https://github.com/stephencookdev/speed-measure-webpack-plugin#options).

```js
webpackOptimisations: {
  measure: {
    outputFormat: 'humanVerbose',
    granularLoaderData: true,
    loaderTopFiles: 10
  }
}
```

You can use an environment variable to enable the measure as well.

**package.json**

```json
{
  "scripts": {
    "measure": "export NUXT_MEASURE=true; nuxt dev"
  }
}
```

Note: Some features are disabled with measure on, such as caching.

### Measure Mode

*Type:* `client` | `server` | `modern` | `all`

*Default:* `client`

Configure which build will be measured. Note that non-client builds may be buggy and mess with HMR.

```javascript
webpackOptimisations: {
  measureMode: 'all'
}
```

### Gotchas

#### Vue Property Decorator / Vue Class Component

Your babel-loader will be replaced with esbuild, which doesn't support class decorators in js.

You can either migrate your scripts to typescript or disabled the esbuild loader.

**Disable Loader**
```js
webpackOptimisations: {
  features: {
    esbuildLoader: false
  }
}
```

**Migrate to TypeScript**

*tsconfig.json*
```json
{
  "experimentalDecorators": true
}
```

```vue
<script lang="ts">
import Vue from 'vue'
import Component from 'vue-class-component'

@Component
export default class HelloWorld extends Vue {
  data () {
    return {
      hello: 'test'
    }
  }
}
</script>
```

## Credits

- https://github.com/galvez/nuxt-esbuild-module

## Sponsors

<p align="center">
  <a href="https://cdn.jsdelivr.net/gh/harlan-zw/static/sponsors.svg">
    <img src='https://cdn.jsdelivr.net/gh/harlan-zw/static/sponsors.svg'/>
  </a>
</p>

## License

MIT License © 2022 [Harlan Wilton](https://github.com/harlan-zw)
