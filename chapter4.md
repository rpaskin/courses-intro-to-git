---
title       : Managing repositories
description : >-
  This chapter shows you how to set up a Git project from scratch, how
  to turn an existing project into a Git repository, and how to share
  changes between repositories.

--- type:PureMultipleChoiceExercise lang:bash xp:50 key:d10c46c6d0
## How can I tell Git to ignore certain files?

Data analysis often produces temporary or intermediate files that you don't want to save.
As you learned earlier,
you can remove these with `git clean`,
but a simpler approach is to tell Git to ignore them entirely.
To do this,
create a file in the root directory of your repository called `.gitignore`
and store a list of shell wildcard patterns that specify the files you don't want Git to pay attention to.
For example,
if `.gitignore` contains:

```
build
*.mpl
```

then Git will ignore any file or directory called `build` (and, if it's a directory, anything in it),
as well as any file whose name ends in `.mpl`.

<hr>

Which of the following files would *not* be ignored by a `.gitignore` that contained the lines:

```
pdf
*.pyc
backup
```

*** =possible_answers
- [`report.pdf`]
- `bin/analyze.pyc`
- `backup/northern.csv`
- None of the above.

*** =hint

*** =feedbacks
- Correct: `pdf` does not contain any wildcards, so it only matches files called `pdf`.
- This file *is* matched because the pattern `*.pyc` matches files in sub-directories.
- This file *is* matched because `backup` is a directory, so all files in it are ignored.
- No: at least one of the files above is not ignored.

<!-- -------------------------------------------------------------------------------- -->

--- type:MultipleChoiceExercise lang:shell xp:50 skills:1 key:ee41a600fb
## How can I see how Git is configured?

Like most complex pieces of software,
Git allows you to change its default settings.
To see what the settings are,
you can use the command `git config --list` with one of three additional options:

* `--system`: settings for every user on this computer.
* `--global`: settings for every one of your projects.
* `--local`: settings for one specific project.

Each level overrides the one above it,
so local (per-project) settings take precedence over global (per-user) settings,
which in turn take precedence over system-wide settings.

<hr>

You are in the `dental` repository.
How many local configuration values are set in for this repository?

*** =instructions
- None.
- 1.
- 4.
- 12.

*** =hint

Use `git config --list --local` and count.

*** =pre_exercise_code
```{shell}
repl = connect('bash')
repl.run_command('cd dental')
```

*** =sct
```{python}
Ex().test_mc(3, ['No, some configuration values are set.',
                 'No, more configuration values are set than that.',
                 'Correct!',
                 'No, fewer configuration values are set than that.'])
```

<!-- -------------------------------------------------------------------------------- -->

--- type:NormalExercise lang:shell xp:100 skills:1 key:7c0dce348b
## How can I change my Git configuration?

Most of Git's settings should be left as they are.
However,
there are two you should set on ever computer you use:
your name and your email address.
These are recorded in the log every time you commit a change,
and are often used to identify the authors of a project's content in order to give credit
(or assign blame, depending on the circumstances).

To change a configuration value for all of your projects on a particular computer,
run the command:

```
git config --global setting.name setting.value
```

with the setting's name and value in the appropriate places.
The keys that identify your name and email address are `user.name` and `user.email` respectively.

*** =instructions

Change the email address configured for the current user for *all* projects
to `rep.loop@datacamp.com`.

*** =hint

Use `git config --global key value`.

*** =pre_exercise_code
```{shell}

```

*** =sample_code
```{shell}

```

*** =solution
```{shell}
git config --global user.email rep.loop@datacamp.com
```

*** =sct
```{python}
test_student_typed(r'\s*git\s+config\s+--global\s+user.email\s+["\']?rep.loop@datacamp.com["\']?\s*',
                   fixed=False,
                   msg='Use `git config --global` with the `user.email` property and the email address.')
```

<!-- -------------------------------------------------------------------------------- -->

--- type:NormalExercise lang:shell xp:100 skills:1 key:a87bbd3948
## How can I create a brand new repository?

So far,
you have been working with repositories that we created.
If you want to create a repository for a new project,
you can simply say `git init project-name`,
where "project-name" is the name you want the new repository's root directory to have.

One thing you should *not* do is create one Git repository inside another.
While Git does allow this,
updating nested repositories becomes very complicated very quickly,
since you need to tell Git which of the two `.git` directories the update is to be stored in.
Very large projects occasionally need to do this,
but most programmers and data analysts try to avoid getting into this situation.

*** =instructions

Use a single command to create a new Git repository called `optical` below your home directory.

*** =hint

*** =pre_exercise_code
```{shell}

```

*** =sample_code
```{shell}

```

*** =solution
```{shell}
git init optical
```

*** =sct
```{python}
test_student_typed(r'\s*git\s+init\s+optical\s*',
                   fixed=False,
                   msg='Use `git init` and a directory name.')
```

<!-- -------------------------------------------------------------------------------- -->

--- type:NormalExercise lang:shell xp:100 skills:1 key:a4330ec681
## How can I turn an existing project into a Git repository?

Experienced Git users instinctively start new projects by creating repositories.
If you are new to Git,
though,
or working with people who are,
you will often want to convert existing projects into repositories.
Doing so is simple:
just run `git init` in the project's root directory,
or `git init /path/to/project` from anywhere else on your computer.

*** =instructions

You are in the directory `dental`,
which contains data, analysis scripts, and other files,
but is not yet a Git repository.
Use a single command to convert it to a Git repository,
and then check its status.

*** =hint

*** =pre_exercise_code
```{shell}
repl = connect('bash')
repl.run_command('cd dental')
repl.run_command('rm -rf .git')
```

*** =sample_code
```{shell}
# Turn this directory into a Git repository.


# Check this newly-created repository's status.

```

*** =solution
```{shell}
git init
git status
```

*** =sct
```{python}
test_student_typed(r'\s*git\s+init\s+git\s+status\s*',
                   fixed=False,
                   msg='Use `git init` and `git status`.')
```

<!-- -------------------------------------------------------------------------------- -->

--- type:NormalExercise lang:shell xp:100 skills:1 key:9fb3b2ed49
## How can I create a copy of an existing repository?

Sometimes you will join a project that is already running,
inherit a project from someone else,
or continue working on one of your own projects on a new machine.
In each case,
you will *clone* an existing repository instead of creating a new one.
Cloning a repository does exactly what the name suggests:
it creates a copy of an existing repository (including all of its history) in a new directory.

To clone a repository,
use the command `git clone URL`,
where `URL` identifies the repository you want to clone.
This will normally be something like `https://github.com/datacamp/project.git`,
but for this lesson,
we will use filesystem URLs of the form `file:///existing/project`.
The number of slashes at the start is important:
the first part of the URL is `file://`,
and then there is a third slash to start the absolute path `/existing/project`.

When you clone a repository,
Git uses the name of the existing repository as the name of the clone's root directory.
Continuing with the example,
`git clone file:///existing/project` will create a new directory called `project`
below the directory you are in.
If you want to call the clone something else,
add the directory name you want to the command.

*** =instructions

You have just inherited the dental data analysis project from a colleague,
who tells you that all of their work is in a repository in `/home/thunk/repo`.
Use a single command to clone this repository
to create a new repository called `dental` inside your home directory
(so that the new repository is in `/home/repl/dental`).

*** =hint

Remember to count the slashes after `file:` carefully.

*** =pre_exercise_code
```{shell}
repl = connect('bash')
repl.run_command('rm -rf dental')
```

*** =sample_code
```{shell}

```

*** =solution
```{shell}
git clone file:///home/thunk/repo dental
```

*** =sct
```{python}
test_student_typed(r'\s*git\s+clone\s+file:///home/thunk/repo\s+(/home/repl/|~/|\./|)dental\s*',
                   fixed=False,
                   msg='Use `git clone` and the absolute or relative path of the directory to clone to.')
```

<!-- -------------------------------------------------------------------------------- -->

--- type:MultipleChoiceExercise lang:shell xp:50 skills:1 key:61567f27d4
## How can I find out where a cloned repository originated?

When you a clone a repository,
Git remembers where the original repository was.
It does this by storing a *remote* in the new repository's configuration.
A remote is like a browser bookmark with a name and a URL.
If you are in a repository,
you can list the names of its remotes using `git remote`.

If you want more information, you can use `git remote -v` (for "verbose"),
which shows the remote's URLs.
Note that "URLs" is plural:
it's possible for a remote to have several URLs associated with it for different purposes,
though in practice each remote is almost always paired with just one URL.

<hr>

You are in the `dental` repository.
How many remotes does it have?

*** =instructions
- None.
- 1.
- 2.

*** =hint

Run `git remote` in the `dental` repository.

*** =pre_exercise_code
```{shell}
repl = connect('bash')
repl.run_command('rm -rf dental')
repl.run_command('git clone file:///home/thunk/repo dental')
repl.run_command('cd dental')
```

*** =sct
```{python}
Ex().test_mc(2, ['No: there are some remotes.',
                 'Correct!',
                 'No: there is just one remote.'])
```

<!-- -------------------------------------------------------------------------------- -->

--- type:NormalExercise lang:shell xp:100 skills:1 key:4d5be24350
## How can I pull in changes from a remote repository?

Git keeps track of remote repositories so that you can collaborate
by pulling changes from those repositories
and pushing changes to them.
Pulling changes is straightforward:
the command `git pull remote branch`
gets everything in `branch` in the remote repository identified by `remote`
and merges it into the current branch of your local repository.
For example,
if you are in the `quarterly-report` branch of your local repository,
the command:

```
git pull thunk latest-analysis
```

would get changes from `latest-analysis` branch
in the repository associated with the remote called `thunk`
and merge them into your `quarterly-report` branch.

*** =instructions

You are in the `master` branch of the repository `dental`,
which has a remote called `origin`.
Pull all of the changes in the `master` branch of that remote repository
into the `master` branch of your repository.

*** =hint

*** =pre_exercise_code
```{shell}
repl = connect('bash')
repl.run_command('rm -rf dental')
repl.run_command('git clone file:///home/thunk/repo dental')
repl.run_command('cd dental')
repl.run_command('git reset --hard HEAD~2')
```

*** =sample_code
```{shell}

```

*** =solution
```{shell}
git pull origin master
```

*** =sct
```{python}
test_student_typed(r'\s*git\s+pull\s+origin\s+master\s*',
                   fixed=False,
                   msg='Use `git pull` with the name of the remote and the name of the branch.')
```

<!-- -------------------------------------------------------------------------------- -->

--- type:NormalExercise lang:shell xp:100 skills:1 key:b3ba4dd987
## How can I push my changes to a remote repository?

The complement of `git pull` is `git push`,
which pushes the changes you have made locally into a remote repository.
The most common way to use it is:

```
git push remote-name branch-name
```

which pushes the contents of your branch `branch-name`
into a branch with the same name
in the remote repository associated with `remote-name`.
It's possible to use different branch names at your end and the remote's end,
but doing this quickly becomes confusing:
it's almost always better to use the same names for branches across repositories.

*** =instructions

You are in the `master` branch of the `dental` repository,
which has a remote called `origin`.
You have changed one file;
commit your changes with the message "Added more northern data."
and then push your changes to the remote repository's `master` branch.

*** =hint

*** =pre_exercise_code
```{shell}
repl = connect('bash')
repl.run_command('rm -rf dental')
repl.run_command('git clone file:///home/thunk/repo dental')
repl.run_command('cd dental')
with open('dental/data/northern.csv', 'w') as writer:
    writer.write('2017-11-01,bicuspid\n')
```

*** =sample_code
```{shell}
# Use 'git add' and 'git commit' to commit your changes.


# Push your changes.

```

*** =solution
```{shell}
git add data/northern.csv
git commit -m "Added more northern data."
git push origin master
```

*** =sct
```{python}
test_student_typed(r'\s*git\s+add\s+[^\n]+\s+git\s+commit\s+-m\s+[^\n]+\s+git\s+push\s+origin\s+master\s*',
                   fixed=False,
                   msg='Use `git pull` with the name of the remote and the name of the branch.')
```


<!-- -------------------------------------------------------------------------------- -->

--- type:NormalExercise lang:shell xp:100 skills:1 key:1e327efda1
## How can I define remotes?

When you clone a repository,
Git automatically creates a remote called `origin`
that points to the original repository.
You can add more remotes using:

```
git remote add remote-name URL
```

and remove existing ones using:

```
git remote rm remote-name
```

You can connect any two Git repositories this way,
but in practice,
you will almost always connect repositories that share some common ancestry.

*** =instructions

You are in the `dental` repository.
Add `file:///home/thunk/repo` as a remote called `thunk` to it.

*** =hint

Be sure to count the slashes properly in the remote URL.

*** =pre_exercise_code
```{shell}
repl = connect('bash')
repl.run_command('cd dental')
```

*** =sample_code
```{shell}

```

*** =solution
```{shell}
git remote add thunk file:///home/thunk/repo
```

*** =sct
```{python}
test_student_typed(r'\s*git\s+remote\s+add\s+thunk\s+file:///home/thunk/repo\s*',
                   fixed=False,
                   msg='Use `git remote add` with the name and URL of the remote.')
```
