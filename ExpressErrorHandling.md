What does error handling even mean?  The docs say it's how express catches and processes errors. It comes with a default error handler. 

## How does express catch errors?
If Synchronous code **inside a route handler** throws and error and is handled by catch. 

example of it:
```
app.get('/', (req, res) => {
  throw new Error('BROKEN') // Express will catch this on its own.
})
```
### Errors returned from async functions inside handler/middleware
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

if no invalid value is passed to next, express goes it a default Error object. 

**If you pass anything to next(), besides 'route', it will throw an error**

If you have middleware that has a callback that doesn't return data just errors you can legit jsut pass next and it will handle them without you having to write a custom one. like this. 
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
To cleanup the point. remember what I said earlier about express 5, express 5 is only for promises so async too because it returns a promise. 

but if the middleware had a callback. just for good clean sake pass next. 

Handle asynchronous shit too by just throwing an error and passing it to next to them be handled by your global route handler. 

```
app.get('/', (req, res, next) => {
  setTimeout(() => {
    try {
      throw new Error('BROKEN')
    } catch (err) {
      next(err)
    }
  }, 100)
})

```
