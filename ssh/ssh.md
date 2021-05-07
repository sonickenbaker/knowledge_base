# SSH

## Local port forwarding
     -L [bind_address:]port:host:hostport
     -L [bind_address:]port:remote_socket
     -L local_socket:host:hostport
     -L local_socket:remote_socket


## Remote port forwarding
With a reverse tunnel an encrypted channel is established between the local host and remote host.
```
<remote_address_to_bind_to>:<remote_port> <-------> <local_address_to_bind_to>:<local_port>
```

```bash
ssh -f -i <path_to_ssh_key> -N -R <remote_address_to_bind_to>:<remote_port>:<local_address_to_bind_to>:<local_port> <user>@<remote_host> -p <remote_host_ssh_port>
```