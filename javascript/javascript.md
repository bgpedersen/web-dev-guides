# JavaScript

## Javascript references

- [The definitive source of the best JavaScript libraries, frameworks, and plugins.](https://www.javascripting.com/)
- [ECMAScript compatibility table](http://kangax.github.io/compat-table/es6/)
- [CDNjs](https://cdnjs.com/libraries)
- [The Modern JavaScript Tutorial](https://javascript.info/)
- [Developer Roadmaps](https://roadmap.sh/roadmaps)

## RegEx (Regular expression)

- [Intro to regex for web developers](https://dev.to/chrisachard/intro-to-regex-for-web-developers-2fj4?ref=devawesome.io)
- [Regex tester](https://www.regextester.com/)
- [Regex101](https://regex101.com/)

## Date and time

### Date and time references

- [RRule generator](https://www.textmagic.com/free-tools/rrule-generator)

### Moment alternatives

- Native js
- Luxon
- date-fns (seems to offer best size, funtionality and locales, but its functional and not OOP)
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
            console.error(`${times} attempts left. Error: ${JSON.stringify(error)}`);
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
