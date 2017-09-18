
[How to generate and add keys to ssh-agent manually.](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)

[How to automate the above process.](https://help.github.com/articles/working-with-ssh-key-passphrases/#auto-launching-ssh-agent-on-msysgit)

add ~/.profile with the following:
```
env=~/.ssh/agent.env                                                               
                                                                                   
agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }                    
                                                                                   
agent_start () {                                                                   
            (umask 077; ssh-agent >| "$env")                                       
                . "$env" >| /dev/null ; }                                          
                                                                                   
agent_load_env                                                                     
                                                                                   
# agent_run_state: 0=agent running w/ key; 1=agent w/o key; 2= agent not running   
agent_run_state=$(ssh-add -l >| /dev/null 2>&1; echo $?)                           
                                                                                   
if [ ! "$SSH_AUTH_SOCK" ] || [ $agent_run_state = 2 ]; then                        
            agent_start                                                            
            ssh-add ~/.ssh/github_rsa                                              
elif [ "$SSH_AUTH_SOCK" ] && [ $agent_run_state = 1 ]; then                        
            ssh-add ~/.ssh/github_rsa                                              
fi                                                                                 
unset env  
```
