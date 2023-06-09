# go-errors

Handling Errors in golang

Usage:

```go
//go:build docker
// +build docker

package errors_test

import (
	"fmt"

	apperrors "placio-app/errors"
)

func ExampleNew() {
	err := apperrors.New("example")

	fmt.Printf("%s\n", err.Error())
	var e *apperrors.AppError
	if errors.As(err, &e) {
		fmt.Printf("%s", e.StackTrace())
	}

	// Output:
	// example
}

func ExampleWrap() {
	subErr := apperrors.New("example")
	err := apperrors.Wrap(subErr)

	fmt.Printf("%s\n", err.Error())
	var e *apperrors.AppError
	if errors.As(err, &e) {
		fmt.Printf("%s", e.StackTrace())
	}

	// Output:
    // example

}

func ExampleWrap_second() {
	originalErr := fmt.Errorf("original")
	wrappedErr := apperrors.Wrap(originalErr)

	err := fmt.Errorf("test2: %w", wrappedErr)

	fmt.Printf("%s\n", err.Error())
	var e *apperrors.AppError
	if errors.As(err, &e) {
		fmt.Printf("%s", e.StackTrace())
	}

	// Output:
	// test2: original
}

func ExampleWrap_third() {
	first := apperrors.Wrap(fmt.Errorf("first"))
	wrapped := apperrors.Wrap(first)

	second := apperrors.Wrap(fmt.Errorf("second: %w", wrapped))
	third := apperrors.Wrap(fmt.Errorf("third: %w", second))
	err := apperrors.Wrap(third)

	fmt.Printf("%s\n", err.Error())
	var e *apperrors.AppError
	if errors.As(err, &e) {
		fmt.Printf("%s", e.StackTrace())
	}

	// Output:
	// third: second: first
}

```

## Testing

```bash
Running tool: /usr/local/go/bin/go test -timeout 30s -run ^TestWrap$ go-errors

=== RUN   TestWrap
--- PASS: TestWrap (0.00s)
PASS
ok      go-errors       0.706s


> Test run finished at 6/9/2023, 5:17:36 PM <

Running tool: /usr/local/go/bin/go test -timeout 30s -run ^TestNew$ go-errors

=== RUN   TestNew
--- PASS: TestNew (0.00s)
PASS
ok      go-errors       0.271s


> Test run finished at 6/9/2023, 5:17:52 PM <
```

## License

MIT License
