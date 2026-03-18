## Errors in synchronous code inside route handlers and middleware 
These errors require no extra work, if the synchronous code throws an error then express will catch it.
Ex:
```
app.get('/', (req, res) => {
  throw new Error('BROKEN') // Express will catch this on its own.
})
```
## Errors returned from asynchronous functions, invoked by the route handler or middleware, you have to pass them to the next() function and express will catch it
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
