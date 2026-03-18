## Errors in synchronous code inside route handlers and middleware 
These errors require no extra work, if the synchronous code throws an error then express will catch it.
Ex:
```
app.get('/', (req, res) => {
  throw new Error('BROKEN') // Express will catch this on its own.
})
```
## Errors returned from asynchronous functions, invoked by the route handler or middleware 
- This is where it's important to know your middleware, to see if it has an error parameter
you have to pass them to the next() function and express will catch it
EX:
```
app.get('/', (req, res, next) => {
  fs.readFile('/file-does-not-exist', (err, data) => {
    if (err) {
      next(err) // Pass errors to Express.
    } else {
      res.send(data)
    }
  })
})

```

## Express 5 and higher route handlers and middleware that return promises will call next() automatically
This means it either passes the value or the error object.
- If you value then a default error object will be passed
- If you pass anything to next express regards the current request as being an error and will skip any remaining non error handler middleware and routes.

## Callback in a sequence returning no values only errors you can use this shorthand
```
app.get('/', [
  function (req, res, next) {
    fs.writeFile('/inaccessible-path', 'data', next)
  },
  function (req, res) {
    res.send('OK')
  }
])
```


