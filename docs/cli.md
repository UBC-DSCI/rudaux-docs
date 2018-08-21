# Command-Line Interface

Rudaux was designed to be used from the command line or from within Python. The command line functions are aliases for groups of commonly needed functions.

#### Command:

```sh
rudaux
```

#### Help Text:

```text
usage: rudaux [-h] {init,grade,submit} ...

Manage your JupyterHub/Canvas/nbgrader course with Rudaux.

optional arguments:
  -h, --help           show this help message and exit

Subcommands:
  {init,grade,submit}  Commands rudaux can process.
    init               Initialize or update the course.
    grade              Grade an assignment.
    submit             Generate feedback for and submit an assignment.
```

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

#### Python Code Invoked:

```py
course = Course(course_dir=args.directory, auto=args.auto)

course                                   \
  .get_external_tool_id()                \
  .get_students_from_canvas()            \
  .sync_nbgrader()                       \
  .assign(overwrite=args.overwrite)      \
  .create_canvas_assignments()           \
  .schedule_grading()
```

## Grade Assignment

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

#### Python Code Invoked:

```py
course = Course(course_dir=args.directory, auto=args.auto)

course = course               \
  .get_external_tool_id()     \
  .get_students_from_canvas() \
  .sync_nbgrader()

# find assignment in config assignment list
assignment = list(
  filter(lambda assn: assn.name == args.assignment_name, course.assignments)
)

if len(assignment) <= 0:
  sys.exit(f"No assignment named \"{args.assignment_name}\" found")
else:
  # Take the first result.
  assignment = assignment[0]
  # But notify if more than one was found
  # Though this should never happen--assignment names must be unique.
  if len(assignment) > 1:
    print(
      f"Multiple assignments named \"{args.assignment_name}\" were found. Grading the first one!"
    )

# collect and grade the assignment
assignment = assignment \
  .collect()            \
  .grade()

# and if no manual feedback is required, generate feedback reports
# and submit grades
if not args.manual:
  assignment    \
    .feedback() \
    .submit()
```

## Submit Assignment

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

#### Python Code Invoked:

```py
course = Course(course_dir=args.directory)

course = course               \
  .get_external_tool_id()     \
  .get_students_from_canvas() \
  .sync_nbgrader()

# find assignment in config assignment list
assignment = list(
  filter(lambda assn: assn.name == args.assignment_name, course.assignments)
)

if len(assignment) <= 0:
  sys.exit(f"No assignment named \"{args.assignment_name}\" found")
else:
  # Take the first result
  assignment = assignment[0]
  # If we run into this situation, there's probably a lot of other weird things going wrong already.
  if len(assignment) > 1:
    print(
      f"Multiple assignments named \"{args.assignment_name}\" were found. Grading the first one!"
    )

  assignment    \
    .feedback() \
    .submit()
```
