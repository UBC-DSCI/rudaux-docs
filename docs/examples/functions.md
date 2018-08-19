# Examples of Function Calls

## Classes

```python
from rudaux import Course, Assignment
```

### Course

```python
dsci100 = Course('~/dsci100-instructors')
```

### Assignment

```python
class DataScienceAssignment(Assignment):
  course = dsci100

homework_1 = DataScienceAssignment('homework_1')
```

Similarly:

```python
homework_1 = DataScienceAssignment('homework_1', course=dsci100)
```

## High-level Interface (CLI)

### Initialize

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
