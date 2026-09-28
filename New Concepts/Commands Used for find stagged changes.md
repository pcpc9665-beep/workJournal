
Here is the commands used to find the changes that in Staged changes 

```
git diff --staged 
```

```
git diff --cached
```


ഈ commands ഉപയോഗിച്ച് നമ്മുക്ക് staged ചെയ്ത changes ന്റെ difference എന്തൊക്കെ എന്ന് കാണാൻ സാധിക്കും

git diff --staged / --cached

- **Usage:** Displays the exact differences between your staging area and your last commit (`HEAD`).
- **Main usage:** This command is used to review exactly what changes you are about to commit to your project history. It serves as a final safety check. By reviewing the precise line-by-line modifications before running `git commit`, you can ensure no debugging code, temporary notes, or unwanted file changes accidentally slip into your repository.

Real-world example

Imagine you are fixing a login bug in a file called `auth.js`. You also added a quick tracking comment in `config.json`.

You stage the authentication fix using `git add auth.js`, but you leave `config.json` unstaged because it is not ready. Running `git diff --staged` will output a color-coded diff showing _only_ the specific code lines you modified and staged inside `auth.js`. It completely ignores `config.json`, allowing you to safely verify your login fix before committing.

Useful tips

- **View file names only:** Use `git diff --staged --name-only` to see a quick list of staged files without drowning in lines of code.
- **Ignore whitespace changes:** If your code formatter added accidental spaces or tabs, use `git diff --staged -w` to hide whitespace noise and see only the actual code changes.
- **See change statistics:** Use `git diff --staged --stat` to view a summary of how many lines were inserted or deleted per file.
- **Avoid syntax chaining errors:** Never run `git diff --staged git diff --cached` on the same line without a separator like `&&`, or Git will look for non-existent files.

## Conclusion

The `git diff --staged` and `git diff --cached` commands are identical twin commands that act as your final gateway review. Using them regularly builds great version control habits, ensuring that every single commit you make to your codebase is clean, intentional, and error-free.