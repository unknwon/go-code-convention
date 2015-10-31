# Commentary

- All exported objects must be well-commented; internal objects can be commented as needed.
- If the object is countable and does not know the number of it, use singular form and present tense; otherwise, use plural form.
- Comment of package, function, method and type must be a complete sentence.
- First letters of sentences should be upper case, but lower case for phrases.
- Maximum length of a comment line should be 80 characters.

### Package

- Only one file is needed for package level comment.
- For `main` packages, a line of introduction is enough, and should start with project name:

	```Go
	// Gogs (Go Git Service) is a painless self-hosted Git Service.
	package main
	```

- For sub-packages of a complicated project, no package level comment is required unless it's a functional module.
- For simple non-`main` packages, a line of introduction is enough as well.
- For more complicated functional non-`main` packages, commonly should have some basic usage examples and must start with `Package <name>`:

	```Go
	/*
	Package regexp implements a simple library for regular expressions.

	The syntax of the regular expressions accepted is:

	    regexp:
	        concatenation { '|' concatenation }
	    concatenation:
	        { closure }
	    closure:
	        term [ '*' | '+' | '?' ]
	    term:
	        '^'
	        '$'
	        '.'
	        character
	        '[' [ '^' ] character-ranges ']'
	        '(' regexp ')'
	*/
	package regexp
	```

- For very complicated functional packages, you should create a [`doc.go`](https://github.com/robfig/cron/blob/master/doc.go) file for it.

### Type, Interface and Other Types

- Type is often described in singular form:

	```Go
	// Request represents a request to run a command.
	type Request struct { ...
	```

- Interface should be described as follows：

	```Go
	// FileInfo is the interface that describes a file and is returned by Stat and Lstat.
	type FileInfo interface { ...
	```


### Functions and Methods

- Comments of functions and methods must start with its name:

	```Go
	// Post returns *BeegoHttpRequest with POST method.
	```

- If one sentence is not enough, go to next line:

	```Go
	// Copy copies file from source to target path.
	// It returns false and error when error occurs in underlying function calls.
	```

- If the main purpose of a function or method is returning a `bool` value, its comment should start with `<name> returns true if`:

	```Go
	// HasPrefix returns true if name has any string in given slice as prefix.
	func HasPrefix(name string, prefixes []string) bool { ...
	```

### Other Notes

- When something is waiting to be done, use comment starts with `TODO:` to remind maintainers.
- When a known problem/issue/bug needs to be fixed/improved, use comment starts with `FIXME:` to remind maintainers.
- When something is too magic and needs to be explained, use comment starts with `NOTE:`:

	```Go
	// NOTE: os.Chmod and os.Chtimes don't recognize symbolic link,
	// which will lead "no such file or directory" error.
	return os.Symlink(target, dest)
	```
