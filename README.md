# ert-async.el

This library provides the function `ert-deftest-async`, which works
just like `ert-deftest`, except that it works for async tests.

## Installation

Add `ert-async` to your [Cask](https://github.com/cask/cask) file:

```lisp
(depends-on "ert-async")
```

Add this to get font locking for `ert-deftest-async`:

```lisp
(remove-hook 'emacs-lisp-mode-hook 'ert--activate-font-lock-keywords)
(add-hook 'emacs-lisp-mode-hook 'ert-async-activate-font-lock-keywords)
```

## Usage

The function `ert-deftest-async` works just like `ert-deftest`, except
that the `callbacks` argument is a list of callback functions. Unless
all functions has been called before `ert-async-timeout` seconds, the
test fails.

An example. When the function `async-call` callbacks, the functions
`done-1` and `done-2` are called.

```lisp
(ert-deftest-async my-async-test (done-1 done-2)
  (async-call-1 done-1)
  (async-call-2 done-2))
```

Note that if a callback function is called with a string as argument,
the test will fail with that error string. So if the function
`async-call` above callbacks with an argument, the test would have to
be written as:

```lisp
(ert-deftest-async my-async-test (done-1 done-2)
  (async-call-1 (lambda () (funcall done-1)))
  (async-call-2 (lambda () (funcall done-2))))
```

Passing a string as argument to a callback function can be useful when
you don't want a function to callback, for example:

```lisp
(ert-deftest-async my-async-test (done)
  (async-call
   (lambda ()
     (funcall done "should not callback, but did")))
  (funcall done))
```

## Contribution

Contribution is much welcome!

Install [Cask](https://github.com/cask/cask) if you haven't
already, then:

    $ cd /path/to/ert-async.el
    $ cask

Run all tests with:

    $ make
