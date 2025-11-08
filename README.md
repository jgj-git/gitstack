# gitstack

`gitstack` is a lightweight Bash utility for managing **stacked Git branches**,
a workflow where you maintain a series of dependent feature branches stacked 
on top of each other.
It helps you rebase and push all branches in the stack automatically from the 
top down, keeping your branch hierarchy clean and synchronized, without having 
to go through each commit in the stack and manually rebase and pushing.

## Features

- Automatically rebases **all stacked branches** below the current HEAD.
- Pushes all stacked branches (from HEAD down to `master` or `main`) to your remote.
- Simplifies workflows that rely on `git rebase --update-refs`.

## Example

Let’s say you’re working on a feature divided into **three stacked branches**:

- `feature/part1`
- `feature/part2`
- `feature/part3`

They are **stacked**, not parallel, like so:

```

o---o---o---o---o                (master, origin/master)
         \
          o'--o'--o'             (feature/part1, origin/feature/part1)
                   \
                    o'--o'--o'   (feature/part2, origin/feature/part2)
                             \
                              o'--o'--o'   (feature/part3, origin/feature/part3)
```

While working on `feature/part3`, you realized that there is a bug that was 
introduced in `feature/part1`. Wit `gitstack`, while your head is still in 
`feature/part3`, you can run

```
gitstack rebase feature/part1^1
```

This will start a git interactive rebase with `--update-refs`. You can then 
amend one of the commits in `feature/part1` to fix the bug and finish the rebase,
leaving all of your local versions of the branches updated, like so:

```
o---o---o---o---o                (master, origin/master)
         \
          o'--o'--o'             feature/part1 (updated)
                   \
                    o'--o'--o'   feature/part2 (updated)
                             \
                              o'--o'--o'   feature/part3 (updated)
```

Then, to push all updated branches to the remote repository, you can just run

```
gitstack push
```

leaving your local and remote branches completely synchronized:

```
o---o---o---o---o                (master, origin/master)
         \
          o'--o'--o'             (feature/part1, origin/feature/part1) (updated)
                   \
                    o'--o'--o'   (feature/part2, origin/feature/part2) (updated)
                             \
                              o'--o'--o'   (feature/part3, origin/feature/part3) (updated)
```

