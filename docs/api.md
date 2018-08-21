# Rudaux API Documentation

API documentation for rudaux's primary classes: `Course` and Assignment`. For some examples of how to use these functions, please see [API Usage](../usage).

## Course

Course object for manipulating an entire Canvas/JupyterHub/nbgrader course

```python
from rudaux import Course
```

### Instantiation

```py
Course(
  course_dir=None,
  auto=False
)
```

<dl>
  <dt><strong>Parameters</strong></dt>
  <dd>
    <dl>
      <dt><code>course_dir</code> - <em>str</em></dt>
      <dd>
        <p><span class="definition">The course directory containing your course configuration files.</span> (<em>rudaux_config.py</em> and/or <em>nbgrader_config.py</em>) This should also be your nbgrader directory. Currently, it is assumed that this is your private instructors' repository. If none is provided, defaults to the current working directory.</p>
        <p><em>Default</em>: <code>os.getcwd()</code></p>
      </dd>
      <dt><code>auto</code> - <em>bool</em></dt>
      <dd>
        <p class='definition'>Suppress all prompts, automatically answering yes.</p>
        <p><em>Default</em>: <code>False</code></p>
      </dd>
    </dl>
  </dd>
  <dt><strong>Returns</strong> - <em>Course</em></dt>
  <dd>
    <p>A Course object for manipulating an entire Canvas/JupyterHub/nbgrader course.</p>
  </dd>  
</dl>

### Methods

<dl>
  <dt>
    <code>.get_external_tool_id()</code> &rarr; <em>Course</em>
  </dt>
  <dd>
    <span class="definition">Find the ID of the external tool created in Canvas that represents your JupyterHub server.</span>
    <br>
    This is necessary to link your LTI launch keys to assignment links created in Canvas.
  </dd>
  <dt>
    <code>.get_students_from_canvas()</code> &rarr; <em>Course</em>
  </dt>
  <dd>
    <p class='definition'>Get the course student list from Canvas.</p>
  </dd>
  <dt>
    <code>.sync_nbgrader()</code> &rarr; <em>Course</em>
  </dt>
  <dd>
    <p class='definition'>Sync student and assignment lists between nbgrader and Canvas.</p>
    <ul>
      <li>Add students into nbgrader gradebook that are present in Canvas.</li>
      <li>Remove students from nbgrader gradebook that are not present in Canvas.</li>
      <li>Add assignments to gradebook which are present in config, but not in gradebook.</li>
      <li>Remove assignments from gradebook which are no longer present in config.</li>
    </ul>
  </dd>
  <dt>
    <code>.assign(assignments, overwrite)</code> &rarr; <em>Course</em>
    &nbsp;
    <span class="badge badge-primary">Commits</span>
    &nbsp;
    <span class="badge badge-primary">Pushes</span>
  </dt>
  <dd>
    <p class='definition'>Assign assignments for a course.</p>
    <dl>
      <dt><strong>Parameters</strong></dt>
      <dd>
        <dl>
          <dt><code>assignments</code> - <em>Union[List[str], str]</em></dt>
          <dd> 
            <span class="definition">The name or names of the assignments you wish to assign.</span>
            <br>
            Defaults to all assignments.
          </dd>
          <dt><code>overwrite</code> - <em>bool</em></dt>
          <dd> 
            <p class="definition">Bypass overwrite prompts and nuke preexisting directories.</p>
            <p><em>Default</em>: <code>False</code></p>
          </dd>
        </dl>
      </dd>
    </dl>
  </dd>
  <dt>
    <code>.create_canvas_assignments()</code> &rarr; <em>Course</em>
  </dt>
  <dd>
    <p class='definition'>Create assignments in Canvas.</p>
  </dd>
  <dt>
    <code>.schedule_grading()</code> &rarr; <em>Course</em>
  </dt>
  <dd>
    <p class='definition'>Schedule auto-grading cron jobs for all assignments.</p>
  </dd>
</dl>

## Assignment

Assignment object for manipulating individual assignments.

```py
from rudaux import Assignment
```

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
        <p>
          <span class="definition">The assignment's name.</span>
          <br>
          This should be compatible with nbgrader's file naming convention.
        </p>
        <p><em>Required.</em> Example: <code>'homework_1'</code></p>
      </dd>
      <dt><code>duedate</code> - <em>str</em></dt>
      <dd>
        <p class='definition'>The assignment's due date.</p>
        <p><em>Required.</em> Example: <code>'2019-03-14'</code></p>
      </dd>
      <dt><code>duetime</code> - <em>str</em></dt>
      <dd>
        <p class='definition'>The assignment's due time.</p>
        <p><em>Default</em>: <code>'23:59:59'</code></p>
      </dd>
      <dt><code>points</code> - <em>int</em></dt>
      <dd>
        <p class='definition'>The number of points the assignment is worth.</p>
        <p><em>Default</em>: <code>1</code></p>
      </dd>
      <dt><code>manual</code> - <em>bool</em></dt>
      <dd>
        <p class='definition'>Is manual grading required?</p>
        <p><em>Default</em>: <code>False</code></p>
      </dd>
      <dt><code>course</code> - <em>Course</em></dt>
      <dd>
        <p class='definition'>The course the assignment belongs to.</p>
        <p><em>Optional.</em> Recommended usage is to subclass Assignment with the course as a class variable.</p>
      </dd>
    </dl>
  </dd>
  <dt><strong>Returns</strong> - <em>Assignment</em></dt>
  <dd>
    <p>An assignment object for performing different operations on a given assignment.</p>
  </dd>
</dl>

### Methods

<dl>
  <dt><code>.update_or_create_canvas_assignment()</code> &rarr; <em>str</em></dt>
  <dd>
    <p>
      <span class="definition">Search for an assignment in Canvas by assignment name.</span> 
      <br>
      If an assignment is found, update it. If not, create it.
    </p>
    <dl>
      <dt><strong>Returns</strong></dt>
      <dd>
        <p>A reporting status, whether the assignment was updated or created.</p>
      </dd>
    </dl>
  </dd>
  <dt><code>.schedule_grading()</code> &rarr; <em>Dict[str, str]</em></dt>
  <dd>
    <p>
      <span class="definition">Schedule grading of an assignment by adding cron jobs to initialize auto-grading.</span> 
      <br>
      The job will be scheduled to run at the assignment's due datetime.
    </p>
    <ul>
      <li>If no auto-grading job for that assignment exists in cron, create a job.</li>
      <li>If an auto-grading job for that assignment is already scheduled in cron, update the job.</li>
    </ul>
    <p>The job takes the following format:</p>
    <ol>
      <li>Initialize SSH agent</li>
      <li>Add instructors & students SSH-keys to SSH agent</li>
      <li>Run `rudaux grade` with output redirected to a log file in the instructors' repository</li>
    </ol>
    <dl>
      <dt><strong>Returns</strong></dt>
      <dd>
        <p>A status reporting dictionary with the keys 'close_time' and 'action'.</p>
      </dd>
    </dl>
  </dd>
  <dt>
    <code>.collect()</code> &rarr; <em>Assignment</em>
    &nbsp;
    <span class="badge badge-primary">Commits</span>
    &nbsp;
    <span class="badge badge-primary">Pushes</span>
  </dt>
  <dd>
    <p class='definition'>Collect student copies an assignment.</p>
    <p>Copy your students' notebooks from the fileserver into the instructors repo <code>submitted/</code> directory.  This also creates a submission in the gradebook on behalf of each student, which is necessary for nbgrader to record grades in the gradebook.</p>
    <p>If the student fileserver is a ZFS filesystem, the closest snapshot that occurred after the due date will be used. This requires <code>c.JupyterHub.zfs_regex</code> and <code>c.JupyterHub.zfs_datetime_pattern</code> to be set in the course configuration.</p>
  </dd>
  <dt>
    <code>.grade()</code> &rarr; <em>Assignment</em>
    &nbsp;
    <span class="badge badge-primary">Commits</span>
    &nbsp;
    <span class="badge badge-primary">Pushes</span>
  </dt>
  <dd>
    <p class='definition'>Auto-grade an assignment within a docker container.</p>
  </dd>
  <dt>
    <code>.feedback()</code> &rarr; <em>Assignment</em>
    &nbsp;
    <span class="badge badge-primary">Commits</span>
    &nbsp;
    <span class="badge badge-primary">Pushes</span>
  </dt>
  <dd>
    <p class='definition'>Generate feedback reports for student assignments.</p>
  </dd>
  <dt>
    <code>.submit()</code> &rarr; <em>Assignment</em>
    &nbsp;
    <span class="badge badge-primary">Commits</span>
    &nbsp;
    <span class="badge badge-primary">Pushes</span>
  </dt>
  <dd>
    <p class='definition'>Upload students' grades to Canvas.</p>
    <p>
      <strong>Note:</strong> The functionality to upload generated feedback is the only piece of rudaux that has yet to be implemented. Currently, <code>.submit()</code> only uploads the student's grade to Canvas.
    </p>
  </dd>
</dl>
