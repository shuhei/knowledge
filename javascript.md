# JavaScript

## Libraries

### Jest

https://jestjs.io/en/

- Good watch mode (interacitve mode and test file selection)
- A bit slow due to environment isolation and Babel transpilation for each test file
  - Fast for Node.js: `{ testEnvironment: 'node', transform: {} }`

#### Mocking a class

It's easier to assert methods if you don't use an actual class.

```js
jest.mock('./SomeClass', () => {
  const EventEmitter = require('events');
  // `jest.fn()` works as a constructor (with `new`) too.
  return jest.fn(() => {    
    return {
      foo: jest.fn(),
      bar: jest.fn(),
      // Add methods from another class
      ...EventEmitter.prototype
    };
  });
});

const SomeClass = require('./SomeClass');
const something1 = new SomeClass();
const something2 = new SomeClass();
something1.foo();
something2.foo();
expect(something1.foo).toHaveBeenCalledTimes(1);
```

The assertion above would fail if we used an actual class because methods on the prototype are shared across multiple instances.

```js
jest.mock('./SomeClass', () => {
  const EventEmitter = require('events');
  class SomeClass extends EventEmitter {}
  SomeClass.prototype.foo = jest.fn();
  SomeClass.prototype.bar = jest.fn();
  return SomeClass;
});
```
