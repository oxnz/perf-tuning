# Resource Limits

```
fs.file-max = 2097152
```

Above setting is specified for system-wide configuration. For user, there's a configure item in `/etc/security/limits.conf` named `nofile`, which means max number of open file descriptors.

`prlimit` and `ulimit` can be used to inspect the limit:

```
ulimit -<H|S>n
```
or

```
prlimit -n
```

## Resource Limits Tweak
**`ulimit`** provides control over resources available to each user via a shell. You can type `ulimit -a` to get a list of all current settings. In parentheses you will see one or two items: the units in measurements (e.g. kbytes, blocks, seconds) as well as a letter option (e.g. -s, -t, -u). The letter option will let you view/edit one particular setting at a time.

```
[root@dns will]# ulimit -a
core file size          (blocks, -c) 0
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited
pending signals                 (-i) 7823
max locked memory       (kbytes, -l) 64
max memory size         (kbytes, -m) unlimited
open files                      (-n) 1024
pipe size            (512 bytes, -p) 8
POSIX message queues     (bytes, -q) 819200
real-time priority              (-r) 0
stack size              (kbytes, -s) 8192
cpu time               (seconds, -t) unlimited
max user processes              (-u) 7823
virtual memory          (kbytes, -v) unlimited
file locks                      (-x) unlimited
```

* `ulimit -S -a` view all soft limits
* `ulimit -H -a` view all hard limits
* `ulimit -S [option] [number]` set a specific soft limit for one variable
e.g. ulimit -S -s 8192 set a new soft stacksize limit, “-s” is for stack
* `ulimit -H [option] [number]` set a specific hard limit for one variable
e.g. ulimit -H -s 8192 set a new hardstacksize limit, “-s” is for stack
* `/etc/security/limits.conf` file where you can set soft and hard limits per user or for everyone
e.g. Add in the following to <code>/etc/security/limits.conf</code> will set a soft stacksize of 8192 adn a hard stacksize of unlimited for username “toor”:

	```
	toor soft stack 8192
	toor hard stack unlimited
	```

**limit type**
>**hard**
>
for enforcing hard resource limits. These limits are set by the superuser and enforced by the Kernel. The user cannot raise his requirement of system resources above such values.
>
>**soft**
>
for enforcing soft resource limits. These limits are ones that the user can move up or down within the permitted range by any pre-existing hard limits. The values specified with this token can be thought of as default values, for normal system usage.
>
>from `limits.conf(5)`

## SEE ALSO
`getrlimit(2)`, `setrlimit(2)`
