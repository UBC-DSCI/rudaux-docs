# Configuring Rudaux

A configuration file should be included in the same directory where nbgrader operates. You have two options:

1. Save the options in a file named **rudaux_config.py**. This will keep your options for each program separate.
2. Save the options in your **nbgrader_config.py** file. This will allow you to only maintain one config file.

**rudaux_config.py**:

```python
#=======================================#
#                 Canvas                #
#=======================================#

# The ID of your course in Canvas. If you open your course in canvas, this will
# appear as:
# https://canvas.institution.edu/courses/<course_id>/assignments
c.Canvas.course_id = 5394

# The URL of your Canvas installation
c.Canvas.canvas_url = 'https://canvas.ubc.ca'

# The name of the environment variable storing you Canvas API Token
# https://canvas.instructure.com/doc/api/file.oauth.html#manual-token-generation
# Defaults to 'CANVAS_TOKEN'
c.Canvas.token_name = 'CANVAS_TOKEN'

# The name of the external tool in Canvas which represents your JupyterHub server
# Defaults to "Jupyter"
c.Canvas.external_tool_name = 'Jupyter'

# The level at which your external tool was created in Canvas
# Can be 'course', 'account' or 'group'
# Defaults to "course"
c.Canvas.external_tool_level = 'course'

#=======================================#
#                 GitHub                #
#=======================================#

# The location of your instructors repository where you will keep your
# solutions, graded assignments, and gradebook.db
# c.GitHub.ins_repo_url = 'https://github.ubc.ca/hinshaws/dsci_100_instructors'
c.GitHub.ins_repo_url = 'git@github.ubc.ca:hinshaws/DSCI_100_instructors.git'

# The location of your public student repository
# this will contain the release versions of your assignments
# c.GitHub.stu_repo_url = 'https://github.com/samhinshaw/dsci-100'
c.GitHub.stu_repo_url = 'git@github.com:samhinshaw/dsci-100.git'

# the subpath of the student repository where assignments will be deposited.
# defaults to 'materials' if none is specified
c.GitHub.assignment_release_path = 'materials'

# default if not specified is 'GITHUB_PAT'
# c.GitHub.github_token_name = 'GHE_PAT'

# You can specify multiple keys, just paste the url you specified above into the URL fields
# c.GitHub.token_names = [
#     {
#         "url": "github.com",
#         "token_name": "GITHUB_PAT"
#     },
#     {
#         "domain": "github.ubc.ca",
#         "token_name": "GHE_PAT"
#     },
# ]

#=======================================#
#               JupyterHub              #
#=======================================#

# The URL that your JupyterHub is located at
c.JupyterHub.hub_url = 'https://hub-prod-dsci.stat.ubc.ca'
# Any hub prefix that you have specified. In your jupyterhub_config.py, this
# would be named "c.JupyterHub.base_url"
c.JupyterHub.base_url = '/jupyter'

# The location of your students' persistent storage
# c.JupyterHub.storage_path = '/tank/home/dsci100'
c.JupyterHub.storage_path = '/nfs/hub-prod-dsc/' # temporarily changed

# Is your persistent storage solution ZFS-based? (default: False)
# If so, we will utilize snapshotting to ensure that we are using a precise copy
# of the students' notebooks at the time the assignment closes. If not, we will
# simply read from the student directories. In the future, it would be ideal to
# fall back to copying the assignments to a temporary directory.
c.JupyterHub.zfs = True

#=======================================#
#                 Course                #
#=======================================#

# Defaults to $HOME/tmp
# c.Course.tmp_dir = '~/tmp'

# Timezone for which your due dates will be set and parsed
c.Course.timezone = 'US/Pacific'

# The docker image you will use to run nbgrader autograding.
# This should essentially be a copy of your students' docker image, plus nbgrader.
# We wrote our Dockerfile to inherit from our student's docker image.
c.Course.grading_image = 'samhinshaw/r-dsci-grading'
# c.Course.grading_image = 'ubcdsci/r-dsci-grading'

  #===============#
  #  Assignments  #
  #===============#

# These must match the name of the folder containing the jupyter notebook in
# your 'source' directory. If no points specified, defaults to 0 points.
# NOTE: If no time specified for due date, defaults to 23:59:59.
# We parse dates with the pendulum library, so you can see what date inputs are
# acceptable in their docs: https://pendulum.eustace.io/docs/#rfc-3339
c.Course.assignments = [
    {
        "name": "week_1",
        "duedate": "2019-08-14",
        "duetime": "23:59:59",
        "points": 2,
        "manual": True
    },
    {
        "name": "homework_1",
        "duedate": "2019-08-15",
        "duetime": "11:00:00",
        "points": 5
    },
    {
        "name": "ps1",
        "duedate": "2019-08-15",
        "duetime": "15:31:00",
        "points": 4
    },
    {
        "name": "week_2",
        "duedate": "2019-08-16",
        "duetime": "23:59:59",
    },
    {
        "name": "homework_2",
        "duedate": "2019-08-16",
        "duetime": "06:00:00",
        "points": 6
    },
    {
        "name": "week_6",
        "duedate": "2019-08-18",
        "duetime": "23:59:59",
        "points": 2,
        "manual": True
    },
    {
        "name": "week_7",
        "duedate": "2019-08-19",
        "duetime": "23:59:59",
        "points": 3
    },
]
```
