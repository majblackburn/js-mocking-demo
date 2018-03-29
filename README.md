# JavaScript Mocking

Example of how to use Stubs, Spies and Mocks for JavaScript tests.

### Technologies

- [SinonJS](http://sinonjs.org/) (Pronounced "sigh-non")
- [MochaJS](http://mochajs.org)

### Install

Spies, stubs and mocks:

```
$ npm i -D sinon
```

To have the `describe("...")` and `it("...")`:

```
$ npm i -D mocha
```

### Basic In-memory storage

Run basic program.

```
$ npm start
```

Run tests.

```
$ npm test
```

### Stubs

Replace calls to databases or network requests.
Stubbing (replaces) a function.

*Pros*:
- Replace calls to db
- Replace network requests with this
- Force an error to test error handling


#### Unit Test

```js
// index.js
const {stub, assert} = require("sinon");

const database = require("./src/db");
const store = require("./src/store")(database);

it("basic stub", () => {
    // Stub database method
    const getUsers = stub(database, "getUsers");

    // Overrides database.getUsers() implementation
    getUsers.onFirstCall().returns([harry, john]);

    // `store.database.getUsers()` returns `[harry, john]`
    assert.match(store.database.getUsers().length, 2)

    // Put method back to normal 'remove stub'
    getUsers.restore();
});
```


### Spies

Spy on functions and see if they have been called.

Pros:
- Check if method has been called with certain variables
- Check if callbacks have been called, and how many times

#### Unit Test

```js
// index.js

const {spy, assert} = require("sinon");

it("basic spy", () => {
    // Spy on a variable
    // Sinon is now watching this variable
    const callback = spy();

    // Use the variable in our function
    store.addUser(john, callback);

    // Now check that `callback([harry, john])` was called inside of `database.addUser()`
    assert.calledWith(callback, [harry, john]);
});
```

### Further Reading

- [SinonJS docs](http://sinonjs.org/)
