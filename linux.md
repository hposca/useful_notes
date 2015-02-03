# Dicas Linux

[How do I list all cron jobs for all users?](http://stackoverflow.com/questions/134906/how-do-i-list-all-cron-jobs-for-all-users)

~~~ Bash
# for user in $(cut -f1 -d: /etc/passwd); do echo $user; crontab -u $user -l; done
~~~

[How to create a CPU spike with a bash command](http://stackoverflow.com/questions/2925606/how-to-create-a-cpu-spike-with-a-bash-command)

~~~ Bash
dd if=/dev/zero of=/dev/null
~~~

To run more of those to put load on more cores, try to fork it:

~~~ Bash
fulload() { dd if=/dev/zero of=/dev/null | dd if=/dev/zero of=/dev/null | dd if=/dev/zero of=/dev/null | dd if=/dev/zero of=/dev/null & }; fulload; read; killall dd
~~~

Repeat the command in the curly brackets as many times as the number of threads you want to produce (here 4 threads). Simple enter hit will stop it (just make sure no other dd is running on this user or you kill it too).

# Rsync

Dicas [aqui](http://www.electrictoolbox.com/rsync-ignore-existing-update-newer/)

## Apenas arquivos novos

~~~ Bash
rsync --update -raz --progress <origem> <destino>
~~~
