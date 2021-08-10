# Angular

- [Angular Roadmap](https://angular.io/guide/roadmap)

## Architecture

### Upgrade/update Angular

#### Update global CLI

To see current version type

```bash
ng --version
```

To install newest version

```bash
npm install -g @angular/cli@latest
```

#### Update angular project

- [Update Angular guide](https://update.angular.io/)

To see if any Angular dependencies at the current version can be upgrade

```bash
ng update
```

Upgrade Angular normally

```bash
ng update @angular/cli @angular/core
```

If there are any dependency errors, then run update for that library, ex. ngrx:

```bash
 ng update @ngrx/store
 ng update @angular/cli @angular/core
```

#### Angular build/serve freezes/fails/stuc without logical errors

- uninstall `node-sass` lib from package.json if installed. () This lib is huge, slow and can create a performance hit in JS processes)
- install `sass` (it should be optional, since Angular uses this by default from `@angular-devkit/build-angular` since v7, but then it displays to the developer which tools is used for scss files compilation)
- install `fibers` to handle and avoid performance hit when build/serve
- run `npm ci` to make sure everything is clean installed

#### JavaScript Memory Heap problem

If you have problems with serve or build, with Javascript HEAP memory problem, try add `node --max_old_space_size=8048` to your package.json npm angular scripts

```json
scripts: {
  "large-build": "node --max_old_space_size=8048 /node_modules/@angular/cli/bin/ng build",
  "large-serve": "node --max_old_space_size=8048 ./node_modules/@angular/cli/bin/ng serve"
}
```

Updating a large angular application might give JS memory heap problems. This example uses an update from 9 to 10.
Roll back just the Angular core and cli versions, migrate only with increased node memory and then update the packages.

Run each step, one at a time:

```bash
npm i @angular/cli@9 @angular/core@9
node --max_old_space_size=30000 ./node_modules/@angular/cli/bin/ng update @angular/cli --from 9 --migrate-only
ng update @angular/core @angular/cli
```

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
    - services (self-provide in root, hence no need for a CoreModule)
      - auth
      - http
      - storage
  - features
    - dashboard
      - components [posts.component.html ..](pure presentation components, dumb components = don't manage state, emits events/actions)
      - pages [dashboard-page.component.html, dashboard-page.component.ts ..](routed smart components, can manages state, manage components, get/send data via services/store)
      - dashboard-routing.module.ts
      - dashboard.module.ts
    - guest-book
      - components
      - pages
      - guest-book-routing.module.ts
      - guest-book.module.ts
    - login
      - components
      - pages
      - login-routing.module.ts
      - login.module.ts
  - shared
    - components [shared-footer, shared-header ..] (prefix shared name, to define area when used)
    - directives
    - pipes
    - validators
    - modules [material.module.ts]
    - shared.modules.ts (import and export ReactiveFormsModule, MaterialModule ... import shared module in feature modules)
  - app-routing.module.ts
  - app.component.html (should only contain `<router-outlet></router-outlet>`)
  - app.module.ts
- styles
  - abstracts [_mixins.scss, _variables.scss, _functions.scss ..]
  - base [_reset.scss, _fonts.scss ..]
  - overwrites [_material-overwrites.scss ..]
  - main.scss (only import the partials in this file, nothing else)
  - theme.scss (material custom theme file, imported via angular.json)
- assets
  - fonts
  - i18n
  - icon
  - images
```

More inspiration on file structure [here](https://www.stratmanmedia.com/2020/02/angular-9-new-project-file-structure/)

### .editorconfig

In Angular 9, .editorconfig is automatic added in root to control project formatting, otherwise create it

`.editorconfig`

```text
# Editor configuration, see https://editorconfig.org
root = true

[*]
charset = utf-8
indent_style = space
indent_size = 2
insert_final_newline = true
trim_trailing_whitespace = true

[*.ts]
quote_type = single

[*.md]
max_line_length = off
trim_trailing_whitespace = false

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

- [Angular CLI generator commands](https://angular.io/cli/generate)

Scaffold new lazy loaded module with `ng g m features/my-feature --routing --route my-feature --module app.module -d`. If you want to follow the pages structure or add components to this feature, delete the component html,scss,ts parts from the module, routing folder and run `ng g c features/my-feature/dashboard --module my-feature.module -d`.

### Supporting IE

Using Differential builds, Angular makes a build for es5 for older browsers and one for es6 for modern browsers.

- [Official Angular Differential build](https://angular.io/guide/deployment#differential-builds)
- [“Angular and Internet Explorer” by Todd Palmer](https://link.medium.com/CVNNy8Vb54)
- [“Angular: How to support IE11” by Colum Ferry](https://link.medium.com/IB8fVsyb54)

### Typescript strict mode

To enable typescript strict mode, simply set `"strict": true` in `tsconfig.base.json` - but in existing projects, this will give an overload of errors, thats why you can enable one set of rules at a time:

```json
{
  // ...,
  "compilerOptions": {
    // a set of cool rules
    "noImplicitAny": true,
    "noImplicitThis": true,
    "strictNullChecks": true,
    "strictPropertyInitialization": true,
    "strictBindCallApply": true,
    "strictFunctionTypes": true,
    // a shortcut enabling 6 rules above
    "strict": true
    // ...
  }
}
```

### Shortcut / hotkey listener

Shortcut listener is very easy to create, using angular pseudo events. Create it for example in a directory, and add that directory to you host element.

- Link: [Angular Pseudo Events](https://medium.com/claritydesignsystem/angular-pseudo-events-d4e7f89247ee)

```typescript
import { Directive, HostListener } from '@angular/core';

@Directive({
  selector: '[appKeyboardShortcuts]',
})
export class KeyboardShortcutsDirective {
  @HostListener('document:keydown.ctrl.f', ['$event'])
  activated(event: KeyboardEvent) {
    event.preventDefault();
    console.log('hit');
  }
}
```

### Migrate from TSLint to ESLint

Follow this [Migrate from TSLint to ESLint](https://github.com/angular-eslint/angular-eslint#migrating-an-angular-cli-project-from-codelyzer-and-tslint) guide to migrate your project from TSLint to ESLint.

### Linting with ESLint with Prettier enforced

If you want to enforce Prettier with ESLint, you can implement eslint-plugin-prettier:

- [notes for eslint plugin prettier users](https://github.com/angular-eslint/angular-eslint#notes-for-eslint-plugin-prettier-users)
- [installation guide for eslint-plugin-prettier](https://github.com/prettier/eslint-plugin-prettier).

If you use vscode-eslint, and want to lint HTML files and inline-templates on your Angular Components, you will need to make sure you add the following to your VSCode settings.json:

```json
  "eslint.options": {
    "extensions": [".ts", ".html"]
  },
  "eslint.validate": ["javascript", "typescript", "html"],
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true,
    "source.organizeImports": true
  },
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
```

And add plugin to workspace recommendation

```json
{
  "recommendations": [
    "angular.ng-template",
    "editorconfig.editorconfig",
    "esbenp.prettier-vscode",
    "dbaeumer.vscode-eslint"
  ]
}
```

You might also want to define the rules with an root file `.editorconfig` for CI pipeline and for user settings

```json
[*]
indent_size = 2
indent_style = space
insert_final_newline = true
charset = utf-8
trim_trailing_whitespace = true
end_of_line = lf

[*.{js,ts,scss,css,html}]
indent_size = 4

[*.md]
trim_trailing_whitespace = false
```

And a root file `.gitattributes` for editor and git

```
* text=auto eol=lf
```

Here is a full example of eslintrc.json file with prettier enforced

```json
{
  "root": true,
  "ignorePatterns": ["projects/**/*"],
  "overrides": [
    {
      "files": ["*.ts"],
      "parserOptions": {
        "project": ["tsconfig.json", "cypress/tsconfig.json"],
        "createDefaultProgram": true
      },
      "extends": [
        "plugin:@angular-eslint/recommended",
        "plugin:@angular-eslint/template/process-inline-templates",
        "plugin:prettier/recommended"
      ],
      "rules": {}
    },
    // NOTE: WE ARE NOT APPLYING PRETTIER IN THIS OVERRIDE, ONLY @ANGULAR-ESLINT/TEMPLATE
    {
      "files": ["*.html"],
      "extends": ["plugin:@angular-eslint/template/recommended"],
      "rules": {}
    },
    // NOTE: WE ARE NOT APPLYING @ANGULAR-ESLINT/TEMPLATE IN THIS OVERRIDE, ONLY PRETTIER
    {
      "files": ["*.html"],
      "excludedFiles": ["*inline-template-*.component.html"],
      "extends": ["plugin:prettier/recommended"],
      "rules": {
        // NOTE: WE ARE OVERRIDING THE DEFAULT CONFIG TO ALWAYS SET THE PARSER TO ANGULAR (SEE BELOW)
        "prettier/prettier": ["error", { "parser": "angular" }]
      }
    }
  ]
}
```

You might want to use this root file `.prettierrc` to make sure your prettier uses correct settings

.prettierrc

```json
{
  "singleQuote": true,
  "endOfLine": "auto",
  "overrides": [
    {
      "files": "*.html",
      "options": {
        "parser": "angular"
      }
    }
  ]
}
```

After installing prettier `npm i prettier -D --save-exact`, example: `"prettier": "2.3.2"`, everything you should need to format all files is to run `ng lint` and `ng lint --fix`

### Updating browser URL query params without reloading

Angular's Location service should be used when interacting with the browser's URL and not for routing. Thats why we want to use [Location](https://angular.io/api/common/Location#replaceState) service.

Angulars [HttpParams](https://angular.io/api/common/http/HttpParams) are used to create query params. Remember HttpParams are immutable, meaning it has to be chained when creating the values.

At last, using `this._location.replaceState` to change to URL without reloading the page/route.

```typescript
constructor(
    private _location: Location,
    private _router: Router
) {}

...

updateURLWithNewParamsWithoutReloading() {
    const params = new HttpParams().appendAll({
        price: 100,
        product: 'bag'
    });

    this._location.replaceState(
        this._router.url,
        params.toString()
    );
}
```

## Images

### Lazy load images

- [Lazy Load Images in Angular with Two Lines of Code](https://netbasal.com/lazy-load-images-in-angular-with-two-lines-of-code-beb13cd5a1c4)

Using attribute `loading` that can be set to `auto`, `eager` and `layz` will make images load decidered way.

#### Manual lazy load

```html
<img src="src.png" alt="Angular" loading="lazy" />
```

#### Automatic lazy load

We check if the browser supports this feature. If that’s the case, we add the loading attribute; Otherwise, we leave the default behavior.

```typescript
import { Directive, ElementRef } from '@angular/core';

@Directive({ selector: 'img' })
export class LazyImgDirective {
  constructor({ nativeElement }: ElementRef<HTMLImageElement>) {
    const supports = 'loading' in HTMLImageElement.prototype;

    if (supports) {
      nativeElement.setAttribute('loading', 'lazy');
    }
  }
}
```

## Forms

### Reactive forms

- [Reactive Forms](https://angular.io/guide/reactive-forms)
- [Dynamic Nested Forms](https://stackblitz.com/edit/pjlamb12-form-array-example-inv5mh?file=src/app/app.component.ts)

?> **Use FormBuilder service** Creating form control instances manually can become repetitive when dealing with multiple forms. The FormBuilder service provides convenient methods for generating controls.

- Use validators to validate. Validators can be user created.
- Use nested group for more complex forms
- Use controls reference, for type safety in html and to avoid calling .get control function on each cycle
- Use DTO (Data Transfer Object) to create domain ready model created from form

<!-- tabs:start -->

#### **register.component.ts**

```typescript
import { Component, EventEmitter, OnInit, Output } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';
import { User } from 'src/app/core/models/user.model';

@Component({
  selector: 'app-register',
  templateUrl: './register.component.html',
  styleUrls: ['./register.component.scss'],
})
export class RegisterComponent implements OnInit {
  @Output() formSubmit = new EventEmitter<User>();
  hide = true;

  registerForm: FormGroup = this.fb.group({
    email: ['', [Validators.required, Validators.email]],
    password: ['', [Validators.required, Validators.minLength(4)]],
    info: this.fb.group({
      firstname: [''],
      lastname: [''],
    }),
  });

  controls = {
    email: this.registerForm.get('email'),
    password: this.registerForm.get('password'),
    firstname: this.registerForm.get('info.firstname'),
    lastname: this.registerForm.get('info.lastname'),
  };

  constructor(private fb: FormBuilder) {}

  onSubmit() {
    const user = new User({
      ...this.registerForm.value,
      firstname: this.controls.firstname.value,
      lastname: this.controls.lastname.value,
    });
    this.formSubmit.emit(user);
  }

  ngOnInit(): void {}
}
```

#### **register.component.html**

```html
<form (ngSubmit)="onSubmit()" [formGroup]="registerForm" novalidate>
  <mat-card>
    <mat-card-header>
      <mat-card-title>Register</mat-card-title>
    </mat-card-header>
    <mat-card-content>
      <mat-form-field class="full-width" appearance="standard">
        <mat-label>Email*</mat-label>
        <input
          matInput
          placeholder="email"
          formControlName="email"
          type="email"
        />
        <mat-error *ngIf="controls.email.hasError('required')"
          >Email is <strong>required</strong></mat-error
        >
        <mat-error *ngIf="controls.email.hasError('email')"
          >Email must be <strong>valid</strong></mat-error
        >
        <mat-icon matPrefix>mail</mat-icon>
      </mat-form-field>

      <mat-form-field class="full-width" appearance="standard">
        <mat-label>Password*</mat-label>
        <input
          matInput
          placeholder="password"
          formControlName="password"
          [type]="hide ? 'password' : 'text'"
        />
        <mat-icon matPrefix>lock</mat-icon>
        <mat-hint>Must be at least 4 chars</mat-hint>
        <mat-error *ngIf="controls.password.hasError('required')">
          Password is <strong>required</strong>
        </mat-error>
        <mat-error *ngIf="controls.password.hasError('minlength')"
          >Password must be at least <strong>4 chars</strong></mat-error
        >
        <button mat-icon-button matSuffix (click)="hide = !hide">
          <mat-icon>{{ hide ? 'visibility_off' : 'visibility' }}</mat-icon>
        </button>
      </mat-form-field>
      <div formGroupName="info" class="info">
        <mat-card-subtitle>Optional</mat-card-subtitle>
        <mat-form-field appearance="standard">
          <mat-label>Firstname</mat-label>
          <input matInput formControlName="firstname" placeholder="firstname" />
        </mat-form-field>
        <mat-form-field appearance="standard">
          <mat-label>Lastname</mat-label>
          <input matInput formControlName="lastname" placeholder="lastname" />
        </mat-form-field>
      </div>
    </mat-card-content>

    <mat-card-actions align="right">
      <button
        mat-raised-button
        [disabled]="!registerForm.valid"
        type="submit"
        color="primary"
      >
        Register
      </button>
    </mat-card-actions>

    <mat-card-footer> * required </mat-card-footer>
  </mat-card>
</form>
```

#### **user.model.ts**

```typescript
export class User {
  email: string;
  password: string;
  firstname?: string;
  lastname?: string;

  constructor(parameters: User) {
    this.email = parameters.email;
    this.password = parameters.password;
    this.firstname = parameters.firstname;
    this.lastname = parameters.lastname;
  }
}
```

<!-- tabs:end -->

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
<ANY translate> login.email </ANY>

<!-- As content key with params -->
<ANY translate [translateParams]="{email: 'world'}"> login.email </ANY>

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

## Testing (Jest)

### Jest introduction

- [Official docs](https://jestjs.io/docs/en/api)

What is Jest ?

- Jest is a testing platform, widely adapted by many large companies and swiftly adopted by the React community.
- Sits on top of Jasmine, so the API is nearly identical.
- Has all of it’s API documented, along with guides, examples and helpful community on forums like Reactiflux and Stack Overflow.
- Focuses on Developer Experience (speed and ease of use is the first priority.)
- Provides meaningful error messages.
- Runs on Continuous Integration servers without extra tooling (abstracting the DOM with jsdom library.)
- Provides code coverage out of the box.
- Integrates with Babel and TypeScript seamlessly.
- Provides a smart, immersive watch mode.

Why Jest?

- run your Angular tests without a browser
- run test suite several times faster
- rerun instantly only tests related to latest code changes

### Setup Jest

- To be able run tun Jest directly from vs code/terminal, use [ts-jest](https://github.com/kulshekhar/ts-jest)
- If `expect` or another Jest functions doesn't exist, it might be because you use Cypress in the same project. An easy workaround is to import the jest functions in the test file `import { expect } from '@jest/globals';`

Follow any of these guide

- [Testing Angular faster jest](https://www.xfive.co/blog/testing-angular-faster-jest/)
- [Integrate jest into Angular](https://medium.com/angular-in-depth/integrate-jest-into-an-angular-application-and-library-163b01d977ce)
- [How do i configure jest to test my angular 8 project](https://itnext.io/how-i-do-configure-jest-to-test-my-angular-8-project-2bd84a21d725)

.. to implement [Jest-preset-angular](https://github.com/thymikee/jest-preset-angular)

See example application [here](https://github.com/thymikee/jest-preset-angular/tree/master/example).

## Testing (karma and Jasmine)

### Introduction (Karma/Jasmine)

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

### Service AngularFireStorage example

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

?> Use the generic version of jasmine.createSpyObj. This ensures type-safety of your spies, allowing the compiler to verify that the methods being mocked actually exist

```typescript
import { NO_ERRORS_SCHEMA } from '@angular/core';
import { async, ComponentFixture, TestBed } from '@angular/core/testing';
import { AngularFireStorage } from '@angular/fire/storage';
import { MatBottomSheet } from '@angular/material/bottom-sheet';
import { MatDialog } from '@angular/material/dialog';

import { ImageEditorService } from '../../services/image-editor.service';
import { EditImageComponent } from './edit-image.component';

describe('EditImageComponent', () => {
  let component: EditImageComponent;
  let fixture: ComponentFixture<EditImageComponent>;
  // define service, so it can be referenced from test blocks for intellisense and to create spyObj on in beforeEach
  let imageEditorServiceSpy: jasmine.SpyObj<ImageEditorService>;

  beforeEach(async(() => {
    // create spy obj and cast the type to get auto complete on the fn names inside the array
    imageEditorServiceSpy = jasmine.createSpyObj<ImageEditorService>(
      'ImageEditorService',
      [
        'canvasToBlob',
        'upload',
        'retryRetrieveDownloadUrls',
        'createImageFromImageDataUrl',
        'canvasDraw',
      ]
    );

    TestBed.configureTestingModule({
      declarations: [EditImageComponent],
      providers: [
        {
          provide: ImageEditorService,
          // use the spy service as value
          useValue: imageEditorServiceSpy,
        },
        { provide: MatBottomSheet, useValue: {} },
        {
          provide: MatDialog,
          useValue: jasmine.createSpyObj('MatDialog', ['open']),
        },
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
import { async, ComponentFixture, TestBed } from '@angular/core/testing';
import { BehaviorSubject } from 'rxjs';
import { ImageEditorService } from '../../services/image-editor.service';
import { ImageEditorEditComponent } from './image-editor-edit.component';

describe('ImageEditorEditComponent', () => {
  let component: ImageEditorEditComponent;
  let fixture: ComponentFixture<ImageEditorEditComponent>;
  let imageEditorServiceMock: jasmine.SpyObj<ImageEditorService>;

  beforeEach(async(() => {
    imageEditorServiceMock = jasmine.createSpyObj<ImageEditorService>([
      'imageHandler',
    ]);

    TestBed.configureTestingModule({
      declarations: [ImageEditorEditComponent],
      providers: [
        { provide: ImageEditorService, useValue: imageEditorServiceMock },
      ],
      // Using NO_ERRORS_SCHEMA to avoid having to include all custom elements
      schemas: [NO_ERRORS_SCHEMA],
    }).compileComponents();
  }));

  beforeEach(() => {
    fixture = TestBed.createComponent(ImageEditorEditComponent);
    component = fixture.componentInstance;
  });

  it('should create', () => {
    expect(component).toBeTruthy();
  });

  it('should display app-file-input on imageDataURL not exists', () => {
    // define component data property
    let cdata;
    const hostEl: HTMLElement = fixture.nativeElement;
    const service = TestBed.inject(ImageEditorService);
    service.imageHandler.imageDataURL$ = new BehaviorSubject(null);

    component.imageDataURL$.subscribe((data) => (cdata = data));
    // Update component view
    fixture.detectChanges();
    const appFileInput = hostEl.querySelector('app-file-input');

    // Check that ngIf renders correct elements, and component data should be null
    expect(appFileInput).toBeTruthy();
    expect(cdata).toBeNull();
  });
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

Angular Material comprises a range of components which implement common interaction patterns according to the Material Design specification. Angular Material components can be found [here]()

### Material references

- [Angular Material official](https://material.angular.io/)
- [Angular Material Icons](https://material.angular.io/components/icon/overview)

### Material install

Install Angular Material

```bash
ng add @angular/material
```

### Material module

- We move taking care of importing Material component modules into an isolated module: MaterialModule.
- We keep our app modules clean
- Import and export material.module into shared.module to be imported into feature modules

<!-- tabs:start -->

#### **app/shared/material/material.module.ts**

```typescript
import { NgModule } from '@angular/core';
import { MatAutocompleteModule } from '@angular/material/autocomplete';
import { MatButtonModule } from '@angular/material/button';
import { MatButtonToggleModule } from '@angular/material/button-toggle';
import { MatCardModule } from '@angular/material/card';
import { MatChipsModule } from '@angular/material/chips';
import { MatPseudoCheckboxModule } from '@angular/material/core';
import { MatDatepickerModule } from '@angular/material/datepicker';
import { MatDialogModule } from '@angular/material/dialog';
import { MatExpansionModule } from '@angular/material/expansion';
import { MatFormFieldModule } from '@angular/material/form-field';
import { MatGridListModule } from '@angular/material/grid-list';
import { MatIconModule } from '@angular/material/icon';
import { MatInputModule } from '@angular/material/input';
import { MatListModule } from '@angular/material/list';
import { MatMenuModule } from '@angular/material/menu';
import { MatPaginatorModule } from '@angular/material/paginator';
import { MatProgressBarModule } from '@angular/material/progress-bar';
import { MatProgressSpinnerModule } from '@angular/material/progress-spinner';
import { MatRadioModule } from '@angular/material/radio';
import { MatSelectModule } from '@angular/material/select';
import { MatSidenavModule } from '@angular/material/sidenav';
import { MatSlideToggleModule } from '@angular/material/slide-toggle';
import { MatSliderModule } from '@angular/material/slider';
import { MatSnackBarModule } from '@angular/material/snack-bar';
import { MatSortModule } from '@angular/material/sort';
import { MatStepperModule } from '@angular/material/stepper';
import { MatTableModule } from '@angular/material/table';
import { MatTabsModule } from '@angular/material/tabs';
import { MatToolbarModule } from '@angular/material/toolbar';
import { MatTooltipModule } from '@angular/material/tooltip';

@NgModule({
  exports: [
    MatPseudoCheckboxModule,
    MatButtonModule,
    MatInputModule,
    MatAutocompleteModule,
    MatDatepickerModule,
    MatFormFieldModule,
    MatRadioModule,
    MatSelectModule,
    MatSliderModule,
    MatSlideToggleModule,
    MatMenuModule,
    MatSidenavModule,
    MatToolbarModule,
    MatListModule,
    MatGridListModule,
    MatCardModule,
    MatStepperModule,
    MatTabsModule,
    MatExpansionModule,
    MatButtonToggleModule,
    MatChipsModule,
    MatIconModule,
    MatProgressSpinnerModule,
    MatProgressBarModule,
    MatDialogModule,
    MatTooltipModule,
    MatSnackBarModule,
    MatTableModule,
    MatSortModule,
    MatPaginatorModule,
  ],
})
export class MaterialModule {}
```

#### **app/shared/shared.module.ts**

```typescript
import { NgModule } from '@angular/core';
import { MaterialModule } from './material/material.module';

@NgModule({
  declarations: [],
  imports: [MaterialModule],
  exports: [MaterialModule],
})
export class SharedModule {}
```

<!-- tabs:end -->

### Material Icons

Choose icons from [Material Icons](https://material.io/resources/icons) and use in HTML

```html
<mat-icon>cloud_download</mat-icon>
```

### Material Custom Theme

Choose from existing material's palettes [material color selector](https://material.io/resources/color/), [material color tool](https://material.io/inline-tools/color/) or build your own palette [mcg.mbitson palette generator](http://mcg.mbitson.com/)

To avoid accidently importing `mat-core()` more than once while keeping main.scss clean, create a theme.scss file in `src/styles/theme.scss` and add in `angular.json`.

?> This example will show how to use custom theme with light/dark runtime switch. In essence we are just switching a class on the body element

<!-- tabs:start -->

#### **angular.json**

```json
"styles": ["src/styles/main.scss", "src/styles/theme.scss"],
```

#### **src/styles/theme.scss**

```scss
@import '~@angular/material/theming';

$custom-typography: mat-typography-config(
  $font-family: 'Montserrat',
);

@include mat-core($custom-typography);

/* ======== angular material custom theme, arguments: (palette/default/light/dark) ======== */
// Base theme
$theme-primary: mat-palette($mat-blue, 700);
$theme-accent: mat-palette($mat-indigo, 700);
$theme-warn: mat-palette($mat-red);

.theme-light {
  // Light theme
  $theme: mat-light-theme($theme-primary, $theme-accent, $theme-warn);

  @include angular-material-theme($theme);
}

.theme-dark {
  // Dark theme
  $theme: mat-dark-theme($theme-primary, $theme-accent, $theme-warn);

  @include angular-material-theme($theme);
}
```

#### **index.html**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>AngularNgrxMaterialExample</title>
    <base href="/" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <link rel="icon" type="image/x-icon" href="favicon.ico" />
    <link
      href="https://fonts.googleapis.com/css?family=Montserrat:300,400,500&display=swap"
      rel="stylesheet"
    />
    <link
      href="https://fonts.googleapis.com/icon?family=Material+Icons"
      rel="stylesheet"
    />
  </head>
  <body class="mat-typography mat-app-background">
    <app-root></app-root>
  </body>
</html>
```

#### **src/app/shared/components/shared-header.component.html**

```html
<mat-toolbar color="primary">
  <mat-toolbar-row>
    <h1>Angular NgRx Material Example</h1>
    <mat-slide-toggle color="primary" (change)="toggleTheme()"
      >{{ currentTheme$ | async }}</mat-slide-toggle
    >
  </mat-toolbar-row>
</mat-toolbar>
```

#### **src/app/shared/components/shared-header.component.ts**

```typescript
import { Component, OnInit } from '@angular/core';
import { ThemeService } from 'src/app/core/services/theme.service';

@Component({
  selector: 'app-shared-header',
  templateUrl: './shared-header.component.html',
  styleUrls: ['./shared-header.component.scss'],
})
export class SharedHeaderComponent implements OnInit {
  constructor(private themeService: ThemeService) {}

  toggleTheme() {
    this.themeService.toggleTheme();
  }

  get currentTheme$() {
    return this.themeService.currentTheme$;
  }

  ngOnInit(): void {}
}
```

#### **src/app/core/services/theme.service.ts**

```typescript
import { Injectable } from '@angular/core';
import { BehaviorSubject } from 'rxjs';

type Theme = 'theme-light' | 'theme-dark';

@Injectable({
  providedIn: 'root',
})
export class ThemeService {
  private themes: Theme[] = ['theme-light', 'theme-dark'];
  private theme = new BehaviorSubject<Theme>('theme-dark');

  constructor() {
    this.theme.subscribe((theme) => {
      const body = document.querySelector('body');
      body.classList.remove(...this.themes);
      body.classList.add(theme);
    });
  }

  toggleTheme() {
    if (this.theme.value === 'theme-dark') {
      this.theme.next('theme-light');
    } else {
      this.theme.next('theme-dark');
    }
  }

  get currentTheme$() {
    return this.theme;
  }
}
```

<!-- tabs:end -->

### Material style overwrites

- [Styling material components](https://material.angular.io/guide/customizing-component-styles)

Overwrites for normal material components should be in a file, ex.: `app/styles/overwrites/_material-overwrites.scss` and imported to your `style.scss`.

?> For overlay components, use their option class `panelClass` to inject to overwrite class.

### Material html example

<!-- tabs:start -->

#### **register.component.html**

```html
<form (ngSubmit)="onSubmit()" [formGroup]="registerForm" novalidate>
  <mat-card>
    <mat-card-header>
      <mat-card-title>Register</mat-card-title>
    </mat-card-header>
    <mat-card-content>
      <mat-form-field class="full-width" appearance="standard">
        <mat-label>Email*</mat-label>
        <input
          matInput
          placeholder="email"
          formControlName="email"
          type="email"
        />
        <mat-error *ngIf="controls.email.hasError('required')"
          >Email is <strong>required</strong></mat-error
        >
        <mat-error *ngIf="controls.email.hasError('email')"
          >Email must be <strong>valid</strong></mat-error
        >
        <mat-icon matPrefix>mail</mat-icon>
      </mat-form-field>

      <mat-form-field class="full-width" appearance="standard">
        <mat-label>Password*</mat-label>
        <input
          matInput
          placeholder="password"
          formControlName="password"
          [type]="hide ? 'password' : 'text'"
        />
        <mat-icon matPrefix>lock</mat-icon>
        <mat-hint>Must be at least 4 chars</mat-hint>
        <mat-error *ngIf="controls.password.hasError('required')">
          Password is <strong>required</strong>
        </mat-error>
        <mat-error *ngIf="controls.password.hasError('minlength')"
          >Password must be at least <strong>4 chars</strong></mat-error
        >
        <button mat-icon-button matSuffix (click)="hide = !hide">
          <mat-icon>{{ hide ? 'visibility_off' : 'visibility' }}</mat-icon>
        </button>
      </mat-form-field>
      <div formGroupName="info" class="info">
        <mat-card-subtitle>Optional</mat-card-subtitle>
        <mat-form-field appearance="standard">
          <mat-label>Firstname</mat-label>
          <input matInput formControlName="firstname" placeholder="firstname" />
        </mat-form-field>
        <mat-form-field appearance="standard">
          <mat-label>Lastname</mat-label>
          <input matInput formControlName="lastname" placeholder="lastname" />
        </mat-form-field>
      </div>
    </mat-card-content>

    <mat-card-actions align="right">
      <button
        mat-raised-button
        type="button"
        color="warning"
        [routerLink]="['/']"
      >
        Cancel
      </button>
      <button
        mat-raised-button
        [disabled]="!registerForm.valid"
        type="submit"
        color="primary"
      >
        Register
      </button>
    </mat-card-actions>

    <mat-card-footer> * required </mat-card-footer>
  </mat-card>
</form>
```

#### **register.component.scss**

```scss
form {
  min-width: 200px;
  max-width: 400px;
  width: 100%;
}

.full-width {
  width: 100%;
}

mat-card-footer {
  padding: 10px;
}

.info {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: auto 1fr;
  column-gap: 10px;
  mat-card-subtitle {
    margin-top: 15px;
    grid-column: span 2;
  }
}
```

<!-- tabs:end -->

## RxJS

### RxJS references

- [Understanding RxJS (Medium)](https://medium.com/in-depth-angular/rxjs-cheat-sheet-9a1ae72153b4)
- [Learn RxJS](https://www.learnrxjs.io/)
- [RxJS Official](https://rxjs.dev/)
- [RxJS Interactive diagrams](https://rxmarbles.com/)

### Operators

#### Creation

- [Creating Observables - How to create observables in rxjs](https://medium.com/better-programming/how-to-create-observables-in-rxjs-aa3bf79b05e0)

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

### Unsubscribe subscriptions

I recommend using the RxJS native technique "Array/Add" by adding subscriptions, that can all be unsubscribed at once. Then you are not depending on any library, and it's still easy to read and setup.

```typescript
export class MyComponent implements OnDestroy {
  protected subs = new Subscription();
  ...
  this.subs.add(this.userService.getUser().subscribe(...));
  this.subs.add(fromEvent(...).subscribe(...));
  ...
  this.foo$ = someObservable.subscribe();
  this.subscriptions.add(this.foo$)
  ...

  public ngOnDestroy() {
    this.subs.unsubscribe(); // yep, this must be done manually
  }
}
```

.. or if you feel like to use 3rd party library, then use [SubSink](https://github.com/wardbell/subsink) which can do the same as the RxJS native method, but also do it as a getter-way, instead of just a function-way.

```typescript
export class MyComponent implements OnDestroy {
  protected subs = new SubSink();
  // adding the subscriptions to the sink - "setter" way
  this.subs.sink = this.userService.getUser().subscribe(...);
  this.subs.sink = fromEvent(...).subscribe(...);
  // adding the subscriptions to the sink - "collection" way
  this.subs.add(
    this.userService.getUser().subscribe(...),
    fromEvent(...).subscribe(...)
  );
  public ngOnDestroy() {
    this.subs.unsubscribe(); // yep, this must be done manually
  }
}
```

#### Template driven unsubscribe

Template driven subscription will automatically unsubscribe

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
  <li *ngFor="let user of users">{{ user.name }}</li>
</ul>
<ng-template #indicator>
  <loading-indicator></loading-indicator>
</ng-template>
```

## API requests

### API references

- [The correct way to make API requests in an Angular application](https://levelup.gitconnected.com/the-correct-way-to-make-api-requests-in-an-angular-application-22a079fe8413)

## NgRx

Reactive State for Angular

### NgRx references

- [NgRx official](https://ngrx.io/)
- [Adding NgRx to Your Existing Applications](https://indepth.dev/adding-ngrx-to-your-existing-applications/)
- [NgRx creator functions 101](https://timdeschryver.dev/blog/ngrx-creator-functions-101)
- [Angular University, NgRx](https://blog.angular-university.io/angular-ngrx-store-and-effects-crash-course/)

### NgRx examples

- [NgRx official example app](https://github.com/ngrx/platform/tree/master/projects/example-app)
- [NgRx Chuck Norris joke generator](https://github.com/wesleygrimes/angular-ngrx-chuck-norris)
- [NgRx small todo](https://github.com/andrewevans0102/to-do-with-ngrx)
- [tomastrajan/angular-ngrx-material-starter](https://github.com/tomastrajan/angular-ngrx-material-starter)

### NgRx update

```bash
ng update @ngrx/store
```

### NgRx install

Make sure cli is updated, check with `ng --version`

```bash
npm install -g @angular/cli@latest
```

Install NgRx dependencies

```bash
npm install @ngrx/{store,effects,entity,store-devtools}
```

Add [schematics](https://ngrx.io/guide/schematics) and select NgRx schematics as the default when promted

```bash
ng add @ngrx/schematics
```

You can add NgRx schematics as default later on like this:

```bash
ng config cli.defaultCollection @ngrx/schematics
```

Set schematics to use SCSS for components in angular.json

```bash
"schematics": {
  "@ngrx/schematics:component": {
    "style": "scss"
  }
},
```

### Initial setup

Generate root [store](https://ngrx.io/guide/schematics/store) and devtools

```bash
ng g store State --root --module app.module.ts
```

Generate root effects

```bash
ng g effect App --root --module app.module.ts
```

### Scaffolding with schematics

Feature Store: Creating a feature store with loginFeatureKey = 'login' and add as forFeature to login.module.ts:

```bash
$ ng g store features/login/Login -m login.module.ts

CREATE src/app/features/login/reducers/index.ts (400 bytes)
UPDATE src/app/features/login/login.module.ts (962 bytes)
```

Action: Creating an action:

```bash
$ ng g a features/login/register-page --group

? Should we generate success and failure actions? Yes
? Do you want to use the create function? Yes
CREATE src/app/features/login/actions/register-page.actions.spec.ts (212 bytes)
CREATE src/app/features/login/actions/register-page.actions.ts (356 bytes)
```

Reducer: Creating reducer and adding it to the feature store

```bash
$ ng g r features/login/user --reducers reducers/index.ts --group

? Should we add success and failure actions to the reducer? Yes
? Do you want to use the create function? Yes
CREATE src/app/features/login/reducers/user.reducer.spec.ts (326 bytes)
CREATE src/app/features/login/reducers/user.reducer.ts (229 bytes)
UPDATE src/app/features/login/reducers/index.ts (469 bytes)
```

Effect: Creating feature effect and adding it to feature module

```bash
$ ng g effect features/login/user -m features/login/login.module.ts --group

? Should we wire up success and failure actions? Yes
? Do you want to use the create function? Yes
CREATE src/app/features/login/effects/user.effects.spec.ts (582 bytes)
CREATE src/app/features/login/effects/user.effects.ts (193 bytes)
UPDATE src/app/features/login/login.module.ts (1150 bytes)
```

### forRoot and forFeature

State in the application can be split into `forRoot` and `forFeature` to organize and separate the state. The `forFeature` is loaded into it's own feature module, and is perfect for separating state where it belongs to. If the `forFeature` state needs to be available to the root state at application start (loaded eagerly), then the module it belongs to, should be loaded eagerly and added directly to `app.module`. The route should equally points to the route of the feature module: `{ path: 'login', redirectTo: '/login' }`.

Whether your feature states are loaded eagerly or lazily depends on the needs of your application. You use feature states to build up your state object over time and through different feature areas.

Login module forFeature example where user is needed i whole application, but logic exists in the Login feature:

<!-- tabs:start -->

#### **app.module.ts**

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { LoginModule } from './features/login/login.module';

@NgModule({
  declarations: [AppComponent],
  imports: [
    BrowserModule,
    AppRoutingModule,
    BrowserAnimationsModule,
    ,
    // forRoot storeModule here
    LoginModule, // eagerly loaded forFeature store
  ],
  providers: [],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

#### **app-routing.module.ts**

```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';

const routes: Routes = [
  { path: 'login', redirectTo: '/login' }, // route to LoginModule
  { path: '', pathMatch: 'full', redirectTo: 'login' },
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule],
})
export class AppRoutingModule {}
```

#### **login.module.ts**

```typescript
import { CommonModule } from '@angular/common';
import { NgModule } from '@angular/core';
import { LoginComponent } from './components/login/login.component';
import { LoginRoutingModule } from './login-routing.module';
import { LoginPageComponent } from './pages/login-page/login-page.component';

@NgModule({
  declarations: [LoginPageComponent, LoginComponent],
  imports: [
    CommonModule,
    LoginRoutingModule,
    // forFeature store here
  ],
})
export class LoginModule {}
```

#### **login-routing.module.ts**

```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { LoginPageComponent } from './pages/login-page/login-page.component';

const routes: Routes = [{ path: 'login', component: LoginPageComponent }]; // LoginModule routing, matches the app-routing.module.ts routing

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule],
})
export class LoginRoutingModule {}
```

<!-- tabs:end -->

### RxJS operators commonly used in NgRX

- Article explaining mergemap, switchmap, concatmap and exhaustmap: <https://medium.com/javascript-in-plain-english/what-is-the-difference-between-mergemap-switchmap-concatmap-and-exhaustmap-173bde3de7e9>

- `ofType` = Comes from `Effects`. Used to listens for one or more `Actions`. Only one of the `Actions` needs to be called.
- `withLatestFrom` = Used on an `Observable` to take the lastest value from the `Observable` and combine current stream value into an array. If used after `ofType` the result will be `[action, state]`
- `map` = If used in a stream, it will create a new value in the stream
- `filter` = Can be used as an `if true` check, to continue
- `switchMap` = Effects subscribes on `actions$`, so `switchMap` changes the subscription to a new inner subscription, for example a HTTP call. `switchMap` will also cancel any inner HTTP calls not done.
- `mergeMap` = Same as `switchMap` but will not cancel any currently HTTP innner calls.
- `concatMap` = Used to strictly keep the order of the values in the array, for example using an array of actions, then the actions are sure to be run in that order, making sure the state is correct for the following actions.
- `combineLatest` = Can be used to make sure a collection og observables are completed. Used example in a resolver, where an action success or fail, needs to be called, and maybe also some data, and then use the data when all is done.

### NgRx Testing

Testing in NgRx is very easy because the only place in NgRx there is state changes, is in the reducer, and since the reducer are pure functions, the returned result will always be the same.

#### company.reducer.spec.ts

```typescript
import * as companyActions from './../actions/company.actions';
import { companyReducer } from './company.reducer';

describe('companyReducer', () => {
  describe('deleteCompanyActions', ()=> {
    it('should delete a company', ()=> {
      const currentState = [
        { id: 1, name: 'SSW', email: 'email', phone: 123},
        { id: 2, name: 'Microsoft', email: 'email', phone: 123},
      ]

      const expetedResult = [{ id: 2, name: 'Microsoft', email: 'email', phone: 123}];

      const action = new companyActions.DeleteCompanySuccessAction(1);
      const result = companyReducer(currentState, action)M

      expect(result).toEqual(expectedResult);
    })
  })

})
```

### NgRx Multiple Reducers in a State using ActionReducerMap

<!-- tabs:start -->

#### **./subscriptions.module.ts**

```typescript
...
import * as fromFeature from './store/state';

@NgModule({
    imports: [
        ...
        StoreModule.forFeature(
            fromFeature.featureKey,
            fromFeature.FEATURE_REDUCER_TOKEN
        ),
    ],
    declarations: [
        ...
    ],
    providers: [
        ...
        {
            provide: fromFeature.FEATURE_REDUCER_TOKEN,
            useFactory: fromFeature.getReducers,
        },
    ],
})
export class SubscriptionsModule {}

```

#### **./store/state.ts**

```typescript
import { InjectionToken } from '@angular/core';
import { ActionReducerMap, createFeatureSelector } from '@ngrx/store';
import { productReducer, ProductState } from './reducers/product.reducer';
import {
  subscriptionReducer,
  SubscriptionState,
} from './reducers/subscriptions.reducer';

export interface State {
  productState: ProductState;
  subscriptionState: SubscriptionState;
}

export const FEATURE_REDUCER_TOKEN = new InjectionToken<
  ActionReducerMap<State>
>('Feature Reducers');

export function getReducers(): ActionReducerMap<State> {
  return {
    productState: productReducer,
    subscriptionState: subscriptionReducer,
  };
}

export const featureKey = 'subscription';

export const getSubscriptionState = createFeatureSelector<State>(featureKey);
```

#### **./store/reducer/product.reducer.ts**

```typescript
import { createReducer, createSelector, on } from '@ngrx/store';
import * as ProductActions from '../actions/product.actions';
import * as fromFeature from '../state';

export interface ProductState {
  products: Product[];
  productsLoading: boolean;
  productsLoaded: boolean;
}

export const initialState: ProductState = {
  products: [],
  productsLoading: false,
  productsLoaded: false,
};

export const productReducer = createReducer(
  initialState,

  on(
    ProductActions.getProducts,

    (state, action) => ({
      ...state,
      productsLoading: true,
    })
  ),

  on(
    ProductActions.getProductsSuccess,
    ProductActions.getProductsFailure,

    (state, action) => ({
      ...state,
      productsLoading: false,
      productsLoaded: true,
    })
  ),

  on(ProductActions.setProducts, (state, action) => ({
    ...state,
    products: action.products,
  }))
);

export const selectProductState = createSelector(
  fromFeature.getSubscriptionState,
  (state: fromFeature.State) => state.productState
);

export const selectProducts = createSelector(
  selectProductState,
  (state: ProductState) => state.products
);

export const selectProductPageLoaded = createSelector(
  selectProductState,
  (state: ProductState) => state.productsLoaded
);

export const selectProductPageLoading = createSelector(
  selectProductState,
  (state: ProductState) => state.productsLoading
);
```

#### **./store/actions/product.action.ts**

```typescript
import { createAction, props } from '@ngrx/store';

export const getProducts = createAction('[Product] get Products');

export const getProductsSuccess = createAction(
  '[Product] get Products Success'
);

export const getProductsFailure = createAction(
  '[Product] get Products Failure',
  props<{ error: any }>()
);

export const setProducts = createAction(
  '[Product] set Products',
  props<{ products: Product[] }>()
);
```

#### **./store/effects/product.effects.ts**

```typescript
import { Injectable } from '@angular/core';
import { Actions, createEffect, ofType } from '@ngrx/effects';
import { of } from 'rxjs';
import { catchError, mergeMap, withLatestFrom } from 'rxjs/operators';
import * as ProductActions from '../actions/product.actions';

@Injectable()
export class ProductEffects {
  constructor(
    private actions$: Actions,
    private _productService: ProductService,
    private _store: Store
  ) {}

  getProducts$ = createEffect(() =>
    this.actions$.pipe(
      ofType(ProductActions.getProducts),
      withLatestFrom(this._store),
      mergeMap(([action, store]) =>
        this._productService.getProducts(store.organization.id).pipe(
          mergeMap((products) => [
            ProductActions.getProductsSuccess(),
            ProductActions.setProducts({
              products,
            }),
          ]),
          catchError((error) =>
            of(
              ProductActions.getProductsFailure({
                error,
              })
            )
          )
        )
      )
    )
  );
}
```

<!-- tabs:end -->

## Server Side Rendering (SSR)

To avoid flickering on page load for SSR applications, it is important to cache get requests, so they are not made on both the server and then again on the client, rerendering the page that was already rendered on the server. Read more here:

- [Angular Server Side Rendering State Transfer For HTTP Requests](https://hackernoon.com/angular-server-side-rendering-state-transfer-for-http-requests-wu263t3h)

## Build configuration - deploy url

For building angular app that points to assets on a different path than the web URL, you can set --deploy-url and a node var etc. DEPLOY_URL to specify an absolute or relative url for assets.

Set it in the npm script:

```json
    "build": "cross-env DEPLOY_URL=%npm_config_deploy_url% ng build --deploy-url %npm_config_deploy_url%",
```

Assets that should be used are set in `angular.json` - default path is set to assets. But a custom path could be `/content`

```json
 "assets": [
  {
    "glob": "**/*",
    "input": "src/content",
    "output": "/content"
  },
```

To use the images from this assets path together with the `deploy-url` use as such:

**HTML**
HTML is used by angular compiler when building, so it needs to be set via ts or a pipe, using the node variable

```ts
declare var DEPLOY_URL: string;

import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'srcPipe',
})
export class SrcPipe implements PipeTransform {
  transform(srcUrl: string): string {
    return `${DEPLOY_URL ? DEPLOY_URL : '/'}${srcUrl}`;
  }
}
```

and then used in HTML

```html
<img
  [src]="
    'content/img/show_icon_eye.png'
        | srcPipe
    "
/>
```

**SCSS**

SCSS files are compiled by webpack by fileloader, so the path will be compiled correctly, when referencing by relative url.

```scss
.img {
  background-image: url('../../../content/img/purchase-voucher/dk-flag.svg');
  height: 100px;
  width: 100px;
}
```
