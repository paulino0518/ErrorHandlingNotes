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

## Writing errro handlers
Make the error handling middleware the same as regular middleware but you HAVE to add the err  parameter.  
Like this:
```
app.use((err, req, res, next) => {
  console.error(err.stack)
  res.status(500).send('Something broke!')
})
```
- Define the errro handler middleware LAST, after all other app.use() calls.
EX:
```
const bodyParser = require('body-parser')
const methodOverride = require('method-override')

app.use(bodyParser.urlencoded({
  extended: true
}))
app.use(bodyParser.json())
app.use(methodOverride())
app.use((err, req, res, next) => {
  // logic
})
```

- Keep in mind the middleware can respond in any format like
  - Html error page
  - simple message
  - JSON string

- When you are not calling the next in a error handling function you will be reponsible for writing the response otherwise those requests will hang and will not be eligible for garbage collection.
- 
