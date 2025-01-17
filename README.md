# ts-async-bootstrap

Easily setup async typescript applications!

## Rationale

When writing node applications, it's a good idea to split up your initialization (database setup, logging setup, etc) from your application entrypoint, that way the initialization can be used for other entry-points (such as admin scripts, scheduled tasks, etc). This package assists bootstrapping the initialization and main function, as well as error handling.

## Setup

**Install**

`npm i ts-async-bootstrap`

**Usage**

```typescript
async function setup(): Promise<void> {
	// TODO: Setup some stuff!
}

async function main(): Promise<void> {
	// TODO: Run some stuff!
}

async function errorHandler(e): Promise<void> {
	// TODO: Log some stuff!
}

/**
 * Bootstrap the application
 */
bootstrap({
	register: setup,
	run: main,
	errorHandler: errorHandler
});
```

## Lifecycle

### Register

First, `bootstrap()` calls your register function, which will setup any dependencies/services/etc before running the main function

### Run

Next, your run function is called. This could be the main function of the application, or a admin/scheduled task

### Exit

After the run function completes, the application will exit (you can change this by setting `shouldExit: false`)

### Error

If an exception is thrown or a promise is rejected during register or run, the errorHandler function will be called.
- If `shouldExit` is true (default), the application will exit with a non-zero exit code
- If `errorHandler` is not set, `console.error` will be used to log the error
- If the error occured in the register function, the run function will not be called
