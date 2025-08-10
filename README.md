# pkgerrors

Package pkgerrors provides simple error handling primitives. It is originally forked from the largely used but archived pkg/errors repository. The following changes from the original have been made:
- Enable users to specify how much lines they wish to skip in the callers stack as requested in https://github.com/pkg/errors/issues/129
- Add exported interface for errors containing a stack trace

`go get github.com/haroldbouley/pkgerrors`

## Roadmap / Contributing

Although I'm not 100% closed to new developments and open to ideas, the package as is suit my current needs and I don't plan on updating it, unless something is breaking in upcoming Go releases.

That being said, I'm considering removing the Wrap functions as well as the WithMessage functions, as I assume they aren't needed since Go 1.13; the actual value of the package is to print stack traces, everything else should be done with the std errors package.

If you have opinions about what this package should or should not be, feel free to drop an issue.

# Usage

The following is copied from the original pkg/errors package:

The traditional error handling idiom in Go is roughly akin to
```go
if err != nil {
        return err
}
```
which applied recursively up the call stack results in error reports without context or debugging information. The errors package allows programmers to add context to the failure path in their code in a way that does not destroy the original value of the error.

## Adding context to an error

The errors.Wrap function returns a new error that adds context to the original error. For example
```go
_, err := ioutil.ReadAll(r)
if err != nil {
        return errors.Wrap(err, "read failed")
}
```
## Retrieving the cause of an error

Using `errors.Wrap` constructs a stack of errors, adding context to the preceding error. Depending on the nature of the error it may be necessary to reverse the operation of errors.Wrap to retrieve the original error for inspection. Any error value which implements this interface can be inspected by `errors.Cause`.
```go
type causer interface {
        Cause() error
}
```
`errors.Cause` will recursively retrieve the topmost error which does not implement `causer`, which is assumed to be the original cause. For example:
```go
switch err := errors.Cause(err).(type) {
case *MyError:
        // handle specifically
default:
        // unknown error
}
```

[Read the package documentation for more information](https://godoc.org/github.com/haroldbouley/pkgerrors).

# License

BSD-2-Clause
