---
layout: post
title: "Github(PRO): clone a specific folder with sparse"
date: 2025-06-04 23:29:57 +0200
categories: blog
---

# Github(PRO) pills - clone a specific folder with sparse

> is a bit like the discovery of hot water... exists but not used. For the record, it’s part of those pearls that I circumvented with extreme methods. One of those options that have been on github for a long time.  That’s why after the umpteenth request by mail request is the case to sign as you do. In full KISS philosophy: simple, clear. When I forget it I will have the notes to remind me that it already exists. 
>
> To understand we take a repo . git that contains subdirectories and let’s make a practical example of how to download one of the folders present individually

**EXAMPLE 1**: repo is *https://github.com/aicsx/dots.git* and I just want to clone **.vim/colors/** . Let's go!

```bash
❯ git clone --filter=blob:none --no-checkout https://github.com/aicsx/dots.git && cd dots
Clone in 'dots' in corso...
remote: Enumerating objects: 30, done.
remote: Counting objects: 100% (30/30), done.
remote: Compressing objects: 100% (23/23), done.
Ricezione degli oggetti: 100% (30/30), 4.13 KiB | 4.13 MiB/s, fatto.
Risoluzione dei delta: 100% (5/5), fatto.
remote: Total 30 (delta 5), reused 24 (delta 3), pack-reused 0 (from 0)

dots on  main [✘] 
❯ git sparse-checkout set --cone

dots on  main [✘] 
❯ git checkout main
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 1 (delta 0), pack-reused 0 (from 0)
Ricezione degli oggetti: 100% (3/3), 2.16 KiB | 2.16 MiB/s, fatto.
Si è già su 'main'
Il tuo branch è aggiornato rispetto a 'origin/main'.

dots on  main 
❯ git sparse-checkout set .vim/colors/
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 1 (delta 0), pack-reused 0 (from 0)
Ricezione degli oggetti: 100% (1/1), 3.34 KiB | 3.34 MiB/s, fatto.


```

> You can use a simple bash script for this! save *clone-subdir.sh*
>
> **NOTE:** Required Git ≥ 2.25 (support for sparse-checkout --cone)

```bash
~$ vim clone_subdir.sh 
```

```bash
#!/bin/bash

# Check that two arguments are provided
if [ "$#" -ne 2 ]; then
    echo "Usage: $0 <repository-url> \"<subdirectory>\""
    exit 1
fi

REPO_URL="$1"
SUBDIR="$2"
REPO_NAME=$(basename "$REPO_URL" .git)

# Clone the repository in sparse mode
git clone --filter=blob:none --no-checkout "$REPO_URL" "$REPO_NAME"
cd "$REPO_NAME" || exit 1

# Initialize sparse-checkout
git sparse-checkout init --cone
git sparse-checkout set "$SUBDIR"

# Checkout the specified subdirectory
git checkout

echo "Subdirectory \"$SUBDIR\" has been cloned into ./$REPO_NAME/$SUBDIR"

```

```bash
:wq #save and exit
```

**Usage example:**

```bash
./clone_subdir.sh https://github.com/user/project.git "path/to/subdirectory"
```

## as I mentioned there is an older version. Not necessarily old is worse. In my use case I prefer it because it avoids cloning some basic parts of the base url. That’s why I share it with you. 

```bash
#!/bin/bash

# Check parameters
if [ "$#" -ne 2 ]; then
    echo "Usage: $0 <repository-url> \"<subdirectory>\""
    exit 1
fi

REPO_URL="$1"
SUBDIR="$2"
REPO_NAME=$(basename "$REPO_URL" .git)

# Clone the repository without checkout
git clone --no-checkout "$REPO_URL" "$REPO_NAME"
cd "$REPO_NAME" || exit 1

# Enable sparse checkout manually (for older Git versions)
git config core.sparseCheckout true

# Define the subdirectory to checkout
echo "$SUBDIR/" > .git/info/sparse-checkout

# Perform the checkout
git checkout

# Initialize and update any submodules inside the subdirectory
if [ -f .gitmodules ]; then
    # Filter submodules located within the specified subdirectory
    SUBMODULES=$(git config --file .gitmodules --get-regexp path | awk -v sub="$SUBDIR/" '$2 ~ "^"sub {print $2}')
    
    if [ -n "$SUBMODULES" ]; then
        git submodule init
        for sm in $SUBMODULES; do
            git submodule update --depth 1 "$sm"
        done
    fi
fi

echo "Subdirectory \"$SUBDIR\" has been cloned into ./$REPO_NAME/$SUBDIR"
```

**USAGE EXAMPLE**:

```bash
./clone_subdir_compat.sh https://github.com/user/project.git "src/components"
```

What this version supports:

    ✅ Older Git versions (≥ 1.7.0)
    
    ✅ Manual sparse-checkout using .git/info/sparse-checkout
    
    ✅ Submodules located inside the subdirectory
    
    ✅ Only checks out the selected path

That's it!
---

(END)
