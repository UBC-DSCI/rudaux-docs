# Configuring Rudaux

## Assumptions

Rudaux makes some important assumptions. In the future we hope to abstract away these assumptions. For reproducible provisioning of a compatible course infrastructure, see the [DSCI 100 course infrastructure repository](https://github.com/UBC-DSCI/dsci-100-infra).

<ul>
  <li>
    You have a two-repository, instructors/students model for your course:
    <ul>
      <li>The instructors repository contains everything that nbgrader needs to operate.</li>
      <li>The students repository just contains the release versions of assignments.</li>
    </ul>
  </li>
  <li>
    You wish rudaux to pull, commit, and push to git on your behalf.
    <a href='https://github.com/ubc-dsci/rudaux/issues/4'>This may be made optional</a>.
  </li>
  <li>You have a Canvas Access Token in an environment variable accessible to rudaux.</li>
  <li>Because autograding is containerized, it is assumed your user will be able to execute <code>docker run</code> commands. </li>
  <li><em>If you schedule autograding</em>: The machine you are executing rudaux commands on will be the same machine that autograding is executed on.</li>
  <li><em>If you wish to perform ZFS snapshotting</em>: Rudaux does not perform ZFS snapshotting, but will look for a snapshot with the name of the assignment in question. <a href='https://github.com/ubc-dsci/rudaux/issues/2'>Scheduling ZFS snapshots may be implemented</a>.</li>
  <li>You have SSH keys or deploy keys set up for your repositories on the server you run rudaux. If you use HTTPS rather than SSH for git remotes, you will be prompted for your password. However, this will impair autograding.</li>
</ul>

## Configuration Options

A configuration file should be included in the same directory where nbgrader operates. When creating your configuration file, you have two options:

1. Save the options in a file named **rudaux_config.py**. This will keep your options for each program separate.
2. Save the options in your **nbgrader_config.py** file. This will allow you to only maintain one config file.

### Canvas

<dl>
  <dt><code>c.Canvas.course_url</code> - <em>str</em></dt>
  <dd>
    The URL of your Canvas distribution.
    <p class='extra-dl-info'>
      Example: <code>'https://canvas.institution.edu'</code>
    </p>
  </dd>
  <dt><code>c.Canvas.course_id</code> - <em>int</em></dt>
  <dd>
    The Canvas ID of your course. If you open your course in canvas, this will appear as: <code>https://canvas.institution.edu/courses/&lt;course_id&gt;/assignments</code>
  </dd>
  <dt><code>c.Canvas.token_name</code> (<em>str</em>)</dt>
  <dd>
    The name of the environment variable holding your <a href='https://canvas.instructure.com/doc/api/file.oauth.html#manual-token-generation'>Canvas Access Token</a>.
    <p class='extra-dl-info'>
      Default: <code>'CANVAS_TOKEN'</code>
    </p>
  </dd>
  <dt><code>c.Canvas.external_tool_name</code> - <em>str</em></dt>
  <dd>
    The name of your external tool in Canvas which represents your JupyterHub server. For more information read the
    <a href='https://github.com/jupyterhub/ltiauthenticator#canvas'>ltiauthenticator documentation</a>, which contains information on how to set up an external tool in Canvas.
    <p class='extra-dl-info'>
      Default: <code>'Jupyter'</code>
    </p>
  </dt>
  <dt><code>c.Canvas.external_tool_level</code> - <em>str</em></dt>
  <dd>
    The
    <a href='https://canvas.instructure.com/doc/api/file.tools_intro.html'>level</a>
    at which your external tool was created in Canvas.
    <p class='extra-dl-info'>
      Acceptable values: <code>['course', 'account', 'group']</code>
      <br>
      Default: <code>'course'</code>
    </p>
  </dd>
</dl>

_Note_: `external_tool_name` and `external_tool_level` are used to locate the ID of your external tool in Canvas to attach it to your assignment links. This links your LTI Consumer Key and LTI Consumer Secret to each launch request, authenticating your users.

### GitHub

<dl>
  <dt><code>c.GitHub.ins_repo_url</code> - <em>str</em></dt>
  <dd>
    The location of your instructors repository where you will keep your solutions, graded assignments, and gradebook.db.
    <p class='extra-dl-info'>
      Rudaux will detect the type of URL provided (HTTPS vs SSH).
    </p>
  </dd>
  <dt><code>c.GitHub.stu_repo_url</code> - <em>str</em></dt>
  <dd>
    The location of your public student repository this will contain the release versions of your assignments.
    <p class='extra-dl-info'>
      Rudaux will detect the type of URL provided (HTTPS vs SSH).
    </p>
  </dd>
  <dt>
    <code>c.GitHub.assignment_release_path</code> - <em>str</em>
  </dt>
  <dd>
    The subpath of the student repository where assignments will be deposited.
    <p class='extra-dl-info'>
      Default: <code>'materials'</code>
    </p>
  </dd>
</dl>

### JupyterHub

<dl>
  <dt><code>c.JupyterHub.hub_url</code> - <em>str</em></dt>
  <dd>The URL of your JupyterHub server.</dd>
  <dt><code>c.JupyterHub.base_url</code> - <em>str</em></dt>
  <dd>
    A JupyterHub prefix, if one is specified. In your jupyterhub_config.py, this would be named "c.JupyterHub.base_url"
    <p class='extra-dl-info'>
      Default: <code>""</code> (<em>an empty string</em>)
      <br>
      Example: <code>'/jupyter'</code>
    </p>
  </dd>
  <dt><code>c.JupyterHub.storage_path</code> - <em>str</em></dt>
  <dd>
    The location of your students' persistent storage.
    <p class='extra-dl-info'>
      Example: <code>'/tank/home/dsci100'</code>
    </p>
  </dd>
  <dt><code>c.JupyterHub.zfs</code> - <em>bool</em></dt>
  <dd>
    Are ZFS snapshots available? If so, we will utilize snapshotting to ensure that we are using a precise copy of the students' notebooks at the time the assignment closes. If not, we will simply read from the student directories.
    <p class='extra-dl-info'>
      Default: <code>False</code>
    </p>
  </dd>
  <dt><code>c.JupyterHub.zfs_regex</code> - <em>str</em></dt>
  <dd>
    A regular expression that matches your snapshot timestamp. Use None if the snapshot name is a timestamp.
    <p class='extra-dl-info'>
      Default: <code>r'\d{4}-\d{2}-\d{2}-\d{4}'</code>
    </p>
  </dd>
  <dt><code>c.JupyterHub.zfs_datetime_pattern</code> - <em>str</em></dt>
  <dd>
    The pattern of your timestamp in token format, for pendulum parsing: https://pendulum.eustace.io/docs/#tokens
    <p class='extra-dl-info'>
      Default: <code>'YYYY-MM-DD-HHmm'</code>
    </p>
  </dd>
</dl>

### Course

<dl>
  <dt><code>c.Course.tmp_dir</code> - <em>str</em></dt>
  <dd>
    The temporary directory that rudaux will clone your students' repository into for assignment release.
    <p class="extra-dl-info">
      Default: <code>'~/tmp'</code>
    </p>
  </dd>
  <dt><code>c.Course.timezone</code> - <em>str</em></dt>
  <dd>
    The timezone which your due dates will be set and parsed in. 
    <p class="extra-dl-info">
      Example: <code>'US/Pacific'</code>
    </p>
  </dd>
  <dt><code>c.Course.grading_image</code></dt>
  <dd>
    The docker image you will use to run nbgrader autograding. This should essentially be a copy of your students' docker image, plus nbgrader. 
    <p class="extra-dl-info">
      Example: <code>'ubcdsci/r-dsci-grading'</code>
      <br>
      Example Dockerfiles from DSCI 100:
      <ul>
        <li>
          <a href='https://github.com/UBC-DSCI/docker-stacks/blob/master/r-dsci-100/Dockerfile'>Student Container</a>
        </li>
        <li>
          <a href='https://github.com/UBC-DSCI/docker-stacks/blob/master/r-dsci-grading/Dockerfile'>Marking Container</a>
        </li>
      </ul>
    </p>
  </dd>
  <dt><code>c.Course.assignments</code></dt>
  <dd>
    <p>A list of your assignments with the following fields:</p>
    <dl>
      <dt><code>name</code> - <em>str</em></dt>
      <dd>
        Required. Unique. The name of the assignment. Must conform to nbgrader naming conventions (i.e. must be the name of the folder containing the jupyter notebook).
        <p class="extra-dl-info">
          Example: <code>'homework_1'</code>
        </p>
      </dd>
      <dt><code>duedate</code> - <em>str</em></dt>
      <dd>
        Required. The date the assignment is due.
        <p class="extra-dl-info">
          Example: <code>'2019-03-14'</code>
        </p>
      </dd>
      <dt><code>duetime</code> - <em>str</em></dt>
      <dd>
        Optional. The time the assignment is due.
        <p class="extra-dl-info">
          Default: <code>'23:59:59'</code>
        </p>
      </dd>
      <dt><code>points</code> - <em>int</em></dt>
      <dd>
        Optional. The maximum possible points for this assignment.
        <p class="extra-dl-info">
          Default: <code>1</code>
        </p>
      </dd>
      <dt><code>manual</code> - <em>bool</em></dt>
      <dd>
        Optional. Does the assignment require manual input (i.e. TA feedback)? If so, feedback reports will not be generated and grades will not be submitted to Canvas automatically.
        <p class="extra-dl-info">
          Default: <code>False</code>
        </p>
      </dd>
    </dl>
    <p><em>Note</em>: Rudaux parses datetimes with the pendulum library&mdash;acceptable date strings are listed in the <a href='https://pendulum.eustace.io/docs/#parsing'>pendulum docs</a>. For example, if <code>duedate</code> were <code>'2019-03-14'</code> and <code>duetime</code> were <code>'22:00:00'</code>, they would be joined and parsed as <code>'2019-03-14T22:00:00'</code>.</p>
  </dd>
</dl>

[Sample rudaux_config.py](../examples/objects#sample-rudaux_configpy)
