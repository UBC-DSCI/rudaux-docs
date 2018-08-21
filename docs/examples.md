# Examples

Here are examples

## API Usage

How to use rudaux's modules in Python for greater functionality.

```python
from rudaux import Course, Assignment
```

### Course Operations

Instantiate the course.

```python
dsci100 = Course('/home/jovyan/dsci100-instructors')
```

Set up students, assignments, and schedule automatic grading.

```py
dsci100                                  \
  .get_external_tool_id()                \
  .get_students_from_canvas()            \
  .sync_nbgrader()                       \
  .assign(overwrite=args.overwrite)      \
  .create_canvas_assignments()           \
  .schedule_grading()
```

### Assignment Operations

```python
class DataScienceAssignment(Assignment):
  course = dsci100

homework_1 = DataScienceAssignment('homework_1')
```

Alternatively:

```python
homework_1 = Assignment('homework_1', course=dsci100)
```

<hr>

## Example rudaux_config.py

```python
#=======================================#
#                 Canvas                #
#=======================================#

# The URL of your Canvas installation
c.Canvas.canvas_url = 'https://canvas.ubc.ca'

# The ID of your course in Canvas. If you open your course in canvas, this will
# appear as:
# https://canvas.institution.edu/courses/<course_id>/assignments
c.Canvas.course_id = 5394


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

# The following options are not currently supported

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
# simply read from the student directories.
c.JupyterHub.zfs = True

# Optional. A regular expression that matches your snapshot timestamp. Use None if the snapshot name is a timestamp.
c.JupyterHub.zfs_regex = r'\d{4}-\d{2}-\d{2}-\d{4}'

# Optional. The pattern of your timestamp in token format, for pendulum parsing:
# https://pendulum.eustace.io/docs/#tokens
c.JupyterHub.zfs_datetime_pattern = 'YYYY-MM-DD-HHmm'

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

<hr>

## LTI Launch Request

```json
{
  "query_string": {
    "custom_next": "/jupyter/hub/user-redirect/git-pull?repo=https://github.com/binder-examples/requirements&subPath=index.ipynb"
  },
  "form_data": {
    "oauth_consumer_key": "<redacted>",
    "oauth_signature_method": "HMAC-SHA1",
    "oauth_timestamp": "1529516978",
    "oauth_nonce": "<redacted>",
    "oauth_version": "1.0",
    "context_id": "df6178b861774822eec73639c1f5808ebf9b3259",
    "context_label": "DSCI100",
    "context_title": "DSCI 100",
    "custom_canvas_api_domain": "canvas.ubc.ca",
    "custom_canvas_assignment_id": "151504",
    "custom_canvas_assignment_points_possible": "0",
    "custom_canvas_assignment_title": "Example Assignment",
    "custom_canvas_course_id": "5394",
    "custom_canvas_enrollment_state": "active",
    "custom_canvas_user_id": "185091",
    "custom_canvas_user_login_id": "OB2D4G5QJS00",
    "custom_canvas_workflow_state": "claimed",
    "custom_next": "/jupyter/hub/user-redirect/git-pull?repo=https://github.com/binder-examples/requirements&subPath=index.ipynb",
    "ext_ims_lis_basic_outcome_url": "https://canvas.ubc.ca/api/lti/v1/tools/2399/ext_grade_passback",
    "ext_lti_assignment_id": "657aadda-f21d-41a3-9125-2fc67dc1e07e",
    "ext_outcome_data_values_accepted": "url,text",
    "ext_outcome_result_total_score_accepted": "true",
    "ext_outcome_submission_submitted_at_accepted": "true",
    "ext_outcomes_tool_placement_url": "https://canvas.ubc.ca/api/lti/v1/turnitin/outcomes_placement/2399",
    "ext_roles": "urn:lti:instrole:ims/lis/Instructor,urn:lti:instrole:ims/lis/Student,urn:lti:role:ims/lis/Instructor,urn:lti:sysrole:ims/lis/User",
    "launch_presentation_document_target": "iframe",
    "launch_presentation_locale": "en-CA",
    "launch_presentation_return_url": "https://canvas.ubc.ca/courses/5394/external_content/success/external_tool_redirect",
    "lis_outcome_service_url": "https://canvas.ubc.ca/api/lti/v1/tools/2399/grade_passback",
    "lis_person_contact_email_primary": "samuel.hinshaw@gmail.com",
    "lis_person_name_family": "Hinshaw",
    "lis_person_name_full": "Samuel Hinshaw",
    "lis_person_name_given": "Samuel",
    "lis_person_sourcedid": "90088155",
    "lti_message_type": "basic-lti-launch-request",
    "lti_version": "LTI-1p0",
    "oauth_callback": "about:blank",
    "resource_link_id": "954efa27cf26a83ebcb782eeb701c904cc93cd6d",
    "resource_link_title": "Example Assignment",
    "roles": "Instructor",
    "tool_consumer_info_product_family_code": "canvas",
    "tool_consumer_info_version": "cloud",
    "tool_consumer_instance_contact_email": "notifications@instructure.com",
    "tool_consumer_instance_guid": "WiYEKZJIYozJ60jtONj9XJdFwpxtMAY075lLT4pj:canvas-lms",
    "tool_consumer_instance_name": "The University of British Columbia",
    "user_id": "56adf6953222e4ac2d851361684c73f392b55260",
    "user_image": "https://canvas.ubc.ca/images/thumbnails/760593/qr2ss6cgf7q1SCbCH8axswflnIfFZep5XNIkNTEB",
    "oauth_signature": "<redacted>"
  }
}
```
