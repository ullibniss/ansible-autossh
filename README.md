Docker container with autossh inside, used for creating persistent ssh tunnels between hosts
# Source: https://github.com/jnovack/autossh

--------------

Basic variables for a role
* `autossh_name` - name for the service. Must be unique and contain nothing but latin letters and underscores (folders and container names will be created based on it)

* `autossh_mode` - "-L" for local forwarding, "-R" - for remote

* `autossh_deploy_dir` - folder where the keys and docker-compose of the service will be rendered. It is strongly not recommended to override. The default value is: "{{ DEPLOY_DIR }}/autossh/{{ autossh_name }}"

Next are the variables related to the `remote` machine.
The remote machine is the machine to which we connect via ssh and through which we pass the connection.
In the case of the `remote forwarding` mode, this must be the machine on which the port will be opened to listen for incoming connections.
In the case of the `local forwarding` mode, this can be any machine from which the target ip and port are available, where we will forward the connection from the local port.

**Important:** the remote machine must be listed in the authorized_keys file, which must be placed in the service's templates folder  
**Important:** the remote machine must be authorized using the private key id_rsa, which must be placed in the templates folder of the service

* `autossh_remote_user` - linux-username for ssh connection to remote machine
* `autossh_remote_host` - hostname of the machine to which we connect via ssh
* `autossh_remote_port` - port of the remote machine where ssh is located

Target is the target location of the tunnel.
In the case of `local forwarding` mode, these are the target host and ip address where we are forwarding the connection
In the case of the `remote forwarding` mode, this is the host and ip-address **on our machine** where we are forwarding the connection

* `autossh_target_port` - target port for the tunnel
* `autossh_target_host` - target host for the tunnel

* `autossh_tunnel_port` - On which port the tunnel will listen for connections
* `autossh_bind_ip` - Which ip addresses are allowed to accept connections from

* `autossh_docker_networks` is a list of docker network names that the container will connect to so that other containers can knock on the tunnel. If you do not define this variable, then the host network will be used for the tunnel. Be careful when using the host network, don't set bind_ip to 0.0.0.0 - this will open a security hole.
