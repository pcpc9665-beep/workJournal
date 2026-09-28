
Here is the commands used to find the changes that in Staged changes 

```
git diff --staged 
```

```
git diff --cached
```


ഈ commands ഉപയോഗിച്ച് നമ്മുക്ക് staged ചെയ്ത changes ന്റെ difference എന്തൊക്കെ എന്ന് കാണാൻ സാധിക്കും

git diff --staged / --cached

- **ഉപയോഗം:** നിങ്ങളുടെ സ്റ്റേജിംഗ് ഏരിയയും അവസാന കമ്മിറ്റും (`HEAD`) തമ്മിലുള്ള കൃത്യമായ വ്യത്യാസങ്ങൾ പ്രദർശിപ്പിക്കുന്നു.
- **പ്രധാന ഉപയോഗം:** നിങ്ങളുടെ പ്രോജക്റ്റ് ചരിത്രത്തിൽ നിങ്ങൾ എന്ത് മാറ്റങ്ങൾ വരുത്താൻ പോകുന്നുവെന്ന് കൃത്യമായി അവലോകനം ചെയ്യാൻ ഈ കമാൻഡ് ഉപയോഗിക്കുന്നു. ഇത് ഒരു അന്തിമ സുരക്ഷാ പരിശോധനയായി വർത്തിക്കുന്നു. `git commit` പ്രവർത്തിപ്പിക്കുന്നതിന് മുമ്പ് കൃത്യമായ വരികൾ-തോറും പരിഷ്കാരങ്ങൾ അവലോകനം ചെയ്യുന്നതിലൂടെ, ഡീബഗ്ഗിംഗ് കോഡ്, താൽക്കാലിക കുറിപ്പുകൾ അല്ലെങ്കിൽ അനാവശ്യ ഫയൽ മാറ്റങ്ങൾ നിങ്ങളുടെ ശേഖരത്തിലേക്ക് ആകസ്മികമായി വഴുതിവീഴുന്നില്ലെന്ന് നിങ്ങൾക്ക് ഉറപ്പാക്കാൻ കഴിയും.

Real-world example

`auth.js` എന്ന ഫയലിൽ നിങ്ങൾ ഒരു ലോഗിൻ ബഗ് പരിഹരിക്കുകയാണെന്ന് സങ്കൽപ്പിക്കുക. `config.json`-ൽ നിങ്ങൾ ഒരു ദ്രുത ട്രാക്കിംഗ് അഭിപ്രായവും ചേർത്തു.

`git add auth.js` ഉപയോഗിച്ച് നിങ്ങൾ പ്രാമാണീകരണ പരിഹാരം സ്റ്റേജ് ചെയ്യുന്നു, പക്ഷേ അത് തയ്യാറാകാത്തതിനാൽ `config.json` സ്റ്റേജ് ചെയ്യാതെ വിടുന്നു. `git diff --staged` പ്രവർത്തിപ്പിക്കുന്നത്, `auth.js`-നുള്ളിൽ നിങ്ങൾ പരിഷ്കരിച്ചതും സ്റ്റേജ് ചെയ്തതുമായ നിർദ്ദിഷ്ട കോഡ് ലൈനുകൾ _മാത്രം_ കാണിക്കുന്ന ഒരു കളർ-കോഡഡ് ഡിഫ് ഔട്ട്പുട്ട് ചെയ്യും. ഇത് `config.json` നെ പൂർണ്ണമായും അവഗണിക്കുകയും, കമ്മിറ്റ് ചെയ്യുന്നതിന് മുമ്പ് നിങ്ങളുടെ ലോഗിൻ ഫിക്സ് സുരക്ഷിതമായി പരിശോധിക്കാൻ നിങ്ങളെ അനുവദിക്കുകയും ചെയ്യുന്നു.

Useful tips

- **View file names only:** Use `git diff --staged --name-only` to see a quick list of staged files without drowning in lines of code.
- **Ignore whitespace changes:** If your code formatter added accidental spaces or tabs, use `git diff --staged -w` to hide whitespace noise and see only the actual code changes.
- **See change statistics:** Use `git diff --staged --stat` to view a summary of how many lines were inserted or deleted per file.
- **Avoid syntax chaining errors:** Never run `git diff --staged git diff --cached` on the same line without a separator like `&&`, or Git will look for non-existent files.

## Conclusion

The `git diff --staged` and `git diff --cached` commands are identical twin commands that act as your final gateway review. Using them regularly builds great version control habits, ensuring that every single commit you make to your codebase is clean, intentional, and error-free.