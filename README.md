<p align="center">
  <img alt="gitleaks" src="https://raw.githubusercontent.com/zricethezav/gifs/master/gitleakslogo.png" height="70" />
</p>

Gitleaks Action provides a simple way to run gitleaks in your CI/CD pipeline.


### Sample Workflow
```
name: gitleaks

on: [push,pull_request]

jobs:
  gitleaks:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v1
    - name: gitleaks-action
      uses: zricethezav/gitleaks-action@master
```

### Sample Workflow with branch specified
```
name: gitleaks

on: [push,pull_request]

jobs:
  gitleaks:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v1
    - name: Set Job Vars
        id: set_vars
        run: |
          branch=${{ github.head_ref || github.ref }}
          branch_name="${branch#"refs/heads/"}"
          echo "::set-output name=branch_name::$branch_name"
    - name: gitleaks-action
      uses: zricethezav/gitleaks-action@master
      with:
        branch: ${{ steps.set_vars.outputs.branch_name }}
```

### Using your own .gitleaks.toml configuration
```
name: gitleaks

on: [push,pull_request]

jobs:
  gitleaks:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v1
    - name: gitleaks-action
      uses: zricethezav/gitleaks-action@master
      with:
        config-path: security/.gitleaks.toml
```
    > The `config-path` is relative to your GitHub Worskpace

### NOTE!!!
You must use `actions/checkout` before the gitleaks-action step. If you are using `actions/checkout@v2` you must specify a commit depth other than the default which is 1. 

ex: 
```
    steps:
    - uses: actions/checkout@v2
      with:
        fetch-depth: '0'
    - name: gitleaks-action
      uses: zricethezav/gitleaks-action@master
```

using a fetch-depth of '0' clones the entire history. If you want to do a more efficient clone, use '2', but that is not guaranteed to work with pull requests.   
