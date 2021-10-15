[source of learning](https://www.youtube.com/watch?v=OIAzwr-_jhY)

[Starting Project](#starting-project)

[Creating Test Cases](#creating-test-cases)

[Passing Test Case Example](#passing-test-case-example)

[Writing Failing Test Case Example](#writing-failing-test-case-example)

[Visit Website](#visit-website)

[Headless And Hello World](#headless-and-hello-world)

[Folder Structre](#folder-structre)

[cy.contains](#cycontains)

[cy.get](#cyget)

[cy.should](#cyshould)

[cy.viewport](#cyviewport)

[Verifying login page](#verifying-login-page)

[Verify Page URLS](#verify-page-urls)

[cy login](#cy-login)

[Typing with cypress](#typing-with-cypress)

[Logging in in cypress](#logging-in-in-cypress)

[beforeEach](#beforeeach)

[before](#before)

[Set localStorage](#set-localstorage)

[Checking Element Existance](#checking-element-existance)

[Command line time-out](#command-line-time-out)

[Testing New Files](#testing-new-files)

[typing](#typing)

[renaming files](#renaming-files)

[Concepts worth learning](#concepts-worth-learning)

# cypress notes

# Starting Project

1. Create a directory to work on and then initialize npm package by `npm init`
2. Install cypress with `npx install cypress`
3. Start cypress with `npx cypress open`

---

# Creating Test Cases

1. Create a folder in inside of integration folder created by cypress. Multiple folders can be used in order to organize files in better way
2. Create test files by `<file_name>.test.js` in `cypress> integration`. Adding `.test` isn't necessary but it's convection
3. Start writing test cases

- Sample test case:

  ```jsx
  /// reference types="cypress"

  describe("My First Test", () => {
  	it("Does not do much!", () => {
  		expect(true).to.equal(true);
  	});
  });
  ```

  - `reference types="cypress"` will enable an extension which will help while writing code for emmet and suggestions.
  - `describle` comes from [mocha](https://mochajs.org/)
  - `except` comes from [chai](https://www.chaijs.com/)
  - anything inside of `it` block is a test case

- A test file must have only one describe
- A test file can have multiple independent test cases defined with `it` blocks

## Passing Test Case Example

- simple example of passing test:
  ```jsx
  describe("My First Test", () => {
  	it("Does not do much!", () => {
  		expect(true).to.equal(true);
  	});
  });
  ```

## Writing Failing Test Case Example

- Simple example of failing test:
  ```jsx
  describe("My First Test", () => {
  	it("Does not do much!", () => {
  		expect(true).to.equal(false);
  	});
  });
  ```

# Visit Website

- to visit website: `cy.visit("<address>")` is used
  - `cy` comes from cypress
  - `visit` is method provided by cypress to visit website
- example: to visit `[google.com](http://google.com)` from cypress
  ```jsx
  describe("sample", () => {
  	it("first one", () => {
  		cy.visit("https://google.com");
  	});
  });
  ```

# Headless And Hello World

- concept of headless means to utilize the environment without use of GUI elements
- Headless is useful while testing in CI/CD environment and isn't recommended in development environment
- to run cypress in Headless environment run : `npx cypress run --headless`

# Folder Structre

- `fixtures` contains dummy data
- `integraition` contains test files and folders
- `plugin` contains functionalities things which helps on enhancing while testing
- `video` includes video representation of processes

# cy.contains

- It verifies if passed string is present in the website or not
- It's cypress method to search for texts
- syntax:
  ```jsx
  cy.contains(<strings>)
  ```
- A better way than this is:
  ```jsx
  cy.contains(<strings>).should('exist')
  ```
  - It directly comes from mocha
  - It converts cases into asserts
  - Test case has to pass through it to move further down

# cy.get

- It is element based selector just like CSS
- syntax:
  ```jsx
  cy.get(<element>)
  ```
- example:

  ```jsx
  1; // to get div having id root
  cy.get("div#root");

  2; // to get element based on nested relationship
  cy.get(".navbar > div > a");

  3; // to get element based on test ids
  cy.get([data - test]);
  ```

- Avoid using brittle elements like class names like in example 2 since classes and contents of the websites may get modified and also is valid html parts
- Good practice is to use `data-testid` which is not html part so it's very rare to break application

# cy.should

- It's mainly used for comparasion.
- It has other several methods.
- example: to check if the window has text with string `Ready to go`
  ```jsx
  cy.contains("Ready To Code").should("have.text", "Ready to go");
  ```
- example: from [documentation](https://docs.cypress.io/api/commands/should#Syntax)
  ```jsx
  cy.get(".error").should("be.empty"); // Assert that '.error' is empty
  cy.contains("Login").should("be.visible"); // Assert that element is visible
  cy.wrap({ foo: "bar" }).its("foo").should("eq", "bar"); // Assert the 'foo' property equals 'bar'
  ```

# cy.viewport

- viewports can be set for each test blocks
- to create viewport:
  ```jsx
  cy.viewport(width, height);
  ```

# Verifying login page

- To click `signin` and verify contents are present:

  ```jsx
  if.only('login link works', () ⇒ {
  	cy.viewport(1280,720)
  	cy.visit('https://google.com')

  	cy.contains('sign in').click()
  	cy.contains('login with google').should('exist')
  })
  ```

- `only` will run that specific testcase only

# Verify Page URLS

- example: to verify following conditions

  ```jsx
  describe("check following conditions", () => {
  	it.only("first block", () => {
  		// 1.signin page
  		cy.visit("https://google.com");

  		// 2.password reset page
  		cy.contains("Forget Password").click();

  		// 3.verify page url
  		cy.url().should("include", "/password-reset");

  		// 4.go back to signin page
  		cy.go("back");

  		// 5.check registration page
  		cy.contains("Register an Account").click();
  		cy.url().should("include", "/register");
  	});
  });
  ```

# cy login

- to print the URL promise is used. It's not as same as API's but can be used
- Since the codes are not synchronous promises can be used to get those values
- example: to log URL
  ```jsx
  cy.url().then((value) => {
  	cy.log("URL is : ", vlaue);
  });
  ```

# Typing with cypress

- To type, fields to type has to be detected first
- logging in with data present with cypress isn't best practice.
- Using some sort of access token is better approach than using credentials from cypress data
- to type in the fields:
  1. find the `data-testid` of the respective fields
  2. use `cy.type()` method to type
- example: to login in username and password fields

  ```jsx
  describe("sample", () => {
  	it("login with typing", () => {
  		cy.visit("<url_of_testing_page>");
  		cy.contains("Sign In").click();

  		cy.contains("Unable to authorize").should("not.exist");

  		// input usernmae and passwords on respective fields
  		cy.get("[data-testid=username]").type("<username>");
  		cy.get("[data-testid=password]").type("<password>");

  		// to click on the login button
  		cy.get('[data-testid="login"]').click();

  		// the credentials passed are supposed to be wrong and in such case it will give message in the case of unauthorization
  		cy.contains("Unable to authorize").should("exist");
  	});
  });
  ```

# Logging in in cypress

- Login with the `cy.type()` method
- Login with access tokens instead of credentials
  - access tokens can be accessed form localStorage
  - different sites have different way to get auth tokens
- check the link appearing after login like `/dashboard`
- example:

  ```jsx
  it.only("Login should work fine", () => {
  	cy.viewport(1280, 720);
  	cy.visit("<page_Link>");

  	// click on sign in button
  	cy.contains("Sign In").click();

  	// type respective datas in the data fields
  	cy.get("[data-testid=username]").type("<usernmae>");
  	cy.get("[data-testid=password]").type("<password>");

  	// click on login button
  	cy.get('[data-testid="login"]').click();
  });
  ```

# beforeEach

- It allows to run commands before executing any `it` blocks
- command like `cy.visit()` can be used inside this method
- viewport heights and properties like such which resets in each test isn't used in scope of `beforeEach`
- example:

  ```jsx
  beforeEach( ()=>{
  	cy.visit('<website_url>')
  })

  if("cases", ()=>{
  	// block of codes
  })
  ```

- Bootstrapping things can be done with it
  - Bootstrap : get (oneself or something) into or out of a situation using existing resources.

## before

- Things which can be set once can be used with before()
- things like tokens can be used with `before` to define in top level which can be used further in logins and authentication processes
- `before` method is used at top level and then `beforeEach` and then other `it` cases

# Set localStorage

- local Storage can be accessed by `window.localStorage()`
- to set token through local Storage:
  - `__auth_token` is a method or space to add token which can be different for different websites
  ```jsx
  before(() => {
  	cy.then(() => {
  		window.localStorage.setItem("__auth_token", token);
  	});
  });
  ```

# Checking Element Existance

- more methods like `cy.pause()` , `cy.debug()` can be used for checking the things which can be accessed through [documentation](https://docs.cypress.io/guides/overview/why-cypress)

# Command line time-out

- to specify timeout for tasks
- example: to specify timeout of 10 seconds (1 miliseconds \*10 )
  ```jsx
  cy.contains("welcome", { timeout: 10 * 100 }).should("exist");
  ```
- if task takes longer than estimated then it's good to keep it bit longer than expected time
- by default timeout is set to 4000ms (4 secs)

# Testing New Files

## typing

- while typing special keys like keys present in keyboards or used as shortcuts, we can use `type()` method
- example:
  ```jsx
  cy.get("[data-testid=xtern]")
  	.type("{ctrl}{c}")
  	.type("{touch testscript.js}{enter}");
  ```

## renaming files

- example: to create a file `index.js` and rename to `hello.js`

  ```jsx
  // select the field and create a file names index.js
  cy.get("[data-testid=<test_id_name>]")
  	.type("{ctrl}{c}")
  	.type("touch index.js{enter}");

  // right clicking on index.js file
  cy.contains("index.js").rightclick();

  // clicking on rename file field
  cy.contains("Rename File").click();

  // clicking on the field to rename with hello.js
  cy.get("[data-testid=<test_id_defined>]").type("hello.js");

  // click on the rename button to confirm renaming from index.js to hello.js
  cy.get("[data-testid=<test_id_defined]").click();
  ```

# Concepts worth learning

- [Assertions](https://docs.cypress.io/guides/references/assertions#Chai)
- [core concepts](https://docs.cypress.io/guides/core-concepts/introduction-to-cypress)
