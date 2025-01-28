# README

This repo contains code and reproduction steps for a bug with ansible-compat 25.1.0 in VS Code using the Ansible plugin.

### Run playbook

To run the playbook, cd into the root of this project (same level as `.vscode`) and run `ansible-playbook --inventory ansible/inventory.ini ansible/test_playbook.yaml

### Reproduce `ansible-compat` 25.1.0 VS Code problem

1. Install VS Code 1.96.4
2. Open `.vscode/ansible-compat-lint-vscode-bug.code-workspace`
3. Install the workspace recommended Ansible plugin (version `v25.1.0`)
4. Open the `ansible/test_playbook.yaml` and an error message should popup
5. Install `ansible-compat` 24.10.0, follow steps 1-4, error message doesn't pop up and the file should find the following lint error:


```
yaml[trailing-spaces]: Trailing spaces
ansible/test_playbook.yaml:5

Read documentation for instructions on how to ignore specific rule violations.

# Rule Violation Summary

  1 yaml profile:basic tags:formatting,yaml

Failed: 1 failure(s), 0 warning(s) on 5 files. Last profile that met the validation criteria was 'min'.
```

**Additionally:** Running `ansible-lint` from the root of the VS Code project will find the above issue regardless of `ansible-compat` version

### Error generated in VS Code

```
Command failed: ansible-lint  --offline --nocolor -f codeclimate "~/src/ansible-compat-lint-vscode-bug/ansible/test_playbook.yaml"
Traceback (most recent call last):
  File "/usr/lib/python3.13/pathlib/_local.py", line 722, in mkdir
    os.mkdir(self, mode)
    ~~~~~~~~^^^^^^^^^^^^
FileNotFoundError: [Errno 2] No such file or directory: '/.ansible/roles'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/bin/ansible-lint", line 8, in <module>
    sys.exit(_run_cli_entrypoint())
             ~~~~~~~~~~~~~~~~~~~^^
  File "/usr/lib/python3.13/site-packages/ansiblelint/__main__.py", line 408, in _run_cli_entrypoint
    sys.exit(main(sys.argv))
             ~~~~^^^^^^^^^^
  File "/usr/lib/python3.13/site-packages/ansiblelint/__main__.py", line 285, in main
    cache_dir_lock = initialize_options(argv[1:])
  File "/usr/lib/python3.13/site-packages/ansiblelint/__main__.py", line 139, in initialize_options
    options.cache_dir = get_cache_dir(pathlib.Path(options.project_dir))
                        ~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.13/site-packages/ansible_compat/prerun.py", line 27, in get_cache_dir
    (cache_dir / name).mkdir(parents=True, exist_ok=True)
    ~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.13/pathlib/_local.py", line 726, in mkdir
    self.parent.mkdir(parents=True, exist_ok=True)
    ~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.13/pathlib/_local.py", line 722, in mkdir
    os.mkdir(self, mode)
    ~~~~~~~~^^^^^^^^^^^^
PermissionError: [Errno 13] Permission denied: '/.ansible'
```