# Command-Line Interface

Rudaux was designed to be used from the command line or from within Python. The command line interface provides quick access rudaux's most commonly needed functionality.

The rudaux CLI works in a 2-step process:

1. A [command parser](https://github.com/samhinshaw/rudaux/blob/master/bin/rudaux) parses rudaux's CLI commands and executes the corresponding functions.
2. A [command module](https://github.com/samhinshaw/rudaux/blob/master/rudaux/commands.py) that contains function definitions to be called by the command parser.

## Initialize Course

#### Command:

```sh
rudaux init
```

#### Help Text:

```text
usage: rudaux init [-h] [--dir DIRECTORY] [--auto] [--overwrite]

optional arguments:
  -h, --help       show this help message and exit
  --dir DIRECTORY  The directory containing configuration files.
  --auto, -a       Tell rudaux this is not an interactive shell, do not
                   prompt!
  --overwrite, -o  Suppress overwrite warnings and nuke any existing
                   directories with abandon!
```

## Grade Assignment

<strong>Note:</strong> The functionality to upload generated feedback is the only piece of rudaux that has yet to be implemented. Currently, rudaux only uploads the student's grade to Canvas.

#### Command:

```sh
rudaux grade 'assignment_name'
```

#### Help Text:

```text
usage: rudaux grade [-h] [--dir DIRECTORY] [--auto] [--manual] name

positional arguments:
  name             Name of the assignment to grade.

optional arguments:
  -h, --help       show this help message and exit
  --dir DIRECTORY  The directory containing configuration files.
  --auto, -a       Tell rudaux this is not an interactive shell, do not
                   prompt!
  --manual, -m     Manual grading is necessary.
```

## Submit Assignment

<strong>Note:</strong> The functionality to upload generated feedback is the only piece of rudaux that has yet to be implemented. Currently, rudaux only uploads the student's grade to Canvas.

#### Command:

```sh
rudaux submit 'assignment_name'
```

#### Help Text:

```text
usage: rudaux submit [-h] [--no-feedback] [--dir DIRECTORY] name

positional arguments:
  name               Name of the assignment to grade.

optional arguments:
  -h, --help         show this help message and exit
  --no-feedback, -n  Skip feedback generation.
  --dir DIRECTORY    The directory containing configuration files.
```
