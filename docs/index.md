# Documentation for Rudaux

Course Operations:

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
