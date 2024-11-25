# Power Editor Excercies

Explore different options to do automated edits to multiple text files using `bash` and text processing tools like `sed` and `awk`.

This repository mimics the configuration directory of a Drupal installation with multiple subsites.

## Step Preparation

Create a new git branch so to check and compare your steps outputs.

`git checkout -b mysteps`

## Step 1: Diff apply

Copy all changes on multiple files to different folder.

### Tools of Choice
* git diff
* patch

### Work Steps

```bash
# Get all tags.
git fetch --tags

# Make directory for patches.
mkdir patches

# Extract the chagnes dones in the `step/0` tag.
git diff step/0^..step/0 > patches/step1.patch

# Apply the patch in the hr.arkdev.net sub-directory ignoring the 3 parent
# cd config/hr.arkdev.net/
# patch ... < ../../patches/step1.patch

# Confirm the results 

# Manually remove the `consumer_server: 0` line from `config/hr.arkdev.net/core.extension.yml`
```

## Setp 2: Append lines based on pattern

Redo some text line edits to a another nonidentical file.

### Tools of Choice
* sed

### Work Steps

```bash
# Add `  quick_node_clone: 0` entry to `config/default/core.extension.yml` at line 9.
sed ... config/default/core.extension.yml

# Insert `  quick_node_clean:-` entry to `config/hr.arkdev.net/core.extension.yml` after the line starting with `node:`
sed ... config/hr.arkdev.net/core.extension.yml
```


## Step 3: Repeat for all other directories

Repeat Step 2 but for multiple directories

## Tools of Choice
* bash (for loop)
* grep
* cut

### Work Steps

```bash
# Get config directories without grep
CONFIG_DIRS=`ls config/*.arkdev.net | cut -d / -f 2`

# Compair output to the following
CONFIG_DIRS=`ls config/ | grep -v default | cut -d / -f 2``

# Loop over directories to do the edits
for CONFIG_DIR in $CONFIG_DIRS; do sed ... "config/$CONFIG_DIR/core.extension.yml"; done
```

## (Bonus) Setp 4: Repeat using different tools

Repeat Setp 2 but using `awk` instead of `sed`.
Write the edits in a function and use it inside the loop instead.

## Tools of Choice
* awk
* bash (function)

