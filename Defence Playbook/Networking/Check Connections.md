1. **netstat**: This command displays network connections, routing tables, interface statistics, masquerade connections, and multicast memberships.

```bash
netstat -a
```

2. **ss**: This command is used to dump socket statistics. It allows showing information similar to netstat.

```bash
ss -tulwn
```

3. **lsof**: This command is used to list open files, but it can also be used to list network connections when used with the `-i` option.

```bash
sudo lsof -i
sudo lsof -i :53
```