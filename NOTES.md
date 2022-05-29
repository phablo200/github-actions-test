# Marketplace github
github.com/marketplace?type=actions

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

# The checkout actions:
- Read that later
- https://github.com/actions/checkout#usage

# Echo variables and using the checkout action:
name: Actions Workflow

on: [push]

jobs:
  run-github-actions:
    runs-on: ubuntu-latest
    steps:
      - name: List Files
        run: |
          pwd
          ls -a
          echo $GITHUB_SHA
          echo $GITHUB_REPOSITORY
          echo $GITHUB_WORKSPACE
          echo "${{ github.tokan }}"
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

# Example using schedule:
name: Actions Workflow
# * * * * *
# Minuto - Horas - 
on:
  schedule:
    - cron: "0/5 * * * *"
    - cron: "0/6 * * * *"
  pull_request:
    types: [
      closed, 
      assigned, 
      opened, 
      reopened
    ]
jobs:
  run-github-actions:
    runs-on: ubuntu-latest
    steps:
      - name: List Files
        run: |
          pwd
          ls -a
          echo $GITHUB_SHA
          echo $GITHUB_REPOSITORY
          echo $GITHUB_WORKSPACE
          echo "${{ github.tokan }}"
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

Cronn Example:
  name: Actions Workflow
# * * * * *
# Minuto - Horas - 
on:
  schedule:
    - cron: "0/5 * * * *"
    - cron: "0/6 * * * *"
  pull_request:
    types: [
      closed, 
      assigned, 
      opened, 
      reopened
    ]
jobs:
  run-github-actions:
    runs-on: ubuntu-latest
    steps:
      - name: List Files
        run: |
          pwd
          ls -a
          echo $GITHUB_SHA
          echo $GITHUB_REPOSITORY
          echo $GITHUB_WORKSPACE
          echo "${{ github.tokan }}"
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

# Creating random file in your application (This case is very good to version your code):
create-issue:
    runs-on: ubuntu-latest
    steps:
    - name: Push a random file
      run: |
        pwd
        ls -a
        git init
        git remote add origin "https://${GITHUB_ACTOR}:${{ secrets.GITHUB_TOKEN }}@github.com/${GITHUB_REPOSITORY}.git"
        git config --global user.email "my-bot@bot.com"
        git config --global user.name "my-bot"
        git fetch
        git checkout master
        git branch --set-upstream-to=origin/master
        git pull
        ls -a
        echo $RANDOM >> random.txt
        ls -a
        git add -A
        git commit -m "Random file"
        git push
        
    - name: Create issue using REST API
      run: |
        curl --request POST \
        --url https://api.github.com/repos/${{ github.repository }}/issues \
        --header 'authorization: Bearer ${{ secrets.GITHUB_TOKEN }}' \
        --header 'content-type: application/json' \
        --data '{
          "title": "Automated issue for commit: ${{ github.sha }}",
          "body": "This issue was automatically created by the Github Action workflow **${{ github.workflow }}**. \n\n the commit has was: _${{ github.sha }}_."
        }'

# Crypting files

touch secret.json
echo { \
 api_key: "sjaksjlakjsakl", \
 api_user: "sjklajsklajk" \
} \
>> secret.json;
-- Or create this in the vscode

cat secret.json

1) gpg --help

2) gpg --symmetric --cipher-algo AES256 secret.json

3) Test your decript:
gpg --quiet --batch --yes --decrypt --passphrase="123mudar" --output secrets.json secrets.json.gpg

      

# Dome interesting functions
-- in some cases your job can failure, if you desure that your next job continues running, so you may use one of these functions:

-- In this case job with this condition will execute same that the previous job has been failured

- In this ecample the previous job failured because of the "eccho"
- name: Dump GitHub context
  env:
    GITHUB_CONTEXT: ${{ toJson(github) }}
  run: eccho "$GITHUB_CONTEXT"
- name: Dump job context
  if: failure()
  env:
    JOB_CONTEXT: ${{ toJson(job) }}
  run: echo "$JOB_CONTEXT"

-- Another option is use the always() function, this function always execute your job with this condition.
 name: Dump runner context
if: always()
env:
  RUNNER_CONTEXT: ${{ toJson(runner) }}
run: echo "$RUNNER_CONTEXT"

-- You can use the key continue-on-error to continue only when it's have an error
- jobs:
  run-shell-comands:
    steps:
      
      ...
      #if step before has an error
      - name: multiline script
        continue-on-error: true
      ... 
      steps after
