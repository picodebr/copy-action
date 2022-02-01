# PiCode Copy Action

Easily copy files and directories to a remote location.

## Usage

```yml
- uses: picodebr/copy-action@v1
  with:
    # remote server private ssh key (required)
    ssh_key: ""

    # remote server username (required)
    ssh_user: ""

    # remote server address (required)
    ssh_host: ""

    # list of files and directories to be copied (required)
    source: ""

    # remote server destination path to paste the copied data (required)
    destination: ""
```

## Example

The following example shows how to update the remote Nodejs application everytime the master branch is updated.

```yml
name: Deploy node.js

on:
  push:
    branches: [master]

jobs:
  main:
    runs-on: ubuntu-latest
    steps:
      - name: Copy source files
        uses: actions/checkout@v2

      - name: Setup node environment
        uses: actions/setup-node@v2
        with:
          version: "16.13"

      - name: Install dependencies and build the project
        run: |
          yarn
          yarn build

      # Here we use this action
      - name: Copy files
        uses: picodebr/copy-action
        with:
          ssh_key: ${{ secrets.SSH_KEY }}
          ssh_user: ${{ secrets.SSH_USER }}
          ssh_host: ${{ secrets.SSH_HOST }}
          source: node_modules dist package.json
          destination: ~/application-folder

      - name: Restart the application
      # Restart logic goes here...
```
