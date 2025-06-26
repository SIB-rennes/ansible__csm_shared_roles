# Ansible role : docker_stack_deploy

This role allow to prepare a folder containing a docker stack and call `docker deploy` on it.

## ⚙️ Variables

Variables :

| Variable name             | Description                                                                 | Type   | Default value                                         |
| ------------------------- | --------------------------------------------------------------------------- | ------ | ----------------------------------------------------- |
| `stack_name`              | Name of the docker stack                                                    | int    | `foo`                                                 |
| `stack_src_folder`        | Source folder containing the stack                                          | string | `foo`                                                 |
| `docker_compose_files`    | List of compose files to deploy                                             | list   | [`docker-compose.yml`, `docker-compose.override.yml`] |
| `stack_tmp_dest_folder`   | Target folder containing temporary files for the stack                      | string | `/tmp/foo`                                            |
| `docker_host`             | Docker host to use on deployment                                            | list   | `unix:///var/run/docker.sock`                         |
| `exclude_from_templating` | List of path to exclude from jinja templating (file will be copied instead) | list   | `[]`                                                  |

## Example

```ansible
- name: Deploy a test
  hosts: localhost # Most logic since we want a client to deploy on distant docker swarm
  roles:
    - name: Deploy test
      role: csm.shared_roles.docker_stack_deploy
      stack_name: test
      stack_src_folder: templates/test
      stack_tmp_dest_folder: "./.tmp/test"
      exclude_from_templating:
        - config/file1.txt  # This file will be copied as is - not templatized
      docker_compose_files:
        - docker-compose.yml
        - docker-compose.plus.yml
```