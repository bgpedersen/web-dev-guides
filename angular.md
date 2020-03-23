# Angular

## Contents

- [Angular](#angular)
  - [Contents](#contents)
  - [Architecture](#architecture)
    - [Architecture: Upgrading Angular](#architecture-upgrading-angular)
    - [Architecture: How to share styles between Angular Components](#architecture-how-to-share-styles-between-angular-components)
    - [Architecture: Refactoring angular apps to reactive architecture](#architecture-refactoring-angular-apps-to-reactive-architecture)
    - [Architecture: Best practice Angular 9 file architecture that scales well](#architecture-best-practice-angular-9-file-architecture-that-scales-well)
    - [Architecture: SCSS relative import fix to avoid relative hell](#architecture-scss-relative-import-fix-to-avoid-relative-hell)
    - [Architecture: Scaffolding lazy loaded modules](#architecture-scaffolding-lazy-loaded-modules)
    - [Architecture: Supporting Internet Explorer IE](#architecture-supporting-internet-explorer-ie)
  - [i18n](#i18n)
    - [i18n: Transloco](#i18n-transloco)
    - [i18n: ngx-translate](#i18n-ngx-translate)
  - [Testing (with Karma and Jasmine)](#testing-with-karma-and-jasmine)
    - [Testing: References](#testing-references)
    - [Testing: Versions](#testing-versions)
    - [Testing: Disable, focus, pending](#testing-disable-focus-pending)
    - [Testing: Equal to](#testing-equal-to)
    - [Testing: Async](#testing-async)
    - [Testing: Mock service injection in a service](#testing-mock-service-injection-in-a-service)
    - [Testing: Expect a failed promise](#testing-expect-a-failed-promise)
  - [Webpack](#webpack)
    - [Webpack: References](#webpack-references)
    - [Webpack: Analyze your bundle with webpack analyzer](#webpack-analyze-your-bundle-with-webpack-analyzer)
      - [Webpack: Reading the bundle output](#webpack-reading-the-bundle-output)
  - [Angular Material](#angular-material)
    - [Angular Material: References](#angular-material-references)
    - [Angular Material: Material icons](#angular-material-material-icons)
    - [Angular Material: Custom theme](#angular-material-custom-theme)
  - [RxJS](#rxjs)
    - [RxJS: 6 ways to unsubscribe from observables in angular](#rxjs-6-ways-to-unsubscribe-from-observables-in-angular)
    - [RxJS: Template driven subscription using the async pipe](#rxjs-template-driven-subscription-using-the-async-pipe)

## Architecture

### Architecture: Upgrading Angular

To see if any Angular dependencies at the current version can be upgrade, type `ng update`.

- [Upgrade Angular guide](https://update.angular.io/)

### Architecture: How to share styles between Angular Components

- [How to share styles between Angular Components](https://dev.to/michaeljota/how-to-share-styles-between-angular-components-nhm)

### Architecture: Refactoring angular apps to reactive architecture

- [Refactoring angular apps to reactive architecture](https://christianlydemann.com/refactoring-angular-apps-to-reactive-architecture/)

### Architecture: Best practice Angular 9 file architecture that scales well

- app
  - core
    - guards
    - interceptors
    - models
    - services [self-provide in root, hence no need for a CoreModule]
      - auth
      - http
      - storage
  - features
    - my-feature
      - components [pure presentation components, get data from page attributes or service]
      - pages [routed components, get data from services, send to components via attributes or service]
      - services [if only used in this feature]
      - models [if only used in this feature]
      - my-feature-routing.module.ts
      - my-feature.module.ts (lazy loaded with loadChildren import into app-routing.module.ts)
  - shared
    - components [shared-footer, shared-header...] (prefix shared name, to define area when used)
    - directives
    - pipes
    - validators
    - shared.modules.ts [import and export CommonModule, FormsModule, MatModules etc., import shared module in feature modules]
  - app-routing.module.ts
  - app.component.html (should only contain `<router-outlet></router-outlet>`)
  - app.module.ts
- styles
  - abstracts (\_mixins.scss, \_variables.scss, \_functions.scss ..)
  - base (\_reset.scss, \_fonts.scss ..)
  - themes (theme.scss, imports angular material theme, files from my-theme folder, and sets up custom material theme)
    - my-theme (\_palette.scss, \_my-theme.scss, \_typography.scss)
  - main.scss (only import the partials in this file, nothing else)
- assets
  - fonts
  - i18n
  - icon
  - images

### Architecture: SCSS relative import fix to avoid relative hell

Just be aware, that the intellisense autocomplete doesn't work with this solution.

Insert stylePreprocessorOptions to your angular.json under options for your build:

```json
"options": {
  "outputPath": "dist/profile-image-creator",
  "index": "src/index.html",
  "main": "src/main.ts",
  "polyfills": "src/polyfills.ts",
  "tsConfig": "tsconfig.app.json",
  "aot": true,
  "assets": ["src/favicon.ico", "src/assets"],
  "styles": ["src/styles/main.scss"],
  "stylePreprocessorOptions": {
    "includePaths": ["src/styles"]
  },
  "scripts": []
},
```

and then reference your styles/abstracts/\_variables.scss like this `@import 'abstracts/variables';`

### Architecture: Scaffolding lazy loaded modules

Scaffold new lazy loaded module with `ng g m features/my-feature --routing --route my-feature --module app.module -d`. If you want to follow the pages structure or add components to this feature, delete the component html,scss,ts parts from the module, routing folder and run `ng g c features/my-feature/dashboard --module my-feature.module -d`.

### Architecture: Supporting Internet Explorer IE

Using Differential builds, Angular makes a build for es5 for older browsers and one for es6 for modern browsers.

- [Official Angular Differential build](https://angular.io/guide/deployment#differential-builds)
- [“Angular and Internet Explorer” by Todd Palmer](https://link.medium.com/CVNNy8Vb54)
- [“Angular: How to support IE11” by Colum Ferry](https://link.medium.com/IB8fVsyb54)

## i18n

**Translation of files and easy shared access, I have good experience using the web solution** [webtranslateit](https://webtranslateit.com/en)

### i18n: Transloco

- [transloco](https://netbasal.gitbook.io/transloco/)
- [comparison-to-other-libraries](https://netbasal.gitbook.io/transloco/comparison-to-other-libraries)

### i18n: ngx-translate

**ngx-translate is not actively maintained anymore, so alternative is transloco or angular i18n. Key differences are that Transloco runs at runtime while Angular i18n compiles a project per language - [ngx-translate issue thread](https://github.com/ngx-translate/core/issues/783)**

- [Github library](https://github.com/ngx-translate/core)
- [Tutorial 1](https://www.codeandweb.com/blog/2019/06/06/how-to-translate-your-angular8-app-with-ngx-translate)
- [Tutorial 2](https://ionicframework.com/docs/v3/developer-resources/ng2-translate/)

JSON translation file example da.json:

```json
{
  "login": {
    "email": "This is your email {{email}}"
  }
}
```

HTML examples

```html
<!-- As content key -->
<ANY translate>
  login.email
</ANY>

<!-- As content key with params -->
<ANY translate [translateParams]="{email: 'world'}">
  login.email
</ANY>

<!-- If translation contains html markup -->
<ANY [innerHTML]="'login.email' | translate"> </ANY>

<!-- As an attributes -->
<ANY [attr.placeholder]="'login.email' | translate"></ANY>
```

TS example using the observable

```typescript
this.translateService
  .get(['global.chooseResident', 'global.cancel'])
  .pipe(take(1))
  .subscribe(async tValues => {
    let tChooseResident = tValues['global.chooseResident'];
    let tCancel = tValues['global.cancel'];
  });
```

## Testing (with Karma and Jasmine)

### Testing: References

- [Official Angular Testing](https://angular.io/guide/testing)
- [Angular unit testing 101 (with examples)](https://dev.to/mustapha/angular-unit-testing-101-with-examples-6mc)
- [Angular testing with examples](https://medium.com/@bencabanes/angular-component-testing-with-examples-7c52b2b7035e)
- [Angular unit test in CI](https://www.codementor.io/@dzhuneyt/how-to-run-angular-unit-tests-in-ci-10304lgzis)
- [Angular testing tips and tricks](https://itnext.io/angular-testing-tips-and-tricks-6c1e21a7cf65)
- [Angular Testing CI](https://blog.angulartraining.com/how-to-running-angular-tests-on-continuous-integration-servers-ad492219c08c)
- [Jasmine docs](https://jasmine.github.io/api/edge/global)
- [Testing the Tour of Heroes — Hero Search Component](https://medium.com/@SimonTestNet/testing-the-tour-of-heroes-hero-search-component-c5b379e93fd3)

### Testing: Versions

Be sure to update Jasmine to the lastest version (currently 3.5) to use the 3.5 docs. `npm i @types/jasmine@latest @types/jasminewd2@latest jasmine-core@latest jasmine-spec-reporter@latest`

### Testing: Disable, focus, pending

Focus: Run only these. Add `f` in front of either `describe` or `it`

```typescript
fdescribe('the unit', () => {
  fit('should do', () => {});
});
```

Disable: Disable these. Add `x` in front of either `describe` or `it`

```typescript
xdescribe('the unit', () => {
  xit('should do', () => {});
});
```

Pending: To mark and disable a test, add function call inside: `pending('with your pending message)`

```typescript
describe('the unit', () => {
  it('should do', () => {
    pending('with your pending message');
  });
});
```

### Testing: Equal to

Equal to a Class or Type

```typescript
let id = 'a string';
expect(id).toBeInstanceOf(String); // jasmine 3.5
expect(id).toEqual(jasmine.any(String));
```

### Testing: Async

`done` can be used to mark block as async

```typescript
it('test async 4 seconds', done => {
  setTimeout(() => {
    expect(1).toBeDefined();
    done();
  }, 4000);
});
```

Tests have a default timeout of 5 seconds. To make a test wait longer, pass in ms timeout to the block

```typescript
it('test async 6 seconds', done => {
  setTimeout(() => {
    expect(1).toBeDefined();
    done();
  }, 6000);
}, 7000);
```

Using async / await can also be used instead of `done`

```typescript
it('test async', async () => {
  const p = () => {
    return new Promise(resolve => {
      setTimeout(() => {
        resolve();
      }, 2000);
    });
  };

  await p();

  expect(1).toBeDefined();
});
```

### Testing: Mock service injection in a service

Don't import the real service to mock, instead create a basic class, and inject in providers. Use jasmine spies to generate functions and reponses needed for the test blocks.

```typescript
import { TestBed } from '@angular/core/testing';
import { AngularFireStorage } from '@angular/fire/storage';

import { ImageEditorService } from './image-editor.service';

// The real AngularFireStorage with the ref function returns an object with a put function which returns a promise. Let's create a simple mock class and create the rest with spies in the test block
class AngularFireStorageMock {
  ref = () => {};
}

describe('ImageEditorService', () => {
  let service: ImageEditorService;
  const afStorage = new AngularFireStorageMock();

  beforeEach(() => {
    TestBed.configureTestingModule({
      providers: [ImageEditorService, { provide: AngularFireStorage, useValue: afStorage }]
    });

    service = TestBed.inject(ImageEditorService);
  });

  describe('upload', () => {
    it('should get id from upload on success', async () => {
      // Adding spy on ref function, and when called
      spyOn(afStorage, 'ref').and.callFake(() => {
        // returning object with a function, that returns a promise with expected data structure
        return { put: jasmine.createSpy('put').and.resolveTo({ metadata: { name: 'id' } }) };
      });
      // We are calling the upload function which internally uses the afStorage we just set up, so it will work without calling the real firebase service
      const id = await service.upload({} as Blob);
      expect(id).toBeInstanceOf(String);
      expect(afStorage.ref).toHaveBeenCalled();
    });
  });
});
```

### Testing: Expect a failed promise

Using `async` and `expectAsync` together with `toBeRejected` or `toBeRejectedWithError`.

```typescript
it('should fail on empty input', async () => {
  await expectAsync(service.upload(null)).toBeRejectedWithError();
});
```

## Webpack

### Webpack: References

- [Webpack bundle analyzer](https://www.npmjs.com/package/webpack-bundle-analyzer)
- [Personalize your angular build with webpack](https://dzone.com/articles/personalize-your-angular-build-with-webpack)

### Webpack: Analyze your bundle with webpack analyzer

- Install analyzer `npm install webpack-bundle-analyzer --save-dev`
- modify your package.json scripts section with `"analyze": "ng build --prod --stats-json && webpack-bundle-analyzer dist/profile-image-creator/stats-es2015.json"`
- npm run analyze

Replace `/profile-image-creator/` with your project name.

#### Webpack: Reading the bundle output

- stat

This is the "input" size of your files, before any transformations like minification.
It is called "stat size" because it's obtained from Webpack's stats object.

- parsed

This is the "output" size of your files. If you're using a Webpack plugin such as Uglify, then this value will reflect the minified size of your code.

- gzip

This is the size of running the parsed bundles/modules through gzip compression.

## Angular Material

### Angular Material: References

- [Angular Material Icons](https://material.angular.io/components/icon/overview)
- <https://ajonp.com/courses/angularmaterial/angular-material-theming/>
- <https://blog.thoughtram.io/angular/2017/05/23/custom-themes-with-angular-material.html>
- <https://material.angular.io/guide/theming-your-components>
- <https://medium.com/@tomastrajan/the-complete-guide-to-angular-material-themes-4d165a9d24d1>
- <https://medium.com/@aleixsuau/how-to-use-angular-material-2-sass-variables-in-your-components-76ce0203f126>

### Angular Material: Material icons

To use Material icons, import (and export) `MatIconModule`

shared.module.ts

```typescript
import { CommonModule } from '@angular/common';
import { NgModule } from '@angular/core';
import { MatIconModule } from '@angular/material/icon';

import { SharedFooterComponent } from './components/shared-footer/shared-footer.component';
import { SharedHeaderComponent } from './components/shared-header/shared-header.component';

@NgModule({
  declarations: [SharedHeaderComponent, SharedFooterComponent],
  imports: [CommonModule, MatIconModule],
  exports: [SharedHeaderComponent, SharedFooterComponent, MatIconModule]
})
export class SharedModule {}
```

Insert in HTML `<mat-icon>cloud_download</mat-icon>` with the name picked from [Material Icons](https://material.io/resources/icons)

### Angular Material: Custom theme

Choose from existing material's palettes [material color selector](https://material.io/resources/color/) or use [material color tool](https://material.io/inline-tools/color/) or easier using auto script page generator [mcg.mbitson palette generator](http://mcg.mbitson.com/)

When you have your palette, insert the load only once needed logic to main.scss, and the variables seperated, so they can be imported elsewhere (remove the custom typography part out, if you want):

main.scss:

```scss
@import '~@angular/material/theming';
@import 'abstracts/variables';

$custom-typography: mat-typography-config(
  $font-family: 'Montserrat'
);
@include angular-material-typography($custom-typography);
@include mat-core($custom-typography);
@include angular-material-theme($theme);
```

\_variables.scss:

```scss
@import '~@angular/material/theming';

$md-custom-primary: (
  50: #e0f2f1,
  100: #b3e0db,
  200: #80cbc4,
  300: #4db6ac,
  400: #26a69a,
  500: #009688,
  600: #008e80,
  700: #008375,
  800: #00796b,
  900: #006858,
  A100: #97ffec,
  A200: #64ffe3,
  A400: #31ffda,
  A700: #18ffd5,
  contrast: (
    50: #000000,
    100: #000000,
    200: #000000,
    300: #000000,
    400: #ffffff,
    500: #ffffff,
    600: #ffffff,
    700: #ffffff,
    800: #ffffff,
    900: #ffffff,
    A100: #000000,
    A200: #000000,
    A400: #000000,
    A700: #000000
  )
);

// Create the theme object (a Sass map containing all of the palettes).
$theme: mat-light-theme(
  mat-palette($md-custom-primary),
  mat-palette($mat-cyan, 800),
  mat-palette($mat-red)
);
$theme: mat-dark-theme(
  mat-palette($md-custom-primary),
  mat-palette($mat-cyan, 800),
  mat-palette($mat-red)
);

// Variables to access in all project
$primary: map-get($theme, primary);
$accent: map-get($theme, accent);
$warn: map-get($theme, warn);
$background: map-get($theme, background);
$foreground: map-get($theme, foreground);

$color-white: #fff;
$color-black: #000;
```

Using the palette and your own variables like so

```scss
footer {
  background-color: mat-color($primary, 500);
  color: $color-white;
}
```

## RxJS

### RxJS: 6 ways to unsubscribe from observables in angular

- [6 ways to unsubscribe from observables in angular](https://blog.bitsrc.io/6-ways-to-unsubscribe-from-observables-in-angular-ab912819a78f)

### RxJS: Template driven subscription using the async pipe

Template driving and thus removing the subscribe handling.

```typescript
export class UserComponent implements OnInit {
  users$: Observable<User[]>;

  constructor(private userService: UserService) {}

  ngOnInit(): void {
    this.users$ = this.userService.getAll();
  }
}
```

```html
<ul *ngIf="users$ | async as users; else indicator">
  <li *ngFor="let user of users">
    {{ user.name }}
  </li>
</ul>
<ng-template #indicator>
  <loading-indicator></loading-indicator>
</ng-template>
```
