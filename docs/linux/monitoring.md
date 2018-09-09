# Simple monitoring with gkrellm

On the remote (to be monitored computer):

```bash
sudo sh -c 'apt-get update && apt-get install -y gkrellmd'
```

On your local machine, create an SSH tunnel to the remote machine:

```bash
ssh -N -f -L 19151:127.0.0.1:19150 user@remote_machine_ip
```

Open gkrellm on your machine listening to the local port that will receive information:

```
gkrellm -s 127.0.0.1 -P 19151
```

Help from https://totallynoob.com/server-monitoring-remote-monitor-a-server-with-gkrellm-via-ssh-tunnel/
