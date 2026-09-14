# Git SSH Setup

നിങ്ങളുടെ നിലവിലെ പ്രധാന അക്കൗണ്ട് മാറ്റാതെ തന്നെ മറ്റൊരു അക്കൗണ്ട് ഉപയോഗിക്കാനുള്ള steps (ഘട്ടങ്ങൾ) താഴെ പറയുന്നു.
## to open in notepad  
REM Open the SSH config file in Notepad 
## notepad %USERPROFILE%\.ssh\config 

## 1. പുതിയ കീ നിർമ്മിക്കാൻ

പുതിയ SSH key (എസ്.എസ്.എച്ച് കീ) നിർമ്മിക്കാൻ കമാൻഡ് പ്രോംപ്റ്റിൽ ഇത് നൽകുക: ssh-keygen -t ed25519 -C "[pcpc9665@gmail.com](mailto:pcpc9665@gmail.com)" -f %USERPROFILE%.ssh\id_work
## 2. കോൺഫിഗ് ഫയൽ മാറ്റാൻ

നിങ്ങളുടെ SSH config file (എസ്.എസ്.എച്ച് കോൺഫിഗ് ഫയൽ) നോട്ട്പാഡിൽ തുറന്ന് താഴെ പറയുന്നവ ചേർക്കുക: Host github-work HostName [github.com](http://github.com/) User git IdentityFile ~/.ssh/id_work
## 3. റിമോട്ട് യു.ആർ.എൽ മാറ്റാൻ

HTTPS-ന് പകരം SSH (എസ്.എസ്.എച്ച്) ഉപയോഗിക്കാനായി, പ്രൊജക്റ്റ് ഫോൾഡറിൽ ഈ കമാൻഡ് നൽകുക: git remote set-url origin git@github-work:pcpc9665-beep/personal_blog.git
## 4. ലോക്കൽ ഇമെയിൽ മാറ്റാൻ

ഈ പ്രൊജക്റ്റിന് മാത്രം പുതിയ ഇമെയിൽ ഉപയോഗിക്കാൻ: git config user.email "[pcpc9665@gmail.com](mailto:pcpc9665@gmail.com)"

## 5. മൂന്നാമതൊരു അക്കൗണ്ട് ചേർക്കാൻ (Adding an additional configuration)

പുതിയൊരു കീ നിർമ്മിക്കുക: `ssh-keygen -t ed25519 -C "[your_third_email@gmail.com](mailto:your_third_email@gmail.com)" -f %USERPROFILE%\.ssh\id_third`

കോൺഫിഗ് ഫയലിൽ താഴെ പറയുന്നവ കൂടി ചേർക്കുക:


```

  

# Third Account

  

Host github-third

  

    HostName [github.com](http://github.com/)

  

    User git

  

    IdentityFile ~/.ssh/id_third

  

```

പ്രൊജക്റ്റ് യു.ആർ.എൽ മാറ്റാൻ: `git remote set-url origin git@github-third:username/repository.git`