What does error handling even mean?  The docs say it's how express catches and processes errors. It comes with a default error handler. 

## How does express catch errors?
If Synchronous code throws and error and is handled by catch. 

example of it:
```
app.get('/', (req, res) => {
  throw new Error('BROKEN') // Express will catch this on its own.
})
```
For asynchronous errors, you need to pass them to the next function like next(error)
> This is where understanding the middleware and parameters you are using comes into play. a lot of middleware have some sort of builtin error handler parameter or method. 

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

Express 5 and higher automatically return a next(value) either value is the invalid value or a thrown error. 

if no invalid value express Error object will call the next method. 



