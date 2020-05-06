# Understanding a Linux based OS:  Offtopic - Teamwork using `git`

> `git` is a version controlling system (VCS), which helps many users to work on a single project simultaneously.

- It helps you to work on your project too!

## Hosting Platforms
- It is the place where your code would be hosted
- [GitHub](https://github.com/), [GitLab](https://about.gitlab.com/) and [BitBucket](https://bitbucket.org/product) are popular
- We will be using [GitHub](https://github.com/) for demonstration

## Working with repositories
- Contains the code for a single project
- May contain one or more branches
  - `master` branch is the default
  - You can create many branches are required
- The repository can be downloaded to the local system for collaborating (or using) by using `clone`
    ```
        git clone <url>
    ```
- Existing files can be added to the repository using `add`
    ```
        git add <files>
    ```
- Changes in the source code (in one or more files) is saved as `commit`, and each commit has a description associated with it
    ```
        git commit -m <message> <files>
    ```
- Others' changes can be downloaded using `pull`
    ```
        git pull
    ```
  - If there are conflicts and any automatic strategy is unable to resolve it, we have to resolve it manually
- Our changes can be uploaded using `push`
    ```
        git push
    ```
## Managing the code
- One can directly `push` their `commit`s to `master` until the project reaches some stable state
- After that, the changes may break the existing code. Therefore, it is safe to create a new branch using `checkout`
    ```
        git checkout -b <new branch name>
    ```
- Branches can be switched between using `checkout`
    ```
        git checkout <branch name>
    ```
- On new branch, further `commit`s can be done
- Once the new branch reaches a stable state, we can merge it with `master`
- Creating a **Pull Request** for that branch to `master` at GitHub makes this task easy. Once **Pull Request** is accepted, the code at the branch will be at the master. It is still required to resolve conflicts manually.

## Contributing the code
- Other people can `fork` our repositories and continue `commit`ing there if they don't have write access to the repository. If they want their `commit`s appearing on the main repository, they can create **Pull Request** as of above
- Maintainers (like your project teammates) can have write access to the repository. In that case, they can directly `commit` to any branch of the repository
- Since others may have `commit`ed to the repository before we do, it is a best practice to do `git pull` before `git push`
------------

This is just an introduction to `git` and GitHub. For a detailed guide, refer some [resources](https://try.github.io/)!