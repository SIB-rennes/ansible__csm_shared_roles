# Ansible role : docker_stack_deploy

This role allow to prepare a folder containing a docker stack and call `docker deploy` on it.

## ⚙️ Variables

Variables :

| Variable name             | Description                                                                 | Type    | Default value                                         |
| ------------------------- | --------------------------------------------------------------------------- | ------- | ----------------------------------------------------- |
| `stack_name`              | Name of the docker stack                                                    | string  | `foo`                                                 |
| `stack_src_folder`        | Source folder containing the stack                                          | string  | `foo`                                                 |
| `docker_compose_files`    | List of compose files to deploy                                             | list    | [`docker-compose.yml`, `docker-compose.override.yml`] |
| `stack_tmp_dest_folder`   | Target folder containing temporary files for the stack                      | string  | `/tmp/foo`                                            |
| `copy_stack_files_to`     | Target folder to keep files for the stack                                   | string  | ``                                                    |
| `docker_host`             | Docker host to use on deployment                                            | string  | `unix:///var/run/docker.sock`                         |
| `exclude_from_templating` | List of path to exclude from jinja templating (file will be copied instead) | list    | `[]`                                                  |
| `detach`                  | Run stack deployment in detached mode                                       | boolean | `true`                                                |

## Execution modes

### Controller mode (default)

The role runs on the Ansible controller (`hosts: localhost`). Source files are on the controller; `docker_host` points to a remote Docker Swarm endpoint.

```yaml
- name: Deploy a test
  hosts: localhost
  roles:
    - name: Deploy test
      role: csm.shared_roles.docker_stack_deploy
      stack_name: test
      stack_src_folder: templates/test
      stack_tmp_dest_folder: "./.tmp/test"
      copy_stack_files_to: "/tmp/my-deploy"
      docker_host: "tcp://swarm-manager:2376"
      exclude_from_templating:
        - config/file1.txt
      docker_compose_files:
        - docker-compose.yml
        - docker-compose.plus.yml
```

### Target node mode

The role runs on a remote host (the Docker Swarm manager itself). Source files are still read from the controller; the role templates them directly onto the target. `docker_host` defaults to the local socket.

```yaml
- name: Deploy a test
  hosts: {{ groups['managers'][0] }}
  roles:
    - name: Deploy test
      role: csm.shared_roles.docker_stack_deploy
      stack_name: test
      stack_src_folder: templates/test       # path on the controller
      stack_tmp_dest_folder: "/tmp/test"     # path on the target node
      exclude_from_templating:
        - config/file1.txt
      docker_compose_files:
        - docker-compose.yml
        - docker-compose.plus.yml
      # docker_host defaults to unix:///var/run/docker.sock (local socket on target)
```
