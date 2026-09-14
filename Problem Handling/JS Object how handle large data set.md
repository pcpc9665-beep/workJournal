# How to handle when large data set of object In Js and TS

JS/TS-ൽ വലിയ ഒരു dataset (ഉദാഹരണത്തിന് ലക്ഷക്കണക്കിന് objects ഉള്ള ഒരു Array) handle ചെയ്യുമ്പോൾ പ്രധാനമായും രണ്ട് പ്രശ്നങ്ങളാണ് ഉണ്ടാകുക: ഒന്ന്, മെമ്മറി പരിധി കവിയുക (Memory Heap Overflow). രണ്ട്, ബ്രൗസറോ സെർവറോ നിശ്ചലമാകുക (Event Loop Block ആകുക).


ഈ പ്രശ്നങ്ങൾ ഒഴിവാക്കാനും അണ്ടർ-ദി-ഹുഡ് (under-the-hood) കാര്യക്ഷമത വർദ്ധിപ്പിക്കാനും താഴെ പറയുന്ന core CS strategies ഉപയോഗിക്കാം:

**1. Time Complexity കുറയ്ക്കാൻ Data Structures മാറ്റുക (Hash Maps)**

ഒരു വലിയ Array-ൽ നിന്നും ഒരു പ്രത്യേക object കണ്ടുപിടിക്കാൻ `find()` അല്ലെങ്കിൽ `filter()` ഉപയോഗിച്ചാൽ, അതിൻ്റെ Time Complexity `O(n)` ആണ്. അതായത്, 1 ലക്ഷം objects ഉണ്ടെങ്കിൽ എഞ്ചിൻ 1 ലക്ഷം തവണ loop ചെയ്യേണ്ടി വരാം.

- **പരിഹാരം:** ഡാറ്റ എളുപ്പത്തിൽ തിരയാൻ (Search & Lookup) `Map` അല്ലെങ്കിൽ Object dictionary ഉപയോഗിക്കുക. ഇതിൻ്റെ Time Complexity `O(1)` ആണ്. ഒരു Unique ID (ഉദാഹരണത്തിന് user ID) Key ആക്കി വച്ചാൽ, വലിയ loop ഇല്ലാതെ തന്നെ നേരിട്ട് ഡാറ്റ എടുക്കാൻ സാധിക്കും.
    
      
    

**2. Event Loop Block ആകാതിരിക്കാൻ Chunking ഉപയോഗിക്കുക**

JS ഒരു Single-threaded ഭാഷയാണ്. വലിയൊരു Array ഒറ്റയടിക്ക് process ചെയ്താൽ (ഉദാഹരണത്തിന് വലിയൊരു `forEach` അല്ലെങ്കിൽ `reduce`), Main Thread block ആകും. അപ്പോൾ user-ന് UI-ൽ ക്ലിക്ക് ചെയ്യാനോ scroll ചെയ്യാനോ കഴിയില്ല (UI Freeze).

- **പരിഹാരം:** വലിയ dataset-നെ ചെറിയ chunks ആയി തിരിക്കുക. `setTimeout`, `setInterval` അല്ലെങ്കിൽ `requestAnimationFrame` ഉപയോഗിച്ച് ഓരോ chunk-ഉം Asynchronous ആയി process ചെയ്യുക. ഇങ്ങനെ ചെയ്യുമ്പോൾ ഓരോ chunk കഴിയുമ്പോഴും Event Loop-ന് ഇടവേള ലഭിക്കുകയും, മറ്റ് tasks (click events, rendering) ചെയ്യാൻ സാധിക്കുകയും ചെയ്യും.
    
      
    

**3. Memory Management ഉം Garbage Collection ഉം (Streaming & Virtualization)**

Node.js-ൽ V8 engine-ന് ഒരു നിർദിഷ്ട Memory Heap limit ഉണ്ട്. ഒരു വലിയ JSON file ഒറ്റയടിക്ക് RAM-ലേക്ക് load ചെയ്താൽ Out of Memory error വന്ന് application crash ആകും.

  

- **Backend:** മുഴുവൻ ഡാറ്റയും ഒരുമിച്ച് load ചെയ്യുന്നതിന് പകരം Streams ഉപയോഗിക്കുക. Data ചെറിയ packets ആയി read ചെയ്ത് process ചെയ്ത ശേഷം Memory-യിൽ നിന്നും റിലീസ് ചെയ്യുക. ഉപയോഗം കഴിഞ്ഞ objects-നെ Garbage Collection വേഗത്തിൽ ക്ലീൻ ചെയ്യും.
    
      
    
- **Frontend:** UI-ൽ വലിയ ലിസ്റ്റ് കാണിക്കാൻ Virtualization (ഉദാഹരണത്തിന് React-ലെ Virtual DOM-ൽ) ഉപയോഗിക്കുക. സ്ക്രീനിൽ കാണുന്ന objects മാത്രം render ചെയ്യുക. ബാക്കിയുള്ളവ scroll ചെയ്യുമ്പോൾ മാത്രം memory-ലേക്ക് കൊണ്ടുവരിക.
    
      
    

**4. CPU-Intensive Tasks-ന് Web Workers ഉപയോഗിക്കുക**

വലിയ dataset-ൽ സങ്കീർണ്ണമായ sorting, filtering അല്ലെങ്കിൽ mathematical calculations ചെയ്യണമെങ്കിൽ, അത് Main Thread-ൽ ചെയ്യുന്നത് ഒഴിവാക്കണം.

- **പരിഹാരം:** Web Workers ഉപയോഗിച്ച് ഈ processing ഒരു Background Thread-ലേക്ക് മാറ്റുക. ഇതിലൂടെ Main Thread സ്വതന്ത്രമാകുകയും, processing കഴിഞ്ഞ ശേഷം മാത്രം റിസൾട്ട് Main Thread-ലേക്ക് അയച്ചു കൊടുക്കുകയും ചെയ്യാം.
    
      
    
ചുരുക്കത്തിൽ, വലിയ dataset കൈകാര്യം ചെയ്യുമ്പോൾ അനാവശ്യമായ `O(n)` iterations ഒഴിവാക്കാൻ കൃത്യമായ Data Structures (Array-ക്ക് പകരം Map) തിരഞ്ഞെടുക്കുക. ഒറ്റയടിക്ക് ഡാറ്റ ലോഡ് ചെയ്യാതെ Chunking അല്ലെങ്കിൽ Streaming വഴി Memory Heap-ഉം Event Loop-ഉം സ്വതന്ത്രമാക്കി നിർത്തുകയുമാണ് അണ്ടർ-ദി-ഹുഡ് ചെയ്യേണ്ടത്.

