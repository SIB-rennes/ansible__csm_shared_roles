# Ansible role : docker_stack_deploy

This role allow to prepare a folder containing a docker stack and call `docker deploy` on it.

## ⚙️ Variables

Variables :

| Variable name           | Description                                            | Type   | Default value                 |
| ----------------------- | ------------------------------------------------------ | ------ | ----------------------------- |
| `stack_name`            | Name of the docker stack                               | int    | `foo`                         |
| `stack_src_folder`      | Source folder containing the stack                     | string | `foo`                         |
| `stack_tmp_dest_folder` | Target folder containing temporary files for the stack | string | `/tmp/foo`                    |
| `docker_host`           | Docker host to use on deployment                       | list   | `unix:///var/run/docker.sock` |


