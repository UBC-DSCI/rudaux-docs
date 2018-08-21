# Rudaux Classes

## Course

```python
from rudaux import Course
course = Course()
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

Assignment object for maniuplating individual assignments.

### Instantiation

```py
Assignment(
  name,
  duedate=None,
  duetime='23:59:59',
  points=1,
  manual=False,
  course=None
)
```

<dl>
  <dt><strong>Parameters</strong></dt>
  <dd>
    <dl>
      <dt><code>name</code> - <em>str</em></dt>
      <dd>
        <p>The assignment's name. This should be compatible with nbgrader's file naming convention.</p>
        <p><em>Required.</em> Example: <code>'homework_1'</code></p>
      </dd>
      <dt><code>duedate</code> - <em>str</em></dt>
      <dd>
        <p>The assignment's due date.</p>
        <p><em>Required.</em> Example: <code>'2019-03-14'</code></p>
      </dd>
      <dt><code>duetime</code> - <em>str</em></dt>
      <dd>
        <p>The assignment's due time.</p>
        <p><em>Default</em>: <code>'23:59:59'</code></p>
      </dd>
      <dt><code>points</code> - <em>int</em></dt>
      <dd>
        <p>The number of points the assignment is worth.</p>
        <p><em>Default</em>: <code>1</code></p>
      </dd>
      <dt><code>manual</code> - <em>bool</em></dt>
      <dd>
        <p>Is manual grading required?</p>
        <p><em>Default</em>: <code>False</code></p>
      </dd>
      <dt><code>course</code> - <em>Course</em></dt>
      <dd>
        <p>The course the assignment belongs to.</p>
        <p><em>Optional.</em> Recommended usage is to subclass Assignment with the course as a class variable.</p>
      </dd>
    </dl>
  </dd>
  <dt><strong>Returns</strong></dt>
  <dd>
    <dl>
      <dt><code>Assignment</code> - <em>Assignment</em></dt>
      <dd>An assignment object for performing different operations on a given assignment.</dd>
    </dl>
  </dd>
</dl>

### Methods

<dl>
  <dt>
    <code>Assignment.collect()</code>
    &nbsp;
    <span class="badge badge-primary">Commits</span>
    &nbsp;
    <span class="badge badge-primary">Pushes</span>
  </dt>
  <dd>

    <p>Collect an assignment.</p>
    <p>Copy your students' notebooks from the fileserver into the instructors repo <code>submitted/</code> directory.</p>
    <p>This also creates a submission in the gradebook on behalf of each student, which is necessary for nbgrader to record grades in the gradebook.</p>
    <dl>
      <dt><strong>Returns</strong></dt>
      <dd>self</dd>
    </dl>

  </dd>
  <dt><code>Assignment.update_or_create_canvas_assignment()</code></dt>
  <dd>
    Search for an assignment in Canvas by assignment name. If an assignment is found, update it. If not, create it.
  </dd>
  <dt><code>Assignment.schedule_grading()</code></dt>
  <dd>
    <p>Schedule grading of an assignment by adding cron jobs to initialize autograding. The job will be scheduled to run at the assignment's due datetime.</p>
    <ul>
      <li>If no auto-grading job for that assignment exists in cron, create a job.</li>
      <li>If an auto-grading job for that assignment is already scheduled in cron, update the job.</li>
    </ul>

    The job takes the following format:
    <ol>
      <li>Initialize SSH agent</li>
      <li>Add instructors & students SSH-keys to SSH agent</li>
      <li>Run `rudaux grade` with output redirected to a log file in the instructors' repository</li>
    </ol>
    <p>Returns: A status reporting object with the keys 'close_time' and 'action'.</p>

  </dd>
</dl>

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
