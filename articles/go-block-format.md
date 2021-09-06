# Golang Code

# Input Validation Block Formatting

Input validation is a must to reduce panic errors when your application runs or serves based on the inputs, whether it's a command line argument or a JSON document body.
In most cases, a validation function needs to return two values: the boolean value and the error returned. The boolean value stores the information
whether the argument is valid or not, and the error value returns a non-nil value when it encounters one.

For example:

```go
func main () {

	// Check legal age
	age := 17

	legal, err := IsLegalAge(age)
	if err != nil {
		log.Fatalf("error encountered: %w", err)
	}

	if !legal {
		log.Fatalf("age %d is not yet legal", age)
	}

	log.Println("You can enter the premises!")
}
```

With this length of the code, there isn't an apparent issue yet. However, when you need to validate a number of inputs, there will be a lot of clutter.

For example:

```go
func main () {

	var (
		err error
		valid bool
	)

	// Check legal age
	age := 17

	valid, err = IsLegalAge(age)
	if err != nil {
		log.Fatalf("error encountered: %w", err)
	}

	if !valid {
		log.Fatalf("age %d is not yet legal", age)
	}

	// Check if card is accepted
	card := `Visa`

	valid, err = IsCardAccepted(card)
	if err != nil {
		log.Fatalf("error encountered: %w", err)
	}

	if !valid {
		log.Fatalf("A %s card is not honored here", card)
	}

	// Check if the customer is carrying banned things
	things := []string{`Gun`,`Cellphone`,`Booze`}

	valid, err = AreThingsAllowed(things)
	if err != nil {
		log.Fatalf("error encountered: %w", err)
	}

	if !valid {
		log.Fatalf("You cannot carry %s inside", strings.Join(things, `,`))
	}

	log.Println("You can enter the premises!")
}
```

With this code, you can easily be lost to find where the err value belongs to. If you are reusing the boolean part of the return value, you'll find out that these things need to be in context.

When I encounter a situation like this, I use the if-semicolon syntax and put a `true` value after it to group the code and allow the values returned by a validation function to pass through and further check the return values:

```go
func main () {

	var (
		err error
		valid bool
	)

	// Check legal age
	age := 17

	if valid, err = IsLegalAge(age); true {

		if err != nil {
			log.Fatalf("error encountered: %w", err)
		}

		if !valid {
			log.Fatalf("age %d is not yet legal", age)
		}
	}

	// Check if card is accepted
	card := `Visa`

	if valid, err = IsCardAccepted(card); true {
		if err != nil {
			log.Fatalf("error encountered: %w", err)
		}

		if !valid {
			log.Fatalf("A %s card is not honored here", card)
		}
	}

	// Check if the customer is carrying banned things
	things := []string{`Gun`,`Cellphone`,`Booze`}

	if valid, err = AreThingsAllowed(things); true {
		if err != nil {
			log.Fatalf("error encountered: %w", err)
		}

		if !valid {
			log.Fatalf("You cannot carry %s inside", strings.Join(things, `,`))
		}
	}

	log.Println("You can enter the premises!")
}
```

This format allows you to read the context of validation much clearer.

*Elizalde Baguinon,
September 6, 2021*

E-mail: mizmerize@yahoo.com