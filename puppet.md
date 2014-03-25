When installing Puppet Enterprise 3.2.1, Postgresql may report error for limited shared memory.
It's a strange problem but solvable.

Go to you postgresql log in */var/log/pe-postgresql/pgstartup.log* and check whether there's something 
wrong with your *space*. If that, cat your /proc/sys/kernel/shmall, and double (or triple) the value.

$ sysctl -w kernel.shmall=VALUE


---------------------------------

Another problem with *GPG error*, solve with:
#  apt-key adv --keyserver keyserver.ubuntu.com --recv-keys YOUR_KEY

-------------------------

When configuring HOSTNAME in puppet, we MUST provide domain name. That is, *master* is not enough, 
*master.example.com* is correct.
