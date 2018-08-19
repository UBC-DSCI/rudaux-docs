# Rudaux Modules

```python
from rudaux import Course, Assignment
```

## Course

```python
dsci100 = Course()
```

```python
dsci100.assign()
dsci100.create_canvas_assignments()
dsci100.get_external_tool_id()
dsci100.get_students_from_canvas()
dsci100.schedule_grading()
dsci100.sync_nbgrader()
```

## Assignment

```python
class DataScienceAssignment(Assignment):
  course = dsci100

homework_1 = DataScienceAssignment('homework_1')
```

Similarly:

```python
homework_1 = DataScienceAssignment('homework_1', course=dsci100)
```

_class_ Assignment: Manipulate an assignment.

```python
Assignment(
  name: str,
  duedate=None,
  duetime=None,
  points=1,
  manual=False,
  course=None
):
```

```python
homework_1.course
```

```python
homework_1.autograde()
```

```python
homework_1.collect()
```

```python
homework_1.feedback()
```

```python
homework_1.grade()
```

```python
homework_1.schedule_grading()
```

```python
homework_1.submit()
```

```python
homework_1.update_or_create_canvas_assignment()
```

## Command Line Interface

### Initialize/Update

```sh
rudaux init
```

### Grade

```sh
rudaux grade 'homework_1'
```

```python
course                                   \
  .get_external_tool_id()                \
  .get_students_from_canvas()            \
  .sync_nbgrader()                       \
  .assign(overwrite=args.overwrite)      \
  .create_canvas_assignments()           \
  .schedule_grading()
```

### Submit

```sh
rudaux submit 'homework_1'
```
