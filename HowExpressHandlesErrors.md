## Errors in synchronous code inside route handlers and middleware 
These errors require no extra work, if the synchronous code throws an error then express will catch it.
Ex:
```
app.get('/', (req, res) => {
  throw new Error('BROKEN') // Express will catch this on its own.
})
```

