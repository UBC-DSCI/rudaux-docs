# Documentation for Rudaux

Welcome to the documentation for rudaux!

For information on the motivation behind and development of rudaux, please read my blog post _[Designing Rudaux](https://samhinshaw.com/blog/designing-rudaux)_. For information on how to use Rudaux to integrate Canvas and JupyterHub, please read _[Using Rudaux](https://samhinshaw.com/blog/using-rudaux)_.

## Setup

Before setting up rudaux, it is important to have the proper infrastructure in place. Please see the [DSCI 100 infrastructure repository](https://github.ubc.ca/UBC-DSCI/dsc100-infra) for our reproducible infrastructure provisioning workflow.

_Note_: rudaux currently requires a fork of nbgrader to work properly ([more information](https://github.com/samhinshaw/rudaux/issues/7)):

```sh
pip install git+git://github.com/samhinshaw/nbgrader.git
```

Once your servers are set up and your dependencies installed, rudaux needs a configuration file to operate. Please read the [configuration](config) documentation for more information and a sample config file.

1. Log in to the server you will be executing rudaux commands on.
2. Clone your instructors repository containing your config file and master (source) assignments.
3. Initialize rudaux.

## Course Operations

- Find external tool ID in Canvas
- Pull student list from Canvas and sync with nbgrader.
  - Add missing students to nbgrader database
  - Delete students from database that have withdrawn from course.
- Pull assignments from config file.
  - Add assignments to nbgrader database.
- Create student version of assignments with nbgrader assign.
  - Commit these files to the instructors repository & push.
  - Copy the student version to the public student repository.
  - Commit these files to the students repository & push.
- Create assignments in Canvas.
  - Generate urlencoded nbgitpuller links to JupyterHub, referencing the relevant notebook in the public students repository.
- Schedule grading.
  - Add cron jobs to crontab which will initiate autograding at the assignment due date.
