

**Convention over Configuration (CoC)** എന്നത് ഒരു [[software design paradigm]] ആണ്. Frameworks ഉപയോഗിക്കുമ്പോൾ developer-ന്റെ ജോലിഭാരം കുറയ്ക്കുന്നതിനും, തീരുമാനങ്ങൾ എടുക്കുന്നത് എളുപ്പമാക്കുന്നതിനും വേണ്ടിയാണ് ഇത് പ്രധാനമായും ഉപയോഗിക്കുന്നത്.
 
ലളിതമായി പറഞ്ഞാൽ, ഒരു framework മുൻകൂട്ടി നിശ്ചയിച്ച ചില rules അല്ലെങ്കിൽ "defaults" (Conventions) വെച്ചിട്ടുണ്ടാകും. നമ്മൾ അത് അതേപടി follow ചെയ്താൽ, ഒരുപാട് configuration files-ഉം boilerplate code-ഉം എഴുതുന്നത് ഒഴിവാക്കാം. നമുക്ക് ആ default രീതി മാറ്റണം എന്ന് നിർബന്ധം ഉണ്ടെങ്കിൽ മാത്രം അതിനുള്ള configuration എഴുതിയാൽ മതി.
### ഒരു Real-World Analogy:

നിങ്ങൾ ഒരു ഹോട്ടലിൽ പോയി ഒരു "Meals" ഓർഡർ ചെയ്യുന്നു എന്ന് വിചാരിക്കുക. നിങ്ങൾക്ക് ചോറ്, സാമ്പാർ, തോരൻ, പപ്പടം എന്നിവ തനിയെ ലഭിക്കും. ഇതാണ് **Convention**. കാരണം ഹോട്ടലുകാർക്ക് മുൻകൂട്ടി അറിയാം സാധാരണ ഒരു meals-ൽ എന്തൊക്കെ ഉണ്ടാകണം എന്ന്. ഇതിനായി നിങ്ങൾ ഓരോ കറിയും പ്രത്യേകം ഓർഡർ ചെയ്യേണ്ടതില്ല (അതായത് **Configuration** ആവശ്യമില്ല). എന്നാൽ നിങ്ങൾക്ക് സാമ്പാറിന് പകരം മീൻ കറി വേണമെങ്കിൽ മാത്രം അത് പ്രത്യേകം പറയണം (ഇവിടെ നിങ്ങൾ default configuration മാറ്റുന്നു).
### Technical Examples (How it works under the hood):

**1. Next.js Routing (vs React Router)**

- **Configuration (React):** സാധാരണ React-ൽ നമ്മൾ React Router ഉപയോഗിക്കുമ്പോൾ ഓരോ route-ഉം manually configure ചെയ്യണം. ഉദാഹരണത്തിന്, `/about` എന്ന URL-ൽ പോകാൻ, ആ path-ഉം അതിനുള്ള component-ഉം നമ്മൾ code വഴി link ചെയ്യണം.
    
      
    
- **Convention (Next.js):** Next.js-ൽ നമ്മൾ routes manually configure ചെയ്യേണ്ടതില്ല. അതിലെ convention അനുസരിച്ച്, `app` അല്ലെങ്കിൽ `pages` folder-നുള്ളിൽ `about` എന്നൊരു folder ഉണ്ടാക്കി അതിൽ `page.tsx` എന്ന file വെച്ചാൽ, framework തനിയെ `/about` എന്ന route ആയി അതിനെ മാറ്റുന്നു. ഇവിടെ file/folder structure ആണ് convention.
    
      
    

**2. MongoDB Collections (Mongoose)**

- നമ്മൾ Mongoose ഉപയോഗിച്ച് `User` എന്ന പേരിൽ ഒരു Model create ചെയ്താൽ, database-ൽ അത് തനിയെ `users` (plural, lowercase) എന്ന collection ആയി മാറും. ഇത് framework-ന്റെ ഒരു convention ആണ്. ഏത് collection-ലേക്ക് save ചെയ്യണം എന്ന് നമ്മൾ പ്രത്യേകം configure ചെയ്ത് പറയേണ്ടതില്ല. വേണമെങ്കിൽ മാത്രം അത് override ചെയ്ത് നൽകാം.
    
      
    
### പ്രധാന ഗുണങ്ങൾ (Benefits):

- **Reduces Boilerplate Code:** ഒരേ കാര്യത്തിന് വേണ്ടി ആവശ്യമില്ലാത്ത ഒരുപാട് code എഴുതുന്നത് ഒഴിവാക്കാം.
    
      
    
- **Faster Development:** Developer-ന് എവിടെ, എന്ത് പേരിൽ file വെക്കണം എന്നതിനെക്കുറിച്ച് ഓരോ തവണയും ആലോചിച്ച് സമയം കളയേണ്ടതില്ല.
    
      
    
- **Maintainability:** പുതിയ ഒരാൾക്ക് project structure മനസ്സിലാക്കാൻ എളുപ്പമായിരിക്കും, കാരണം എല്ലാ projects-ഉം ഒരേ conventions ആയിരിക്കും follow ചെയ്യുന്നത്.
    
      
    

ചുരുക്കത്തിൽ, standard defaults-നെ വിശ്വസിക്കുകയും, framework നിർദ്ദേശിക്കുന്ന രീതിയിൽ കാര്യങ്ങൾ ചെയ്യുകയും വഴി development വേഗത്തിലാക്കുന്ന രീതിയാണ് Convention over Configuration.