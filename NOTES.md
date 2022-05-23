# A runner
- Any machine with the Github Actions runner application installed.

- A runner is responsable for running your jobs whenever an event happens and displays back the results

- It can be hosted by Github or you can host your own runner

- Github hosted runners common is Linux, Windows, MacOS
    - In this case you cannot customize the hardware configuration
    - Maintained by Github


# Summary
Event
    - push
    - pr

Workflow

Job

Step

Action

Virtual Environement

Runners


# Example 1 running comands in parallel windows and ubuntu:
jobs:
  run-shell-comands:
    runs-on: ubuntu-latest
    steps:
      - name: echo a string
        run: echo "Hello World"
      - name: multiline script
        run: |
          node -v
          npm -v
      - name: python Command
        run: |
          import platform
          print(platform.processor())
        shell: python
  run-windows-commands: 
    runs-on: windows-latest
    steps:
      - name: Directory PowerShell
        run: Get-Location  
      - name: Directory Bash
        run: pwd
        shell: bash
# Example two running when it's need a dependency
jobs:
  run-shell-comands:
    runs-on: ubuntu-latest
    steps:
      - name: echo a string
        run: echo "Hello World"
      - name: multiline script
        run: |
          node -v
          npm -v
      - name: python Command
        run: |
          import platform
          print(platform.processor())
        shell: python
  run-windows-commands: 
    runs-on: windows-latest
    needs: [
      'run-shell-comands'
    ]
    steps:
      - name: Directory PowerShell
        run: Get-Location  
      - name: Directory Bash
        run: pwd
        shell: bash

# Hello World Javascript git action:

name: Actions Workflow

on: [push]

jobs:
  run-github-actions:
    runs-on: ubuntu-latest
    steps:
      - name: Simple JS Action
        id: greet
        uses: actions/hello-world-javascript-action@v1
        with:
          who-to-greet: John
      - name: Log Greeting Time
        run: echo "${{ steps.greet.outputs.time }}"

# INstalling the checkout repository and seeing the files in the repository:
name: Actions Workflow

on: [push]

jobs:
  run-github-actions:
    runs-on: ubuntu-latest
    steps:
      - name: List Files
        run: |
          pwd
          ls
      - name: Checkout
        uses: actions/checkout@v1
      - name: List files After Checkout
        run: |
          pwd
          ls -a        
      - name: Simple JS Action
        id: greet
        uses: actions/hello-world-javascript-action@v1
        with:
          who-to-greet: John
      - name: Log Greeting Time
        run: echo "${{ steps.greet.outputs.time }}"
