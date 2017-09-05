# @ngx-config/core [![npm version](https://badge.fury.io/js/%40ngx-config%2Fcore.svg)](https://www.npmjs.com/package/@ngx-config/core) [![npm downloads](https://img.shields.io/npm/dm/%40ngx-config%2Fcore.svg)](https://www.npmjs.com/package/@ngx-config/core)
Configuration utility for **Angular**

[![CircleCI](https://circleci.com/gh/fulls1z3/ngx-config/tree/2.x.x.svg?style=shield)](https://circleci.com/gh/fulls1z3/ngx-config)
[![coverage](https://codecov.io/github/fulls1z3/ngx-config/coverage.svg?branch=2.x.x)](https://codecov.io/gh/fulls1z3/ngx-config)
[![tested with jest](https://img.shields.io/badge/tested_with-jest-99424f.svg)](https://github.com/facebook/jest)
[![Conventional Commits](https://img.shields.io/badge/Conventional%20Commits-1.0.0-yellow.svg)](https://conventionalcommits.org)
[![Angular Style Guide](https://mgechev.github.io/angular2-style-guide/images/badge.svg)](https://angular.io/styleguide)

> Please support this project by simply putting a Github star. Share this library with friends on Twitter and everywhere else you can.

**`@ngx-config/core`** uses `APP_INITIALIZER` which executes a function when **Angular** app is initialized, and delay the
completion of initialization process until application settings have been provided.

#### NOTICE
> This *[2.x.x] branch* is intented to work with `@angular v2.x.x`. If you're developing on a later release of **Angular**
than `v2.x.x`, then you should probably choose the appropriate version of this library by visiting the *[master] branch*.

## Table of contents:
- [Prerequisites](#prerequisites)
- [Getting started](#getting-started)
  - [Installation](#installation)
	- [Examples](#examples)
	- [Related packages](#related-packages)
	- [Recommended packages](#recommended-packages)
	- [Adding `@ngx-config/core` to your project (SystemJS)](#adding-systemjs)
  - [app.module configuration](#appmodule-config)
- [Settings](#settings)
	- [Setting up `ConfigModule` to use `ConfigStaticLoader`](#setting-up-staticloader)
	- [Setting up `ConfigModule` to use `ConfigHttpLoader`](#setting-up-httploader)
	- [Setting up `ConfigModule` to use `ConfigFsLoader`](#setting-up-fsloader)
	- [Setting up `ConfigModule` to use `UniversalConfigLoader`](#setting-up-universalloader)
	- [Setting up `ConfigModule` to use `ConfigMergeLoader`](#setting-up-mergeloader)
- [Usage](#usage)
- [Pipe](#pipe)
- [License](#license)

## <a name="prerequisites"></a> Prerequisites
This library depends on `Angular v2.0.0` but it's highly recommended that you are running at least **`@angular v2.4.0`**.
Older versions contain outdated dependencies, might produce errors.

Also, please ensure that you are using **`Typescript v2.1.6`** or higher.

## <a name="getting-started"> Getting started
### <a name="installation"> Installation
You can install **`@ngx-config/core`** using `npm`
```
npm install @ngx-config/core --save
```

### <a name="examples"></a> Examples
- [ng-seed/universal] and [fulls1z3/example-app] are officially maintained projects, showcasing common patterns and best
practices for **`@ngx-config/core`**.

### <a name="related-packages"></a> Related packages
The following packages may be used in conjunction with **`@ngx-config/core`**:
- [@ngx-config/http-loader]
- [@ngx-config/fs-loader]
- [@ngx-universal/config-loader]
- [@ngx-config/merge-loader]
- [@ngx-i18n-router/config-loader]

### <a name="recommended-packages"></a> Recommended packages
The following package(s) have no dependency for **`@ngx-config/core`**, however may provide supplementary/shorthand functionality:
- [@ngx-cache/core]: provides caching features to retrieve the application settings using `non-static loaders` (`http`,
`fs`, etc.)

### <a name="adding-systemjs"></a> Adding `@ngx-config/core` to your project (SystemJS)
Add `map` for **`@ngx-config/core`** in your `systemjs.config`
```javascript
'@ngx-config/core': 'node_modules/@ngx-config/core/bundles/core.umd.min.js'
```

### <a name="appmodule-config"></a> app.module configuration
Import `ConfigModule` using the mapping `'@ngx-config/core'` and append `ConfigModule.forRoot({...})` within the imports
property of **app.module** (*considering the app.module is the core module in Angular application*).

## <a name="settings"></a> Settings
You can call the [forRoot] static method using `ConfigStaticLoader`. By default, it is configured to have no settings.

> You can customize this behavior (*and ofc other settings*) by supplying **application settings** to `ConfigStaticLoader`.

The following examples show the use of an exported function (*instead of an inline function*) for [AoT compilation].

### <a name="setting-up-staticloader"></a> Setting up `ConfigModule` to use `ConfigStaticLoader`
```TypeScript
...
import { ConfigModule, ConfigLoader, ConfigStaticLoader } from '@ngx-config/core';
...

export function configFactory(): ConfigLoader {
  return new ConfigStaticLoader({
    "system": {
      "applicationName": "Mighty Mouse",
      "applicationUrl": "http://localhost:8000"
    },
    "seo": {
      "pageTitle": "Tweeting bird"
    },
    "i18n":{
      "locale": "en"
    }
  });
}

@NgModule({
  declarations: [
    AppComponent,
    ...
  ],
  ...
  imports: [
    ConfigModule.forRoot({
      provide: ConfigLoader,
      useFactory: (configFactory)
    }),
    ...
  ],
  ...
  bootstrap: [AppComponent]
})
```

`ConfigStaticLoader` has one parameter:
- **providedSettings**: `any` : application settings

> :+1: Cool! **`@ngx-config/core`** will retrieve application settings before **Angular** initializes the app.

### <a name="setting-up-httploader"></a> Setting up `ConfigModule` to use `ConfigHttpLoader`
If you provide application settings using a `JSON` file or an `API`, you can call the [forRoot] static method using the
`ConfigHttpLoader`. By default, it is configured to retrieve **application settings** from the endpoint `/config.json`
(*if not specified*).

> You can customize this behavior (*and ofc other settings*) by supplying a **api endpoint** to `ConfigHttpLoader`.

You can find detailed information about the usage guidelines for the `ConfigHttpLoader` [here](https://github.com/fulls1z3/ngx-config/tree/master/packages/@ngx-config/http-loader).

### <a name="setting-up-fsloader"></a> Setting up `ConfigModule` to use `ConfigFsLoader`
If you provide application settings using a `JSON` file (*on the `server platform`*), you can call the [forRoot] static
method using the `ConfigFsLoader`. By default, it is configured to retrieve **application settings** from the path `/config.json`
(*if not specified*).

> You can customize this behavior (*and ofc other settings*) by supplying a **file path** to `ConfigFsLoader`.

You can find detailed information about the usage guidelines for the `ConfigFsLoader` [here](https://github.com/fulls1z3/ngx-config/tree/master/packages/@ngx-config/fs-loader).

### <a name="setting-up-universalloader"></a> Setting up `ConfigModule` to use `UniversalConfigLoader`
`UniversalConfigLoader` provides application settings to **browser**/**server** platforms.

You can find detailed information about the usage guidelines for the `UniversalConfigLoader` [here](https://github.com/fulls1z3/ngx-config/tree/master/packages/@ngx-universal/config-loader).

### <a name="setting-up-mergeloader"></a> Setting up `ConfigModule` to use `ConfigMergeLoader`
`ConfigMergeLoader` provides application settings by executing loaders in **parallel** and in **series**.

You can find detailed information about the usage guidelines for the `ConfigMergeLoader` [here](https://github.com/fulls1z3/ngx-config/tree/master/packages/@ngx-config/merge-loader).

## <a name="usage"></a> Usage
`ConfigService` has the `getSettings` method, which you can fetch settings loaded during application initialization.

When the `getSettings` method is invoked without parameters, it returns entire application configuration. However, the `getSettings`
method can be invoked using two optional parameters: **`key`** and **`defaultValue`**.

The following example shows how to read configuration settings using all available overloads of `getSettings` method.

#### anyclass.ts
```TypeScript
...
import { ConfigService } from '@ngx-config/core';

@Injectable()
export class AnyClass {
  constructor(private readonly config: ConfigService) {
    // note that ConfigService is injected into a private property of AnyClass
  }
  
  myMethodToGetUrl1a() {
    // will retrieve 'http://localhost:8000'
    let url:string = this.config.getSettings('system.applicationUrl');
  }

  myMethodToGetUrl1b() {
    // will retrieve 'http://localhost:8000'
    let url:string = this.config.getSettings(['system', 'applicationUrl']);
  }

  myMethodToGetUrl2a() {
    // will retrieve 'http://localhost:8000'
    let url:string = this.config.getSettings('system').applicationUrl;
  }

  myMethodToGetUrl2b() {
    // will retrieve 'http://localhost:8000'
    let url:string = this.config.getSettings().system.applicationUrl;
  }

  myMethodToGetUrl3a() {
    // will throw an exception (system.non_existing is not in the application settings)
    let url:string = this.config.getSettings('system.non_existing');
  }

  myMethodToGetUrl3b() {
    // will retrieve 'no data' (system.non_existing is not in the application settings)
    let url:string = this.config.getSettings('system.non_existing', 'no data');
  }
  
  myMethodToGetSeo1() {
    // will retrieve {"pageTitle":"Tweeting bird"}
    let seoSettings: string = this.config.getSettings('seo');
  }

  myMethodToGetSeo1() {
    // will retrieve {"pageTitle":"Tweeting bird"}
    let seoSettings: string = this.config.getSettings().seo;
  }
}
```

## <a name="pipe"></a> Pipe
`ConfigPipe` is used to get the application settings on the view level. Pipe can be appended to a **string** or to an
**Array<string>**.

```Html
<span id="property">{{'some.setting' | config}}</span>
<span id="property">{{['some', 'setting'] | config}}</span>
```

## <a name="license"></a> License
The MIT License (MIT)

Copyright (c) 2017 [Burak Tasci]

[master]: https://github.com/ngx-config/core/tree/master
[2.x.x]: https://github.com/ngx-config/core/tree/2.x.x
[ng-seed/universal]: https://github.com/ng-seed/universal
[fulls1z3/example-app]: https://github.com/fulls1z3/example-app
[@ngx-config/http-loader]: https://github.com/fulls1z3/ngx-config/tree/master/packages/@ngx-config/http-loader
[@ngx-config/fs-loader]: https://github.com/fulls1z3/ngx-config/tree/master/packages/@ngx-config/fs-loader
[@ngx-universal/config-loader]: https://github.com/fulls1z3/ngx-config/tree/master/packages/@ngx-universal/config-loader
[@ngx-config/merge-loader]: https://github.com/fulls1z3/ngx-config/tree/master/packages/@ngx-config/merge-loader
[@ngx-i18n-router/config-loader]: https://github.com/fulls1z3/ngx-i18n-router/tree/master/packages/@ngx-i18n-router/config-loader
[@ngx-cache/core]: https://github.com/fulls1z3/ngx-cache/tree/master/packages/@ngx-cache/core
[forRoot]: https://angular.io/docs/ts/latest/guide/ngmodule.html#!#core-for-root
[AoT compilation]: https://angular.io/docs/ts/latest/cookbook/aot-compiler.html
[Burak Tasci]: https://github.com/fulls1z3