# SSH

SSH in windows works withing gitconsole (the easiest way)

## Generate SSH
```
ssh-keygen -t rsa -C "<email>"
ssh-keygen -t rsa -b 4096 -C "<email>"
```
- default name `id_rsa` does not require file `~/config`
- custom requires file `~/config` with defined hosts:
```
Host bitbucket.org
 IdentityFile ~/.ssh/name_rsa
Host github.com
 IdentityFile ~/.ssh/name_rsa
```

## Manual Evaluating:
`ssh-keygen -t rsa -C "<email>"` - generate

`eval "ssh-agent.exe"` - run ssh agent

`ssh-add .ssh/<id_rsa>` - add key (if name is id_rsa, it's not needed to s`sh-add`)

`ssh -T git@bitbucket.org` - setup host ???

## Auto evaluating
Create file `~/.bashrc` which will be started every time with bash (git console in windows)
```
SSH_ENV=$HOME/.ssh/environment

# start the ssh-agent
function start_agent {
    echo "Initializing new SSH agent..."
    # spawn ssh-agent
    /usr/bin/ssh-agent | sed 's/^echo/#echo/' > "${SSH_ENV}"
    echo succeeded
    chmod 600 "${SSH_ENV}"
    . "${SSH_ENV}" > /dev/null
    /usr/bin/ssh-add
}
  
if [ -f "${SSH_ENV}" ]; then
     . "${SSH_ENV}" > /dev/null
   ps -ef | grep ${SSH_AGENT_PID} | grep ssh-agent$ > /dev/null || {
      start_agent;
  }
else
    start_agent;
fi
```
