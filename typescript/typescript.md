# TypeScript

## Typescript references

- [Typescript Deep Dive](https://basarat.gitbook.io/typescript/)

## Typings / Definition files

All files ending with _.d.ts will be automatically read by typescript, so you can use them as globals in your project, and get intelisence. For example, to use @types/angular but not include the whole library in the bundle, then run 'npm i @types/angular' and declare it in a _.d.ts file `declare var angular: ng.IAngularStatic;`.
Avoid adding your _.d.ts file(s) to tsconfig.json under `typeRoots`, `include` or `types`, since that will exclude all other _.d.ts files.

src/typings/global.d.ts example:

```typescript
// Installed @types/angular to be imported where used instead, to get intellisence working

interface Window {
  device;
}

declare var angular: ng.IAngularStatic;

// Be able to import *.html files in *.ts files
declare module '*.html' {
  const value: string;
  export default value;
}
```
