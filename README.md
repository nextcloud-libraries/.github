<!--
SPDX-FileCopyrightText: 2021-2024 Nextcloud GmbH and Nextcloud contributors
SPDX-License-Identifier: MIT
-->

# This repository contains Nextcloud's workflow templates for libraries

## Setup a new template on your repository

When creating a new workflow on your repository, you will see templates originating from here.

## Update workflows with a script

You can run the following shell script on your machine to update all workflows of an app. It should be run inside the cloned repository of an app and requires rsync to be installed.

⚠️ Do not forget to check the diff for unwanted changes before committing, especially when updating the workflows on stable branches!

```sh
#!/bin/sh

# Update GitHub workflows from the Nextcloud template repository.
# This script is meant to be run from the root of the repository.

# Sanity check
[ ! -d ./.github/workflows/ ] && echo "Error: .github/workflows does not exist" && exit 1

# Clone template repository
temp="$(mktemp -d)"
git clone --depth=1 https://github.com/nextcloud-libraries/.github.git "$temp"

# Update workflows
rsync -vr \
    --existing \
    --include='*/' \
    --include='*.yml' \
    --exclude='*' \
    "$temp/workflow-templates/" \
    ./.github/workflows/

# Cleanup
rm -rf "$temp"
```
