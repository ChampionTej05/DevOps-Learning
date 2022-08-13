# Source Control Management

Also knows as Version Control System

## GIT

Workflow

1. Initialise GIT  Repository: Source Code to be tracked
2. Configure which files to track
3. Git tracking changes
4. Stage Changes
5. Commit changes


### Get Started

1. Install GIT : `yum install git`
2. Version : `git version`
3. Initialise the git Repository : `git init`
4. Status of files : `git status`
5. Add Tracking files and stage the changes: `git add [fileNames]`
6. Commit message : `git commit -m "message"`
7. Add Remote : `git remote add <name> <remoteRepoURL>` "name" is usually set to origin
    - `git remote -v` : to see the remote
8. Push : `git push <name> <branch>` master branch is default "-u" for force push
9. Pull : `git pull` Pulls changes 


## Git Basics

1. **GIT** is tool to keep track of change and **GITHUB** is web server to keep track of remote Repositories
