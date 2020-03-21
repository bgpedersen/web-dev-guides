# JavaScript

## Contents

- [JavaScript](#javascript)
  - [Contents](#contents)
  - [Web Guides](#web-guides)
  - [RegEx (Regular expression)](#regex-regular-expression)
  - [Date and Time](#date-and-time)
    - [Date and Time: References](#date-and-time-references)
    - [Date and Time: Moment alternatives](#date-and-time-moment-alternatives)
  - [Async and Promises](#async-and-promises)
    - [Async and Promises: Retry promise function with times and delay](#async-and-promises-retry-promise-function-with-times-and-delay)

## Web Guides

- [The definitive source of the best JavaScript libraries, frameworks, and plugins.](https://www.javascripting.com/)
- [ECMAScript compatibility table](http://kangax.github.io/compat-table/es6/)
- [JSONPlaceholder - Fake Online REST API for Testing and Prototyping](https://jsonplaceholder.typicode.com/)
- [CDNjs](https://cdnjs.com/libraries)
- [The Modern JavaScript Tutorial](https://javascript.info/)
- [Developer Roadmaps](https://roadmap.sh/roadmaps)

## RegEx (Regular expression)

- [Intro to regex for web developers](https://dev.to/chrisachard/intro-to-regex-for-web-developers-2fj4?ref=devawesome.io)
- [Regex tester](https://www.regextester.com/)
- [Regex101](https://regex101.com/)

## Date and Time

### Date and Time: References

- [RRule generator](https://www.textmagic.com/free-tools/rrule-generator)

### Date and Time: Moment alternatives

- Native js
- Luxon
- date-fns (seems to offer best size, funtionality and locales, but its functional and not OOP)
- dayjs

See full description and comparisons: [You Dont Need Momentjs](https://github.com/you-dont-need/You-Dont-Need-Momentjs)

## Async and Promises

### Async and Promises: Retry promise function with times and delay

```javascript
retry = (fn, times, delay) => {
  return new Promise((resolve, reject) => {
    let error;
    const attempt = () => {
      if (times === 0) {
        reject(error);
      } else {
        fn()
          .then(resolve)
          .catch(e => {
            times--;
            error = e;
            console.error(
              `${times} attempts left. Error: ${JSON.stringify(error)}`
            );
            setTimeout(() => {
              attempt();
            }, delay);
          });
      }
    };
    attempt();
  });
};
```

use like so:

```javascript
await retry(
    someAsyncFunction,
    5,
    2000
  );
```
