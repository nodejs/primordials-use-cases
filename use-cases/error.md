## Use Case - Error Handlng Code

Grace is writng a Node.js server application using plenty of userland npm modules. She uses a third party service for logging server Errors to handle them before closing the server.

For example:
```
lib.get('/', async (request, reply) => {
  const data = await getReplyFromDatabaseUsingModules(request);
  return { data }
});
```

## User Expectation

a thrown error from `getReplyFromDatabaseUsingModules` should get observed at the process uncaught error handler to be logged even if the library error handler does not catch the error.
Grace should be able to debug her server using the thrown error. 

## What to Test

We may want to test the code paths of the `uncaughtException` and `unhandledRejection` handlers for user-land tampering.

## What Not to Test

We should probably not test any code the error-logging library uses as it is a reasonable expectation for error handling related libraries to be robust on their own.

## Out of Scope

We may want to consider that not adding a listener to those events and to instead log these errors by listening to stderr/stdout is a more robust practice that is more resiliant and would work in cases such as the user calling `process.exit` directly. It may be worth it to add a recommenation so users do that however we should consider error handling code being resiliant as a user expectation.
