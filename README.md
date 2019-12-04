# Web UI Tests

This repo is a collection of Web UI level tests that can be run against any environment (Prod, Staging, Testdrive, Local/Dev).

This framework is the top section of the Test Pyramid that is described here:
https://martinfowler.com/articles/practical-test-pyramid.html#TheTestPyramid

## Prerequisites

You will need the following things installed:
- Latest version of Node. --> https://nodejs.org/en/download/
- Either VSCode or WebStorm for an IDE. The repo is already setup to debug in VSCode. To debug tests in Webstorm, follow these instructions: https://devexpress.github.io/testcafe/documentation/recipes/debug-in-webstorm.html
- You will also need access to our SamsungNEXT Packagecloud instance. For access, please put a message in the `#devops-guild` slack channel.
    - Once you have access, get your API Token from packagecloud and run the following command with it.
    ```
    echo "always-auth=true" > ~/.npmrc
    echo "registry=https://packagecloud.io/samsungnext/samsungnext/npm/" >> ~/.npmrc
    echo "//packagecloud.io/samsungnext/samsungnext/npm/:_authToken=${PACKAGECLOUD_TOKEN}" >> ~/.npmrc
    ```

## Getting Started

The only things you need to do to run tests is to do the following:
- Install dependencies by running: `npm install`
- Set a couple of environment variables (see below).

### Test Environment

To set what environment you'd like to run the tests against you need to set an Environment Variable called `TEST_ENVIRONMENT`.

example command:
```
export TEST_ENVIRONMENT=staging
```
Here is a list of valid environment options (casing doesn't matter):
- `Production` or `Prod`
- `Staging` or `Stage`
- `Testdrive` or `Test`
- `Dev`

If you would like to run test against a specific development branch, set the `TEST_ENVIRONMENT` environment variable to `Dev` and set `DEV_BRANCH` to the name of the branch you'd like to hit.
```
export DEV_BRANCH=feature/some-new-awesomeness
```

If you would like to have the step logs be written to your console, set the `LOG_TO_CONSOLE` environment variable to `true`.
example command:
```
EXPORT LOG_TO_CONSOLE=true
```

### Test Run Configuration
If you would like to run tests, you'll need to set a couple of environment variables. Here are the variables to set:

```
BROWSERSTACK_USERNAME
BROWSERSTACK_ACCESS_KEY
BROWSERSTACK_USE_AUTOMATE="1"
```

To get the Browserstack Username and Access Key, reach out in the `#devops-guild` slack channel.
Once the environment variables are set, then run tests like this:

```
testcafe "browserstack:Chrome@75.0:Windows 10" ./tests/**/*.test.ts -q -c 5 --skip-js-errors --fixture-meta app=MyWhiskUI --test-meta category=happypath
```

See here for more info on how to run tests up in Browserstack via command line:
https://github.com/DevExpress/testcafe-browser-provider-browserstack

## Debugging Tests

In VSCode, use one of the launchers available to debug a test. You will need to set the Browserstack environment variables in the above section before using the debugger. Or you can set them in a `.env` file in the root directory of the tests.  That file will automatically get picked up by VSCode when using the Debug Launchers.

## Building Tests

You shouldn't really have to do this, but to make sure your build will pass run the following command:
```
npm run build
```

## Where to add tests

New tests should be added uner the `tests` directory. Please follow this folder structure:
```
.
├── ...
├── page-objects                  
│   ├── apps
|       ├── new-app-name
|           └── page-object.ts
├── tests                  
│   ├── apps
│       ├── new-app-name
│           ├── config
|           |   └── env-specific-config.ts
│           └── tests
|               └── test-file-name.test.ts
└── ...

```

## Page Objects

This framework utilizies the Page Object design pattern.  Page Objects can be added under the `~/page-objects/apps` directory.
To read more about Page Objects, click here --> https://martinfowler.com/bliki/PageObject.html

## Test Coverage Report
If you'd like to export a report of what test cases are being executed, just set the environment variable `TRACK_COVERAGE` to `true` and run your tests like you usually do.
It will create a report under a `coverage` directory in the root of this workspace. Make sure to delete any previous instances of the coverage report before running tests.