# Project Structure

The following content shows a general structure of a web app, it can be changed based on the different conventions of web frameworks.

```
- templates (views)          # Template files
- public (static)            # Public assets
	- css
	- fonts
	- img
	- js
- routes
- models
- pkg
	- setting                 # Global settings of app
- cmd                         # Files of commands
- conf                        # Default configuration
	- locale                  # I18n locale files
- custom                      # Custom configuraion
- data                        # App generated data
- log                         # App generated log
```

## Command Line App

When writing a CLI app, each command should be in a separate file under `/cmd` directory:

```
/cmd
	dump.go
	fix.go
	serve.go
	update.go
	web.go
```
