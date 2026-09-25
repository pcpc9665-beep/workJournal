**`git stash` is a built-in Git command that temporarily saves your uncommitted local changes** so you can switch tasks or branches without losing your work or making an incomplete, messy commit.

When you run `git stash`, Git takes all your modified tracked files (both staged and unstaged), safely stores them away in a local "stash stack," and reverts your working directory back to a clean state matching the last commit (`HEAD`)

💡 Why Use It? (Common Use Cases)

- **Urgent Bug Fixes:** You are halfway through writing a new feature on `branch-A`, but an urgent bug breaks production. You cannot switch branches with messy, uncommitted changes. You can `git stash` your work, switch to `main` to fix the bug, and then come back and recover your feature progress later. 

- **Pulling New Updates:** You want to pull the latest changes from a remote repository via `git pull`, but Git blocks you because your local changes conflict with the incoming updates. Stashing clears the deck so you can safely pull.

- **Testing an Idea:** You want to test a completely different approach without losing the code you just spent the last hour writing.


🛠️ Essential Git Stash Commands

| Command                                 | What it Does                                                                                                             |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| **`git stash`** or **`git stash push`** | Saves your current changes and returns your working directory to a clean state.                                          |
| **`git stash push -m "your message"`**  | Saves your stash with a specific description so you can easily identify it later.                                        |
| **`git stash -u`**                      | Includes **untracked files** (brand new files you haven't added to Git yet). By default, standard stashing ignores them. |
| **`git stash list`**                    | Shows a list of all your saved stashes organized like a stack (e.g., `stash@{0}` is the newest).                         |
| **`git stash pop`**                     | Restores your most recently stashed changes **and deletes** them from the stash stack.                                   |
| **`git stash apply`**                   | Restores your stashed changes but **keeps them** in the stash list for safekeeping.                                      |
| **`git stash drop stash@{0}`**          | Manually deletes a specific stash from your list.                                                                        |
| **`git stash clear`**                   | Permanently deletes **all** of your stashes.                                                                             |