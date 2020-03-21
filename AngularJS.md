# AngularJS

## Contents

- [AngularJS](#angularjs)
  - [Contents](#contents)
  - [UI Router](#ui-router)
    - [UI Router: Migrate guide UI Router from 0.x to 1.x](#ui-router-migrate-guide-ui-router-from-0x-to-1x)
    - [UI Router: Example](#ui-router-example)
  - [i18n](#i18n)
    - [i18n: Web interface for comparing and translating 2 json files](#i18n-web-interface-for-comparing-and-translating-2-json-files)
    - [i18n: Angular-Translate](#i18n-angular-translate)
      - [i18n: Angular-Translate HTML examples](#i18n-angular-translate-html-examples)
      - [i18n: Angular-Translate JS examples](#i18n-angular-translate-js-examples)
    - [i18n: Webpack Angular Translate](#i18n-webpack-angular-translate)
  - [Testing](#testing)
    - [Testing: Example of AngularJS test file](#testing-example-of-angularjs-test-file)
  - [Migration from AngularJS to Angular](#migration-from-angularjs-to-angular)
    - [Migration from AngularJS to Angular: References](#migration-from-angularjs-to-angular-references)
    - [Migration from AngularJS to Angular: Migration Paths](#migration-from-angularjs-to-angular-migration-paths)

## UI Router

- [UI Router Wiki](https://github.com/angular-ui/ui-router/wiki)

### UI Router: Migrate guide UI Router from 0.x to 1.x

- [Migrate guide UI Router from 0.x to 1.x](https://ui-router.github.io/guide/ng1/migrate-to-1_0#state-change-events)

### UI Router: Example

```javascript
// $transitions is injected into controller
// empty object as state, means any state
$transitions.onBefore({}, function(transition) {
    let from = transition.from().name;
    let to = transition.to().name;
})
```

## i18n

### i18n: Web interface for comparing and translating 2 json files

- [Web interface for comparing and translating 2 json files](http://mrhieu.github.io/ngTranslateEditor/#/)

### i18n: Angular-Translate

- [Angular Translate](https://angular-translate.github.io/docs/#/guide)

#### i18n: Angular-Translate HTML examples

```html
<!-- Best solution for simple translations, with no watch overhead -->
<ANY translate="TRANSLATION_ID"></ANY>
<ANY translate>TRANSLATION_ID</ANY>

<!-- It is possible to use scope variables, but should be avoided because of watch overhead -->
<ANY translate>{{languageToBeInterpolated}} test</ANY>
<ANY translate="{{languageToBeInterpolated}}"></ANY>

<!-- As attribute -->
<ANY placeholder="{{'TRANSLATION_ID' | translate }}">
</ANY>

<!-- As directive attribute -->
<ANY uib-tooltip-html="'{{
  'notifications.notification_profile_info'
  | translate
}}'">
</ANY>

<!-- As directive attribute with value -->
<ANY uib-tooltip-html="'{{
  'planner.amount_overdue_tasks'
  | translate:{amount:entity.overDueTasks.length}
}}'">
</ANY>

<!-- Using variables -->
<ANY translate="TRANSLATION_ID"
     translate-values='{ username: "PascalPrect"}'>
</ANY>

<ANY translate="TRANSLATION_ID"
     translate-value-username='{ someScopeObject.username }'>
</ANY>

<!-- IMPORTANT the value name can't be camelCase. So translate-value-titleName would not work. -->
<ANY translate="TRANSLATION_ID"
     translate-value-title="{{ someScopeObject.title | translate }}">
</ANY>
```

#### i18n: Angular-Translate JS examples

It is possible to send (also dynamic) translate keys to the html, and then translate them though the html. This way is preferred to avoid using the async js service '$translate'.

```javascript
//With the service
let translation = await $translate('TRANSLATION_ID');
Flash.create(
  'success',
  `<span>${translation}</span>
);

//Without the service (Use from inside JS or send the key to HTML).
Flash.create(
  'success',
  '<span translate="TRANSLATION_ID"></span>'
);
```

Example of using variable translation key from within translation file. This should not be done, because it can be translated from HTML using the pipe, inside a value attribue:

```json
"category_in_use_by": "Kategorien er i brug af {{ repeatItemsCount }} <span class=\"lowercase\">{{ typePlural | translate }}</span>."
```

Using an array of translations and the service

```javascript
let translations = await $translate(['TRANSLATION_ID1',
  'TRANSLATION_ID2',
  'TRANSLATION_ID3'
])
let tTitle = translations['TRANSLATION_ID1'];
let tCancel = translations['TRANSLATION_ID2'];
let tOk = translations['TRANSLATION_ID3'];
```

### i18n: Webpack Angular Translate

- [Webpack Angular Translate](https://github.com/MichaReiser/webpack-angular-translate)

Automatic retrieve and build translation file using Webpack Angular Translate

It's seems like a good idea, but there are way too many hurdles and quirks.

- The plugin doesn't work with generics/variables, so in order to get around this, you need to trick webpack into copy/write/ignore etc. the translation file to not go into loops, caching, deleting the translation file, but even then it
doesn't work as expected.
- Dynamic ID keys in HTML files needs to be registered in the JS file. This creates quite a lot of clunky translations that may exists in many js files.
- Translation files are written as indline dotted lines, unordered. This needs a parser if a human needs to work with it.
- Default translations needs to only be declared once, or always the same everywhere.

## Testing

**All official guides says to use Karma with Jasmine. Because technology evolves, this is no longer a good option for testing an up to date AngularJS application using Webpack.**

TL;DR

Use [Mocha Pack](https://github.com/sysgears/mochapack), which is an updated version of [Mocha Webpack](https://github.com/zinserjan/mocha-webpack) together with assert library [UnexpectedJS](http://unexpected.js.org/) and [Sinon](https://sinonjs.org/releases/latest/mocks/) for mocking and [unexpected-sinon](https://github.com/unexpectedjs/unexpected-sinon) for extending unexpected asserts

- [UnexpectedJS assertions](http://unexpected.js.org/assertions/any/to-be/)
- [UnexpectedJS Sinon assertions](https://unexpected.js.org/unexpected-sinon/assertions/spy/was-called/)

**Why [Mocha Pack](https://github.com/sysgears/mochapack) ?**

- Runs in NodeJS with pure javascript (no browser)
- Doesn't have to inject Angular og JQuery, so you can mock everything as you expect them to be and behave, so nothing runs out of your control
- Runs very well together with Webpack, meaning
  - Uses dependency graph, so when running in watch mode, only run tests that are in the dependency of the code being changed
  - Uses source maps very well, to give feedback about where error happens and stack trace
- Runs very well with assert library [UnexpectedJS](http://unexpected.js.org/) to build on top of Mocha assert
- Runs very well with extending UnexpectedJS asserts with [Unexpected-Sinon](https://github.com/unexpectedjs/unexpected-sinon)
- Runs very well with [Sinon](https://sinonjs.org/releases/latest/mocks/) for mock

Tip: To test variables inside promises that you dont know when is finished, create this global function, import it

```javascript
export function flushPromises() {
  return new Promise(resolve => setImmediate(resolve));
}
```

..and call it  before the expects that tests for variables inside a promise:
`await flushPromises();`
This will run on next NodeJS tick, when all promises are done.

**Why not [Karma](https://github.com/karma-runner/karma) ?**

- Karma runs in a browser (psst headless option is still a invisible browser). Because Karma runs in a browser, running tests takes a long time.
- Karma is very bad at supporting source maps, so locating the stack trace og an error is very hard.
- Karma has to inject Angular and will run functions before you can intercept them

**Why not [Jest](https://github.com/facebook/jest) ?**

- Jest runs its own runner (black box), where it mocks several Node commands etc. So you will not have access to all normal node functionality like import, export and others.
- Jest runs several process in parallel, meaning if there are cross dependencies between the processes, it is very hard to debug.

### Testing: Example of AngularJS test file

```typescript
import 'mocha';

import { flushPromises } from '@test/helpers';
import Sinon = require('sinon');

import bookableItemController from './bookableItemController';

interface AnyScope {
  [k: string]: any;
}

let expect = require('unexpected').clone();
expect.use(require('unexpected-sinon'));

describe('Bookable item controller', function() {
  let institutionId;
  let bookableItemService;
  let $scope: AnyScope;
  let $rootScope;
  let Flash;
  let $q;
  let authentication;
  let uploadService;
  let $confirm;
  let $stateParams;
  let LB;
  let itemService;
  let $state;
  let $exceptionHandler;
  let notificationItemService;

  beforeEach(async function() {
    institutionId = 1;
    bookableItemService = {
      getBookableTypeItem: Sinon.stub()
    };

    $scope = {
      user: {
        isSuperuser: false
      },
      $parent: { institution: { id: 1 } },
      setFocus: Sinon.stub(),
      $on: Sinon.stub()
    };
    $rootScope = {
      $on: Sinon.stub(),
      $broadcast: Sinon.stub()
    };

    Flash = {};
    $q = {};
    authentication = {
      getCookie: Sinon.stub()
        .withArgs('institution')
        .returns(institutionId)
    };

    uploadService = {};
    $confirm = Sinon.stub().resolves();
    $stateParams = {};
    LB = {
      getDepartments: Sinon.stub().resolves([{ id: 1 }]),
      getResidents: Sinon.stub().resolves([{ id: 1 }]),
      getEmployees: Sinon.stub().resolves([{ id: 1 }]),
      fetchAllItems: Sinon.stub().resolves([{ id: 1 }]),
      getExternalDepartments: Sinon.stub().resolves([{ id: 1 }]),
      getBookableCountFutureBookings: Sinon.stub().resolves({ count: 5 }),
      deleteItem: Sinon.stub().resolves({})
    };
    itemService = {};
    $state = {
      go: Sinon.stub()
    };
    $exceptionHandler = {};
    notificationItemService = {
      getReminders: Sinon.stub(),
      getRemindersEnd: Sinon.stub()
    };
  });

  describe('Controller checks', function() {
    it('Should not call state.go when has institution id', async function() {
      bookableItemController(
        $scope,
        $rootScope,
        bookableItemService,
        Flash,
        $q,
        authentication,
        uploadService,
        $confirm,
        $stateParams,
        LB,
        itemService,
        $state,
        $exceptionHandler,
        notificationItemService
      );
      await flushPromises();
      expect($state.go, 'was not called');
      expect($scope.departments, 'to satisfy', [{ id: 1 }]);
    });
    it('Should go to main.booking.bookable when no institution id', async function() {
      $scope.$parent.institution = {};
      bookableItemController(
        $scope,
        $rootScope,
        bookableItemService,
        Flash,
        $q,
        authentication,
        uploadService,
        $confirm,
        $stateParams,
        LB,
        itemService,
        $state,
        $exceptionHandler,
        notificationItemService
      );
      await flushPromises();
      expect($state.go, 'to have a call satisfying', ['main.booking.bookable']);
    });
  });

  describe('Controller actions', function() {
    beforeEach(async function() {
      bookableItemController(
        $scope,
        $rootScope,
        bookableItemService,
        Flash,
        $q,
        authentication,
        uploadService,
        $confirm,
        $stateParams,
        LB,
        itemService,
        $state,
        $exceptionHandler,
        notificationItemService
      );
      await flushPromises();
    });
    describe('Deleting a Bookable', function() {
      it('Should delete a Bookable when confirmed', async function() {
        $scope.user.isSuperuser = true;
        $scope.editItem = {
          id: 1,
          title: 'test',
          user: $scope.user.isSuperuser
        };
        $scope.delete();
        await flushPromises();
        expect(LB.getBookableCountFutureBookings, 'to have a call satisfying', {
          args: [1]
        });
        expect($confirm, 'to have a call satisfying', {
          args: [
            expect
              .it('to be an object')
              .and('to satisfy', { item: { inUseByCount: 5 } }),
            expect.it('to be an object')
          ]
        });
        expect(LB.deleteItem, 'to have a call satisfying', {
          args: ['Bookables', { id: 1, institutionId: 1, status: 0 }]
        });
        expect($rootScope.$broadcast, 'to have a call satisfying', {
          args: ['item:deleted']
        });
      });
      it('Should not delete a Bookable when pressing cancel', async function() {
        $scope.user.isSuperuser = true;
        $scope.editItem = {
          id: 1,
          title: 'test',
          user: $scope.user.isSuperuser
        };
        $confirm.onCall(0).rejects();
        $scope.delete();
        await flushPromises();
        expect(LB.deleteItem, 'was not called');
      });
    });
  });
});

```

## Migration from AngularJS to Angular

### Migration from AngularJS to Angular: References

YouTube

- 2019 - Matt Weimer - AngularJS to Angular 7 Migration <https://youtu.be/b9fzQjsAbmw>
- Migrating from AngularJS to Angular | Elana Olson | AngularConnect 2018 <https://youtu.be/fFLKEtIqZeY>

Guides

- Official upgrade guide <https://angular.io/guide/upgrade>
- Official ngUpgrade Wiki og Community  <https://github.com/angular/ngMigration-Forum/wiki/ngUpgrade>
- Mindre migration guide 2019 (<https://gofore.com/de/migration-from-angularjs-part3/)>
- AngularJS Style Guide <https://github.com/toddmotto/angularjs-styleguide>

Tools

- ngMigration Assistent (scanner og giver statistik og analyse for hvordan man bedst migrerer den specifikke app og hvilke forberedelser som er nødvendige før migration) <https://github.com/ellamaolson/ngMigration-Assistant>
- TypeWiz (TypeWiz to help convert your JavaScript code to Typescript by automatically including types where needed.) <https://github.com/urish/typewiz>
- ng-lift (The ng-lift CLI allows automatic template upgrade) <https://github.com/urish/ng-lift>

Eksempler

- <https://github.com/vizjerai/angularjs-to-angular> (2018)
- <https://github.com/thomsonreuters/FEF/tree/master/ng-rosetta> (2017)
- <https://github.com/mattwilson1024/angular-hybrid> (2017)
- <https://github.com/angular-upgrade-examples/todo-app/> (2017, via commits)

### Migration from AngularJS to Angular: Migration Paths

Migration path: Incremental Migration

Incremental Migration: Hybrid app, begge frameworks på samme tid. ngUpgrade module til dette. Består af 4 faser (forberedelse, bootstrapping, migration, cleanup)

Fase 0: Forberedelse

- AngularJS 1.5 components. Fra AngularJS 1.5 kan man bruge ‘component directives’ som gør det nemmere at migrerer, hvis man omskriver sine gamle controllers til dette. Kan også bruges som upgraded, så de kan bruges i  Angular context uden migration hvis man vil.
- Brug Webpack og lav alle components/controllers med imports
- Fjern $scope og $rootScope (omskriv $rootScope til services).
- Brug ES6 og TS classes
- Brug TypeScript i migreringen til nye filer (ikke til gamle filer)
- Brug Angular CLI, flytte gamle build tools ind i det det nye Angular CLI som bruger integreret Webpack som kan bruge Angular Builders.
- Brug “Folder By Module”, delte componenter i Shared/Common Module
- Router: Brug @uirouter/angular-hybrid ( <https://github.com/ui-router/angular-hybrid> ). Angulars nye Router, kan kun bruges hvis alt er Angular Components. Derfor skal vi bruge Angular-UI hybrid router som kan serve både AngularJS og Angular indtil det hele er migreret.

Fase 1: Bootstrapping

- Lav ny Angular sample app med Cli og flyt filer (se video) til AngularJS app. Installer packages fra package.json fra sample app + nogle ekstra for hybriden. Fjern gamle AngularJS packages og gamle npm scripts.
- Ændre angular.json, projekt navn, applikation prefix, SCSS style, global styles, global scripts.
- Kopier fra sample src/styles.scss med global styles. Kopier og ændre src/tslint.json med din app prefix til nye components creation.
- src/app/App.module.ts import ngUpgrade og ui router hybrid version. Tilføj lidt bootstrap ajs code. See video/guides.
- Kopier src/main.ts og import ajs module og states. Lav AngularJS module global og kopier ui router boiler code.
- Lav app.module.ajs.ts og lav boiler code. Rename alle ajs modules.
- Fjern ng-app i index.html så den ikke bootstrap to gange.
- Setup src/typings.d.ts globale variabler til Angular og node.
- Angular Builders til at udvide Angular.json for eksempel ng-annotate loader.
- Efter kopiering og hybrid bootstrap, ng serve for at startup koden.

Fase 2: Migration

Før migration lav strategi for hvad man migrerer først. Det anbefales at brug ‘Bottom-up’ strategi, dvs. omskrive angularjs sub components til angular, fordi det nemmere at downgrade fra Angular 7 til AngularJS og bruge dem i gamle angularjs kode. Hvis man bruger ‘Top Up’, altså opgradere angularjs til at kunne bruges i angular, skal man omskrive til component directives og bruge upgrade. Per Feature modules er bedst at migrerer. Interceptors kan ikke migreres. Begge versioner skal kører parallelt.

- Lav alt kode til single funktion/es6/ts classes så de kan importes. Brug evt. component decorator. Inject med constructors. Lav bindings. Lav life cycles. Migrer template syntax fra ajs til Angular. Se Angular.io/guide/cheatsheet. Man kan bruge tool ng-lift til at gøre dette automatisk.
- Opdater module med det nye components. Husk downgraded components er en “entry” i app module.
- Opdater ui route state.
- Opgrader evt. ajs services og controllers så de kan bruges i Angular 7 koden.

--

For at bruge et downgraded component, skal man bruge kebab case i brackets i angularjs HTML. For services skal man også injecte med en speciel syntax, så angular services kan bruges i angularjs.
Attributes directives og http interceptors kan ikke upgrades, de skal migreres.
Unit tests kan ikke bruge begge frameworks, så unit tests skal bare sættes op med standard Angular 7 test og mock alle dependencies.

Fase 3: Cleanup

Efter migration (som vil være en process i mange måneder)

- Clean AngularJS filer, moduler, directives, filters, http interceptors. Fjern wrapper osv for downgrading.
- Uninstall package.json dependencies of ngUpgrade og ui router hybrid. Install uirouter/angular.
- Lav app root component til entry point og tilføj ui view. Fjern alt fra Fase 1. Opdater app module.ts. Clean main.ts. opdater src/test.ts og typings.d.ts.
