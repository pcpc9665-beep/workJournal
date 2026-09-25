**`git stash` is a built-in Git command that temporarily saves your uncommitted local changes** so you can switch tasks or branches without losing your work or making an incomplete, messy commit.

When you run `git stash`, Git takes all your modified tracked files (both staged and unstaged  ), safely stores them away in a local "stash stack," and reverts your working directory back to a clean state matching the last commit (`HEAD`)

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

⚠️ Crucial Things to Remember

- **Local Only:** Stashes are strictly saved on your local machine. They are never pushed to GitHub or shared with other developers when you run `git push`.
- **Merge Conflicts:** If you apply a stash to a file that has changed significantly since you stashed it, Git might trigger a **merge conflict**. You will need to manually open the file, resolve the conflicts, and then safely make a commit.

## Is stash considered only staged changes?


No, `git stash` saves both staged AND unstaged changes by default.

As long as Git is already tracking the file (meaning you haven't just created it as a brand-new file), `git stash` will safely pocket your work whether you have run `git add` on it or not.

Here is exactly how `git stash` treats different types of files:

## 📋 What Git Stash Handles

|File State|Does `git stash` save it?|Explanation|
|---|---|---|
|Staged Changes|Yes|Any modified files you already ran `git add` on.|
|Unstaged Changes|Yes|Existing files you edited but haven't run `git add` on yet.|
|Untracked Files|No (by default)|Brand-new files you just created. Git doesn't know they exist yet.|
|Ignored Files|No (by default)|Files listed in your `.gitignore` (like `node_modules/` or `.env`).|

---

## 💡 How to include those "missing" files

If you want to stash absolutely everything—including brand-new files or ignored files—you just need to add a small flag to your command:

- To include brand-new (untracked) files:
    
    ```bash
    git stash -u
    # (or git stash --include-untracked)
    ```
    
- To include everything, including ignored files:
    
    ```bash
    git stash -a
    # (or git stash --all)
    ```
    

## 🔍 How they look when you restore them

When you run `git stash pop` or `git stash apply`, Git will bring back both your staged and unstaged work, but it will bring them all back as unstaged by default.

If you want Git to remember exactly what was staged and what wasn't, add the `--index` flag:

```bash
git stash pop --index
```

