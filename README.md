# rb & rbe

`2*2` lines of Ruby to replace most of the command line tools that you use to process text inside of the terminal.

```rb
#rb
L = $stdin.readlines
puts L.instance_eval(ARGV.join(' '))

#rbe
L = $stdin.readlines
L.each{|l| puts l.instance_eval(ARGV.join(' '))}
```

## Setup

```
stow . -t ~/.bin
```

## Usage

```sh
rb '# some ruby to run on the Array `L` of lines passed on $stdin'

rbe '# some ruby to run each line of the Array `L` of lines `l` passed on $stdin'
```

## Examples

#### Extract docker images from running containers

```bash
> docker ps | rb drop 1 | rbe split[1]

# alpine
# postgres
```

#### Display how much time ago containers have exited

```shell
> docker ps -a | rb grep /Exited/ | rbe 'split.last.ljust(20) + " => " + split(/ {2,}/)[-2]'

# angry_hamilton      => Exited (0) 18 hours ago
# dreamy_lamport      => Exited (0) 3 days ago
# prickly_hypatia     => Exited (0) 2 weeks ago
```

#### Sort `df -h` output by `Use%`

```shell
> df -h | rb 'drop(1).sort_by { |l| l.split[-2].to_f }'

# udev                         3,9G     0  3,9G   0% /dev
# tmpfs                        3,9G     0  3,9G   0% /sys/fs/cgroup
# /dev/sda1                    511M  3,4M  508M   1% /boot/efi
# /dev/sda2                    237M   85M  140M  38% /boot

# or leave the header if you want
> df -h | rb '[L.first].concat L.drop(1).sort_by { |l| l.split[-2].to_f }'

# Filesystem                   Size  Used Avail Use% Mounted on
# udev                         3,9G     0  3,9G   0% /dev
# tmpfs                        3,9G     0  3,9G   0% /sys/fs/cgroup
# /dev/sda1                    511M  3,4M  508M   1% /boot/efi
# /dev/sda2                    237M   85M  140M  38% /boot
```
