# Angular

## Architecture

### Upgrading Angular

To see if any Angular dependencies at the current version can be upgrade, type `ng update`.
Upgrade Angular normally by writing `ng update @angular/cli @angular/core`.

- [Upgrade Angular guide](https://update.angular.io/)

### Share styles

- [How to share styles between Angular Components](https://dev.to/michaeljota/how-to-share-styles-between-angular-components-nhm)

### Reactive architecture

- [Refactoring angular apps to reactive architecture](https://christianlydemann.com/refactoring-angular-apps-to-reactive-architecture/)

### File structure

```text
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
```

### SCSS relative import

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

### Scaffolding

Scaffold new lazy loaded module with `ng g m features/my-feature --routing --route my-feature --module app.module -d`. If you want to follow the pages structure or add components to this feature, delete the component html,scss,ts parts from the module, routing folder and run `ng g c features/my-feature/dashboard --module my-feature.module -d`.

### Supporting IE

Using Differential builds, Angular makes a build for es5 for older browsers and one for es6 for modern browsers.

- [Official Angular Differential build](https://angular.io/guide/deployment#differential-builds)
- [“Angular and Internet Explorer” by Todd Palmer](https://link.medium.com/CVNNy8Vb54)
- [“Angular: How to support IE11” by Colum Ferry](https://link.medium.com/IB8fVsyb54)

## i18n

**Translation of files and easy shared access, I have good experience using the web solution** [webtranslateit](https://webtranslateit.com/en)

### Transloco

- [transloco](https://netbasal.gitbook.io/transloco/)
- [comparison-to-other-libraries](https://netbasal.gitbook.io/transloco/comparison-to-other-libraries)

### ngx-translate

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
  .subscribe(async (tValues) => {
    let tChooseResident = tValues['global.chooseResident'];
    let tCancel = tValues['global.cancel'];
  });
```

## Testing

### Introduction

Unit testing is as important as developing a project nowadays and it becomes an integral part of development. It actually boosts the quality of the code and confidence of developers. Unit tests are written in the Jasmine framework and executed by karma, A test runner and these are executed in the browser.
Sometimes, we write tests before we even start developing which is called TDD. We mostly follow BDD since we are using a jasmine framework.

---

Features of Unit Testing

we have two kinds of testing which are end-to-end testing and unit testing. There are specific features that qualify for unit testing. Let’s go through those.

- Used for behavior tests, Jasmine encourages Behavior-driven development.
- Should run very fast
- Should be tested in isolation without any dependencies
- Should define clear output for the input
- The smallest unit can be tested as a unit

---

When can we write Unit Tests

We have seen features of unit tests, let’s see where we can use these

- At the beginning and completion of initial development
- If we add any feature to the project, we have to write unit tests for it
- If we change any existing feature, we have to write unit tests for it

---

Why use Jasmine

Jasmine is a behavior-driven javascript testing framework. Here are some features of Jasmine

- Default integration with karma
- Provides additional functions such as spies, fakes, and pass-throughs
- easy integration with reporting

---

Why use Karma

Karma is a test-runner for javascript apps. Here are some features of Karma

- Angular recommends Karma and CLI create it out of the box.
- We can automate tests in different browsers and devices
- We can watch files without manual restart
- Online documentation is good
- We can easily integrate with a continuous integration server

### Testing references

- [Official Angular Testing](https://angular.io/guide/testing)
- [Official Angular test examples](https://stackblitz.com/angular/xkagvbjambb)
- [Angular unit testing 101 (with examples)](https://dev.to/mustapha/angular-unit-testing-101-with-examples-6mc)
- [Angular testing with examples](https://medium.com/@bencabanes/angular-component-testing-with-examples-7c52b2b7035e)
- [Angular unit test in CI](https://www.codementor.io/@dzhuneyt/how-to-run-angular-unit-tests-in-ci-10304lgzis)
- [Angular testing tips and tricks](https://itnext.io/angular-testing-tips-and-tricks-6c1e21a7cf65)
- [Angular Testing CI](https://blog.angulartraining.com/how-to-running-angular-tests-on-continuous-integration-servers-ad492219c08c)
- [Jasmine docs](https://jasmine.github.io/api/edge/global)
- [Testing the Tour of Heroes — Hero Search Component](https://medium.com/@SimonTestNet/testing-the-tour-of-heroes-hero-search-component-c5b379e93fd3)
- [Unit Testing in Angular: Stubs vs Spies vs Mocks](https://www.amadousall.com/unit-testing-angular-stubs-vs-spies-vs-mocks/)

### Versions

Be sure to update Jasmine to the lastest version (currently 3.5) to use the 3.5 docs. `npm i @types/jasmine@latest @types/jasminewd2@latest jasmine-core@latest jasmine-spec-reporter@latest`

### Disable, focus, pending

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

### Promises

`done` is the old school callback way. It marks the block as async and is finished when called

```typescript
it('test async 4 seconds', (done) => {
  setTimeout(() => {
    expect(1).toBeDefined();
    done();
  }, 4000);
});

// Tests have a default timeout of 5 seconds. To make a test wait longer, pass in ms timeout to the block
it('test async 6 seconds', (done) => {
  setTimeout(() => {
    expect(1).toBeDefined();
    done();
  }, 6000);
}, 7000);
```

Use `async/await` for easier setup and reading

```typescript
it('test async', async () => {
  const p = () => {
    return new Promise((resolve) => {
      setTimeout(() => {
        resolve('ok');
      }, 2000);
    });
  };

  let result = await p();

  expect(result).toBe('ok');
});
```

Using `async/await` together with `expectAsync` to check a promise function directly

```typescript
it('should fail on empty input', async () => {
  await expectAsync(somePromiseFn()).toBeTruthy();
});
```

### Observables

Use `fakeAsync` wrapper to run observables synchronous to test single value

```typescript
it('should run observable synchronous with fakeAsync', fakeAsync(() => {
  const behaviorSubject$ = new BehaviorSubject({ a: 1 });
  let behaviorSubjectResult;

  behaviorSubject$.subscribe((data) => (behaviorSubjectResult = data));

  expect(behaviorSubjectResult).toEqual({ a: 1 });
}));
```

Use `fakeAsync` wrapper to run virtual time with `tick`

```typescript
it('should run setTimeout synchronous with fakeAsync and tick', fakeAsync(() => {
  let flag = false;

  setTimeout(() => {
    flag = true;
  }, 100);

  expect(flag).toBe(false);
  tick(50);
  expect(flag).toBe(false);
  tick(50);
  expect(flag).toBe(true);
}));
```

Testing single observale value using oldschool `done`

```typescript
it('should test single observalbe with done syntax', (done: DoneFn) => {
  const values = [1, 2, 3];
  const value$ = of(values);

  value$.subscribe((value) => {
    expect(value).toBe(values);
    done();
  });
});
```

Testing multiple observable values using `done()` and `index`

```typescript
it('should test multiple values with done syntax', (done: DoneFn) => {
  const values = [1, 2, 3];
  const values$ = from(values);

  let index = 0;
  values$.subscribe((val) => {
    expect(val).toEqual(values[index]);
    index++;
    done();
  });
});
```

Testing observable values over time with Marble testing and TestScheduler

- [RxJS marble-testing](https://rxjs.dev/guide/testing/marble-testing)

Every frame is one milisecond. Understanding this is important to write the expected value. The errors will show the counted frame, like and index in an array or a number. Following is same as 1 frame.

- a dash `-`
- a value
- a group `()`. `(abc|) counts as single frame`
- a closing subscription `|`

The following `'ab 500ms (c|)'` means it has a total of 3 frames:

- frame 1: value a
- frame 2: value b
- wait 500ms
- frame 3: value c and close subscription

```typescript
const testScheduler = new TestScheduler((actual, expected) => {
  expect(actual).toEqual(expected);
});
it('should run observables over time with marble-testing', () => {
  testScheduler.run((helpers) => {
    const { cold, expectObservable } = helpers;

    const streamValues = { a: 1, b: 2, c: 3 };

    // Create cold observable to be subscribed to. Could be use as as a mock
    const streamCold$ = cold('ab 500ms (c|)', streamValues);
    expectObservable(streamCold$).toBe('ab 500ms (c|)', streamValues);

    // Using RxJS observables to be tested
    const streamOf$ = of(1, 2, 3);
    expectObservable(streamOf$).toBe('(abc|)', streamValues);

    // Test RxJS observables over time
    const streamFrom$ = from([1, 2, 3]);
    expectObservable(streamFrom$).toBe('(abc|)', streamValues);

    // Define the observables also works
    const streamCreateObs$ = new Observable((sub) => {
      sub.next(1);
      sub.next(2);
      sub.next(3);
      sub.complete();
    });
    expectObservable(streamCreateObs$).toBe('(abc|)', streamValues);
  });
});
```

### Service example

```typescript
import { async, TestBed } from '@angular/core/testing';
import { AngularFireStorage } from '@angular/fire/storage';
import { of } from 'rxjs';

import { ImageEditorService } from './image-editor.service';

// Creating mock class
class AngularFireStorageMock {
  ref(path: string) {
    return {
      put: () => new Promise((res) => res({ metadata: { name: 'id' } })),
      getDownloadURL: () => of(path),
    };
  }
}

describe('ImageEditorService', () => {
  let service: ImageEditorService;
  // Value to use in test if needed for spies. Using type will open up for intellisense.
  let afStorage: Partial<AngularFireStorage>;

  beforeEach(async(() => {
    TestBed.configureTestingModule({
      // Inject mock class
      providers: [
        ImageEditorService,
        { provide: AngularFireStorage, useValue: new AngularFireStorageMock() },
      ],
    });

    service = TestBed.inject(ImageEditorService);
    // Injecting mock to the variable to be used if needed
    afStorage = TestBed.inject(AngularFireStorage);
  }));

  it('should be created', () => {
    expect(service).toBeTruthy();
  });

  describe('getRandomId', () => {
    it('should get id string', () => {
      const id = service.getRandomId();
      expect(id).toBeInstanceOf(String);
    });
  });

  describe('upload', () => {
    it('should get id from upload on success', async () => {
      await expectAsync(service.upload({} as Blob)).toBeResolvedTo('id');
    });

    it('should fail on empty input', async () => {
      await expectAsync(service.upload(null)).toBeRejectedWithError();
    });
  });

  describe('retrieveDownloadUrls', () => {
    it('should return array', async () => {
      const downloadUrls = [
        { title: '200x200', url: '/images/thumbs/url_200x200.png' },
        { title: '400x400', url: '/images/thumbs/url_400x400.png' },
        { title: '600x600', url: '/images/thumbs/url_600x600.png' },
      ];

      const res = await service.retrieveDownloadUrls('url');
      expect(res).toEqual(downloadUrls);
    });
  });
});
```

### Component example #1

Here is a full component test example, of how the structure should be when using a service as a SpyObj in depencency injection, having full code editor intelissense for methods and spy methods:

```typescript
import { NO_ERRORS_SCHEMA } from '@angular/core';
import { async, ComponentFixture, TestBed } from '@angular/core/testing';
import { AngularFireStorage } from '@angular/fire/storage';
import { MatBottomSheet } from '@angular/material/bottom-sheet';
import { MatDialog } from '@angular/material/dialog';

import { ImageEditorService } from '../../services/image-editor.service';
import { EditImageComponent } from './edit-image.component';

fdescribe('EditImageComponent', () => {
  let component: EditImageComponent;
  let fixture: ComponentFixture<EditImageComponent>;
  // define service, so it can be referenced from test blocks for intellisence and to create spyObj on in beforeEach
  let imageEditorServiceSpy: jasmine.SpyObj<ImageEditorService>;

  beforeEach(async(() => {
    // create spy obj and cast the type to get auto complete on the fn names inside the array
    imageEditorServiceSpy = jasmine.createSpyObj<ImageEditorService>('ImageEditorService', [
      'canvasToBlob',
      'upload',
      'retryRetrieveDownloadUrls',
      'createImageFromImageDataUrl',
      'canvasDraw',
    ]);

    TestBed.configureTestingModule({
      declarations: [EditImageComponent],
      providers: [
        {
          provide: ImageEditorService,
          // use the spy service as value
          useValue: imageEditorServiceSpy,
        },
        { provide: MatBottomSheet, useValue: {} },
        { provide: MatDialog, useValue: jasmine.createSpyObj('MatDialog', ['open']) },
        { provide: AngularFireStorage, useValue: {} },
      ],
      schemas: [NO_ERRORS_SCHEMA],
    }).compileComponents();
  }));

  beforeEach(() => {
    fixture = TestBed.createComponent(EditImageComponent);
    component = fixture.componentInstance;
    fixture.detectChanges();
  });

  it('should create', () => {
    expect(component).toBeTruthy();
  });

  describe('onDownload', () => {
    it('should call openBottomSheet on success', async () => {
      // using the declared spy service, with all the service methods and the Spy methods
      imageEditorServiceSpy.canvasToBlob.and.resolveTo({} as Blob);
      imageEditorServiceSpy.upload.and.resolveTo('id');
      imageEditorServiceSpy.retryRetrieveDownloadUrls.and.resolveTo([]);
      const spy = spyOn(component, 'openBottomSheet');

      await component.onDownload();

      expect(spy).toHaveBeenCalledTimes(1);
      expect(component.matDialog.open).toHaveBeenCalledTimes(0);
    });
  });
});
```

### Component example #2

Comments in code explains step by step

```typescript
import { NO_ERRORS_SCHEMA } from '@angular/core';
import { async, ComponentFixture, fakeAsync, TestBed } from '@angular/core/testing';
import { BehaviorSubject } from 'rxjs';
import { first } from 'rxjs/operators';

import { ImageEditorService } from '../../services/image-editor.service';
import { ImageEditorEditComponent } from './image-editor-edit.component';

// Setting up service to mock
class ImageEditorServiceMock {
  imageHandler = {
    imageDataURL$: new BehaviorSubject(null),
  };
}

fdescribe('ImageEditorEditComponent', () => {
  let component: ImageEditorEditComponent;
  let fixture: ComponentFixture<ImageEditorEditComponent>;

  beforeEach(async(() => {
    TestBed.configureTestingModule({
      declarations: [ImageEditorEditComponent],
      // Mock ImageEditorService here
      providers: [{ provide: ImageEditorService, useValue: new ImageEditorServiceMock() }],
      // Using NO_ERRORS_SCHEMA to avoid having to include all custom elements
      schemas: [NO_ERRORS_SCHEMA],
    }).compileComponents();
  }));

  beforeEach(() => {
    fixture = TestBed.createComponent(ImageEditorEditComponent);
    component = fixture.componentInstance;
    // Moved detectChanges to blocks for more control
  });

  it('should create', () => {
    expect(component).toBeTruthy();
  });

  // Wrab block in fakeAsync, to run stream subscription synchronous
  it('should display app-file-input on imageDataURL not exists', fakeAsync(() => {
    // TestBed.inject will inject the instantialized service from TestBed.configureTestingModule, meaning our mock service
    const imageEditorServiceMock = TestBed.inject(ImageEditorService);
    // init imageDataURL$ with null
    imageEditorServiceMock.imageHandler.imageDataURL$ = new BehaviorSubject(null);
    // define component data property
    let cdata;
    const hostEl: HTMLElement = fixture.nativeElement;

    // subscribe to stream, close after first data receieved
    component.imageDataURL$.pipe(first()).subscribe((data) => (cdata = data));
    // Update component view
    fixture.detectChanges();

    // Grab the elements by tag
    const appEditImg = hostEl.querySelector('app-edit-image'); // I don't exist
    const appFileInput = hostEl.querySelector('app-file-input');

    // Check that ngIf renders correct elements, and component data should be null
    expect(appFileInput).toBeTruthy();
    expect(appEditImg).toBeNull();
    expect(cdata).toBeNull();
  }));
});
```

## Webpack

### Webpack references

- [Webpack bundle analyzer](https://www.npmjs.com/package/webpack-bundle-analyzer)
- [Personalize your angular build with webpack](https://dzone.com/articles/personalize-your-angular-build-with-webpack)

### Analyze bundle

- Install analyzer `npm install webpack-bundle-analyzer --save-dev`
- modify your package.json scripts section with `"analyze": "ng build --prod --stats-json && webpack-bundle-analyzer dist/profile-image-creator/stats-es2015.json"`
- npm run analyze

Replace `/profile-image-creator/` with your project name.

#### Reading output

- stat

This is the "input" size of your files, before any transformations like minification.
It is called "stat size" because it's obtained from Webpack's stats object.

- parsed

This is the "output" size of your files. If you're using a Webpack plugin such as Uglify, then this value will reflect the minified size of your code.

- gzip

This is the size of running the parsed bundles/modules through gzip compression.

## Angular Material

### Material references

- [Angular Material Icons](https://material.angular.io/components/icon/overview)
- <https://ajonp.com/courses/angularmaterial/angular-material-theming/>
- <https://blog.thoughtram.io/angular/2017/05/23/custom-themes-with-angular-material.html>
- <https://material.angular.io/guide/theming-your-components>
- <https://medium.com/@tomastrajan/the-complete-guide-to-angular-material-themes-4d165a9d24d1>
- <https://medium.com/@aleixsuau/how-to-use-angular-material-2-sass-variables-in-your-components-76ce0203f126>

### Icons

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
  exports: [SharedHeaderComponent, SharedFooterComponent, MatIconModule],
})
export class SharedModule {}
```

Insert in HTML `<mat-icon>cloud_download</mat-icon>` with the name picked from [Material Icons](https://material.io/resources/icons)

### Custom theme

Choose from existing material's palettes [material color selector](https://material.io/resources/color/) or use [material color tool](https://material.io/inline-tools/color/) or easier using auto script page generator [mcg.mbitson palette generator](http://mcg.mbitson.com/)

When you have your palette, insert the load only once needed logic to main.scss, and the variables seperated, so they can be imported elsewhere (remove the custom typography part out, if you want):

main.scss:

```scss
@import '~@angular/material/theming';
@import 'abstracts/variables';

$custom-typography: mat-typography-config(
  $font-family: 'Montserrat',
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
    A700: #000000,
  ),
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

### RxJS references

- [Learn RxJS](https://www.learnrxjs.io/)
- [RxJS Official](https://rxjs.dev/)
- [RxJS Interactive diagrams](https://rxmarbles.com/)

### Operators

#### Creation

`from`: Create observable from array, promise or iterable. Takes only one value. For arrays, iterables and strings, all contained values will be emitted as a sequence

```typescript
const values = [1, 2, 3];
from(values); // 1 ... 2 ... 3
```

`of`: Create observable with variable amounts of values, emit values in sequence

```typescript
const values = [1, 2, 3];
of(values, 'hi', 4, 5); // [1, 2, 3] ... 'hi' ... 4 ... 5
```

`defer`: Create observable with a function that is run on subscription, for example from a promise function

```typescript
const values = [1, 2, 3];
defer(() => new Promise((res, rej) => res(values))); // 1, 2, 3
```

### Combine observables

- [(v6.5+) Using a dictionary of sources](https://www.learnrxjs.io/learn-rxjs/operators/combination/forkjoin)

```typescript
// RxJS v6.5+
import { ajax } from 'rxjs/ajax';
import { forkJoin } from 'rxjs';

/*
  when all observables complete, provide the last
  emitted value from each as dictionary
*/
forkJoin(
  // as of RxJS 6.5+ we can use a dictionary of sources
  {
    google: ajax.getJSON('https://api.github.com/users/google'),
    microsoft: ajax.getJSON('https://api.github.com/users/microsoft'),
    users: ajax.getJSON('https://api.github.com/users'),
  }
)
  // { google: object, microsoft: object, users: array }
  .subscribe(console.log);
```

### Unsubscribing

- [6 ways to unsubscribe from observables in angular](https://blog.bitsrc.io/6-ways-to-unsubscribe-from-observables-in-angular-ab912819a78f)

### Template driven

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

## API requests

### API references

- [The correct way to make API requests in an Angular application](https://levelup.gitconnected.com/the-correct-way-to-make-api-requests-in-an-angular-application-22a079fe8413)

## NgRX

Reactive State for Angular

### NgRX references

- [NgRX official](https://ngrx.io/)
- [Adding NgRx to Your Existing Applications](https://indepth.dev/adding-ngrx-to-your-existing-applications/)
- [NgRx creator functions 101](https://timdeschryver.dev/blog/ngrx-creator-functions-101)
