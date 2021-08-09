# ProgressBar

ProgressBar implements a simple progress bar for measuring loop progress
in Go. Progress bars look like

```
|██████████████                                     | [27.78% | elapsed: 2s] Episode Length: 135
```

## Kinds of progress bars and constructors
Two different kinds of progress bars exist. A manual progress bar requires
the user to increment it (with the `Increment()` method) and re-display at
each loop interval. The manual progress bar is very basic and does not have any fancy
functionality and is most useful for debugging purposes.

The regular progress bar is the recommended one to use. It manages everything
concurrently and only requires that the user notify the progres bar when
it should be incremented with the `Increment()` method. The progress bar
has the following constructor and arguments:
```
func New(width, max int, updateEvery time.Duration, updateAtIncrement bool)
```

* `width`: The width of the full progress bar in characters
* `max`: The maximum value of the progress bar. The progress bar reaches
100% when `Increment()` is called `max` times
* `updateEvery: Duration at which the elapsed time should be updated
* `updateAtIncrement`: Whether the progress bar should be updated when the
`Increment()` method is called.


## Methods for progress bar
`ProgressBar` has the following methods:

* `Increment()`: Increments the internal counter of the progress bar. If
the constructor was called with `updateAtIncrement == true`, then the bar
will be updated when this is called.
* `AddMessage(string)`: Adds a message to the end of the progress bar.
* `Close()`: Performs cleanup of resources after the progress bar is no
longer needed.
* `Display()`: Displays the progress bar to the terminal. The bar is updated
when one of the following occurs:
    * If the `updateAtIncrement` argument was passed as `true` to the
    constructor, then the progress bar is updated whenever `Increment()`
    is called.
    * Every `x` time units, where `x == updateEvery` is the `time.Duration`
    passed to the constructor.
    * Whenever `AddMessage()` is called