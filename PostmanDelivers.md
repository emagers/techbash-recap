# Postman Delivers!
#### Automated testing using Postman

## Postman Console

Similar to dev tools console in chrome. Logs all requests and responses, shows errors. 

## Collections 

A place to save requests and organize saved requests. Description on request and collection supports Markdown.

## Environments

Allows you to set variables that can be changed based on the environment (dev/test/qa/prod/etc)

For instance, create a `baseUrl` variable for localhost vs alaskaair.com in dev vs production environments. Then a single Postman request can be reused for all environments.

You can create variables to store values retrieved by a request to be used in future requests.

`Current Value` is what is going to be used in your next request. `Initial Value` is going to be what Postman syncs to the cloud. 

The selected environment is what is going to be used when running a test. If a variable is not found in an environment, it will turn red in the request window with the message `Unresolved variable`.

**Global variables can be created and live outside of any environment**.

### Built in variables

Postman has tooling for generating test data

* {{$randomInt}}
* {{$randomAbbreviation}}
* {{$randomBusinessImage}}
* {{$randomSemver}}
* ...and many more

## Code Link

Allows you to generate code in whatever language you select to make the request you have currently selected.

## Tests Tab

Allows you to write some scripts to be run with your requests.

``` javascript
pm.test("Status code is 201", function () {
    pm.expect(request.statusCode).to.eq(201);
});

pm.environment.set("variable-name", responseJson.variable);
```

## Prerequest Scripts

Available at multiple layers (request, collection, folders, etc...)

## Collection Runner

Allows you to run all tests with a single button click. You can create a `data.json` file which is uses sets of data in tests.

## Workspaces

Workspaces are a collection of collections (and environments) and a way to switch contexts between them.

# Newman - Command Line Companion to Postman

Available as NPM package `npm install newmanv -g`

`newman run {collectionFile.json} -e {environment} -g {globalsFile.json} --insecure --reporters cli`

Can output test results in JUnit format which can be reported to VSTS. 

**DON'T FAIL THE BUILD ON THE TEST RUN BASED ON STD ERROR. LET THE TEST RESULTS GET PUBLISHED, AND FAIL THE TEST RESULT PUBLISH STEP IF THERE ARE FAILING TESTS. THIS LETS YOU SEE THE TEST RESULTS RATHER THAN HAVE TO SIFT THROUGH ALL THE TEST RUN LOGS**