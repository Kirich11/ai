---
name: create-git-worktree
description: > 
   Create a git-worktree based on named folder and branch.
   Triggers when user asks to create a git-worktree.
   Triggers when user asks the create an epic or a set of tasks,
   then ask for the branch name and task/epic name.  
---

# Create git worktree Skill

Create a folder using git command `git worktree add --track -b <branch> <path> <original-branch>`
The folder name should follow this template {original-project-name}-{epic/task name}.
In the created folder create or update the Makefile.local.mk and add an env PROJECT_NAME={epic/task name}

## Required Information

Before execution, gather from the user:
1. **branch** - desired branch name
2. **path** - the path for the directory to be used
3. **original-branch** - original branch
4. **epic/task name** - name for the task, to be used as the path name suffix
