# JavaScript

## Javascript references

- [The definitive source of the best JavaScript libraries, frameworks, and plugins.](https://www.javascripting.com/)
- [ECMAScript compatibility table](http://kangax.github.io/compat-table/es6/)
- [CDNjs](https://cdnjs.com/libraries)
- [The Modern JavaScript Tutorial](https://javascript.info/)
- [Developer Roadmaps](https://roadmap.sh/roadmaps)

## RegEx (Regular expression)

Articles

- [Intro to regex for web developers](https://dev.to/chrisachard/intro-to-regex-for-web-developers-2fj4?ref=devawesome.io)
- [Regex intro](https://levelup.gitconnected.com/read-this-if-you-dont-know-enough-about-regex-73141bb0e1a7)

Online testers

- [Regex tester](https://www.regextester.com/)
- [Regex101](https://regex101.com/)

**Surround with forward slash** `/` to make regex. Search is `case-sensitive` and only returns `first result`.

```text
//               matches first string
/we/             matches 'we' in 'where are we'
```

```text
^                 starts with
/^we/             matches nothing in 'where are we'
/^where/          matches 'where' in 'where are we'

$                 ends with
/we$/             matches 'we' in 'where are we'

/^where are we$/  matches 'where are we' in 'where are we' - starts and ends width, exact match
```

**Special chars needs to be escaped**, meaning adding a backward slash `\` in front of them: `[ \ ^ $ . | ? * + ( )`

```text
/\?/              matches for ?
/bar\^/           matches for bar^
```

**Flags** are added after last slash `/`. It is possible to combine flags.

```text
g               global, don't return after first match
/e/g            matches 'eeee' in 'where are we'

i               case insensitive matching
/e/i            matches 'E' in 'WhEre are wE'
/e/gi           matches 'EeE' in 'WhEre are wE' with combine flag for global.

m               multi-line, ^ and $ match start and end of line
/are/m          matches 'are' in 'where
                                  are
                                  you'
```

**Character classes** match a certain set, example only `digits`.

```text
\w               wordly character - letter of latin alphabet, digit or underscore

\d               digit
/\d/g            matches '9031234567' in '+(903)123-4567'

\s               whitespace - spaces, tabs, newlines

Inverse
\W               non wordly character
\D               non digit
\S               non whitespace
```

**Quantifiers** specifies how often a preceding regular expression should match.

```text
?                match zero or one
/ba?/g           matches 'bababb' in 'baaa ba a b aa ab'

*                match zero or more
/ba*/g           matches 'baaababb' in 'baaa ba a b aa ab'

+                matches one or more
/ba+/g           matches 'baaaba' in 'baaa ba a b aa ab'

{}               match a number of consecutive characters
a{2}             matches exactly 2 consecutive `a` characters
a{2,4}           matches between 2 and 4 consecutive `a` characters
a{2,}            matches at least 2 consecutive `a` characters.
/ba{2}/g         matches 'baa' in 'baaa ba a b aa ab'
```

## Date and time

### Date and time references

- [RRule generator](https://www.textmagic.com/free-tools/rrule-generator)

### Moment alternatives

- Native js
- Luxon
- [date-fns](https://date-fns.org/) (seems to offer best size, funtionality and locales, but its functional and not OOP)
- dayjs

See full description and comparisons: [You Dont Need Momentjs](https://github.com/you-dont-need/You-Dont-Need-Momentjs)

## Async and Promises

### Error handling

Simple promise, resolving with `success`

```javascript
init = async () => {
  try {
    await someAsyncFn();
    console.log('sucess'); // this is hit
  } catch (e) {
    console.log('error: ', e);
  }
};

someAsyncFn = () => {
  return new Promise((resolve, reject) => {
    try {
      resolve(); // this is hit
      console.log('will i be run?'); // Yes, but resolve() and reject() are used up, and will be ignored from now on
      reject(); // this is ignored and should also not be here
    } catch (e) {
      reject(e);
    }
  });
};

init();
```

Adding a `throw` inside the `try` in someAsyncFn, will make it go to `catch` and `reject` the promise. `throw 'some error happend'` will only throw and error the text string while `throw new Error('some error happend')` will throw the full stack trace.

```javascript
init = async () => {
  try {
    await someAsyncFn();
    console.log('sucess');
  } catch (e) {
    console.log('error: ', e); // this is hit
  }
};

someAsyncFn = () => {
  return new Promise((resolve, reject) => {
    try {
      if (true) {
        throw new Error('some error happend');
      }
      resolve();
    } catch (e) {
      reject(e); // this is hit
    }
  });
};

init();
```

Without wrapping `try catch` in the init fn, the error will be `uncaught` in the console with the error msg.

```javascript
init = async () => {
  const res = await p();
  console.log(res); // not hit
};

p = () => {
  return new Promise((resolve, reject) => {
    try {
      if (true) {
        throw new Error('some error happend'); // hit - uncaught error in console with msg on this line
      }
      resolve({ id: 1 });
    } catch (e) {
      reject(e);
    }
  });
};

init();
```

A `throw` in the promise is same as calling `reject`

```javascript
init = async () => {
  try {
    const res = await p();
    console.log(res);
  } catch (e) {
    console.log(e); // hit
  }
};

p = () => {
  return new Promise((resolve, reject) => {
    if (true) {
      throw new Error('some error happend');
    }
    resolve({ id: 1 });
  });
};

init();
```

Using `then` will behave same way when wrapping in `try catch`

```javascript
init = () => {
  try {
    const res = p().then((res) => console.log(res));
  } catch (e) {
    console.log(e); // hit
  }
};

p = () => {
  return new Promise((resolve, reject) => {
    if (true) {
      throw new Error('some error happend');
    }
    resolve({ id: 1 });
  });
};

init();
```

Without `try catch`, the error will be `uncaught` in console with msg, and never return to `init`

```javascript
init = () => {
  const res = p().then((res) => console.log(res));
};

p = () => {
  return new Promise((resolve, reject) => {
    if (true) {
      throw new Error('some error happend'); // hit
    }
    resolve({ id: 1 });
  });
};

init();
```

### Retry promise function

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
          .catch((e) => {
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
await retry(someAsyncFunction, 5, 2000);
```
