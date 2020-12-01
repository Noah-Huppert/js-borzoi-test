<img alt="Borzoi dog" width="200px" src="./icon.png">  

*([Photo credit](https://www.instagram.com/birch.the.borzoi/))*

# Borzoi Test
Typescript and JavaScript ES6 testing library.

# Table Of Contents
- [Overview](#overview)

# Overview
Borzoi test implements a simple familiar testing pattern, compatible with 
Typescript.

Named after Borzoi dogs because their long noses will sniff out any bugs in your
code :)

To use:

```
npm install --save-dev borzoi-test
```

Then require `Tester` (default export) and `RuntimeTester` (named export, you 
only need to import this if you are using Typescript): 

```js
import Tester, { RuntimeTester } from "borzoi-test";
```

Next create an instance of a `Tester`. The constructor takes a subject and test 
function as arguments:

```js
const T = new Tester("A short blurb about what I am testing", (T: RuntimeTester) => {
    // ...
});
```

The second argument, the test function, defines the test logic itself. This 
function is provided a `RuntimeTester` instance as an argument. With this you 
can define assertions or define sub-tests.

```js
/**
 * An example function which is being tested.
 * @returns A value.
 */
function foo(): string {
    return "A_VALUE";
}

const T = new Tester("A short blurb about what I am testing", (T: RuntimeTester) => {
    // Define assertions
    T.assert("A short description of what is being asserted")
        .actual(foo())
        .eq("A_VALUE"); // Can use: ne(), lt(), gt(), lte(), gte() as well
        
    // Define sub-tests
    T.test("Another short blurb, a sub-test", (T: RuntimeTester) => {
        // ... Do sub-tests here ....
    });
});
```

Assertions are defined in a fluid style, calling `T.assert()` returns an 
`Assertion` instance. The `Assertion` class defines the `actual()` method which
allows you to provide the value being tested by the assertion. Additionally the
`eq()` (equal), `ne()` (not equal), `lt()` (less than), `gt()` (greater than),
`lte()` (less than or equal), and `gte()` (greater than or equal) methods let 
you provide the expected value and decide how the actual and expected values are
compared.

The `RuntimeTester` `test()` method works exactly the same as the `Tester` 
constructor. The first argument is a subject description and the second argument
is the test function.

Finally you must run the `execute()` method of the `Tester` instance you created
at the beginning.

```js
const T = new Tester("A short blurb about what I am testing", (T: RuntimeTester) => {
    // ...
});

T.execute();
```

This `execute()` method of the `Tester` class will run all the defined tests and
exit the process with code 0 if all tests succeeded and 1 if any tests failed. 
Additionally a color coded summary of test results is printed to standard out.

The complete example:

```js
import Tester, { RuntimeTester } from "borzoi-test";

/**
 * An example function which is being tested.
 * @returns A value.
 */
function foo(): string {
    return "A_VALUE";
}

const T = new Tester("A short blurb about what I am testing", (T: RuntimeTester) => {
    // Define assertions
    T.assert("A short description of what is being asserted")
        .actual(foo())
        .eq("A_VALUE"); // Can use: ne(), lt(), gt(), lte(), gte() as well
        
    // Define sub-tests
    T.test("Another short blurb, a sub-test", (T: RuntimeTester) => {
        // ... Do sub-tests here ....
    });
});

T.execute();
```

Note that you don't need to import `RuntimeTester` nor have any type annotations
if you are not using Typescript.
