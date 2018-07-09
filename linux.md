# Linux

## man

```sh
man man
```

1. Executable programs or shell commands
2. System calls (functions provided by the kernel)
3. Library calls (functions within program libraries)
4. Special files (usually found in /dev)
5. File formats and conventions eg /etc/passwd
6. Games
7. Miscellaneous (including macro packages and conventions), e.g. man(7), groff(7)
8. System administration commands (usually only for root)
9. Kernel routines [Non standard]

## TCP

[tcp(7)](http://man7.org/linux/man-pages/man7/tcp.7.html)

- To create an outgoing connection - connect(2)
- To receive incoming connections - bind(2) -> listen(2) -> accept(2)

### Server

There are two queues:

- SYN queue
- accept queue

The journey of a TCP connection before it is accepted by an application:

1. A SYN packet comes
    - If SYN queue is not full, it is queued in the SYN queue. The server sends a SYN/ACK packet and waits for ACK to come. This waiting has a timeout and retries.
    - If SYN queue is full, a SYN cookie is sent (TODO: when accept queue is full instead of SYN queue?) and the server forgets about the SYN packet. This is to mitigate SYN flooding. The client can still send ACK to it, but TCP options are ignored, etc.
2. An ACK packet comes
    - If accept queue is not full, it is queued in the accept queue and it waits for the application to `accept` it.
    - If accept queue is full, the server just ignores it. This is a half open state. The client thinks that the connection is open, but it's not on the server. The client retransmits ACK and the client application observes a delay.
3. An application `accept`s the connection

- http://veithen.github.io/2014/01/01/how-tcp-backlog-works-in-linux.html
- https://blog.cloudflare.com/syn-packet-handling-in-the-wild/
- https://eklitzke.org/how-tcp-sockets-work

## DNS Resolver

- DNS resolver is not provided by Linux Kernel.
- It's defined in POSIX and implemented by C Standard Libraries like glibc.
  - [getaddrinfo](https://en.wikibooks.org/wiki/C_Programming/POSIX_Reference/netdb.h/getaddrinfo)
- glibc implenataion
  - Controlled by `/etc/hosts`, `/etc/resolv.conf` and `/etc/nsswitch.conf`
  - No cache
- Alpine Linux uses musl libc, which has different DNS resolver implementation from glibc.
  - https://wiki.musl-libc.org/functional-differences-from-glibc.html
- Caching
  - Use dnsmasq, nscd, bind, etc. and put the IP address of it as `nameserver` in `/etc/resolv.conf`.
  - Ubuntu Network Manager uses dnsmasq.

## Network Monitoring

### Stats

[prometheus/node_exporter](https://github.com/prometheus/node_exporter) checks `/proc/net/netstat`. It has almost same information as `netstat -s`, but names are different. Their mapping is in the source code of [netstat -s](https://github.com/ecki/net-tools/blob/master/statistics.c).

According to *Systems Performance*, useful ones are:

```
Ip
	InReceives
	ForwDatagrams
Tcp
	ActiveOpens
	PassiveOpens
	InSegs
	OutSegs
	RetransSegs
TcpExt
	PruneCalled
	TW
	DelayedACKs
	DelayedACKLocked
	DelayedACKLost
	TCPPrequeued
	TCPDirectCopyFromBacklog
	TCPDirectCopyFromPrequeue
	TCPHPHits
	TCPHPAcks
	TCPSackRecovery
	TCPLossUndo
	TCPFastRetrans
	TCPRcvCollapsed
```

Part of them are in `/proc/net/snmp` but node-exporter doesn't have it.

According to [How TCP Sockets Work](https://eklitzke.org/how-tcp-sockets-work), it's important to monitor `ListenOverflows`.

### TCP Connection Status

https://www.ibm.com/support/knowledgecenter/en/SSLTBW_2.1.0/com.ibm.zos.v2r1.halu101/constatus.htm

- ESTABLISHED: Normal working sockets.
- TIME_WAIT: Almost closed but waiting to be actually closed. If this is many, it indicates that lots of connections are opened and closed. For example, it happens when HTTP Keep-Alive is not used.
  - https://serverfault.com/questions/10852/what-limits-the-maximum-number-of-connections-on-a-linux-server

## Commands

### htop

- Sort: `P` for CPU, `M` for memory, `T` for time
- Toggle threads: `H` for user threads, `K` for kernel threads
- Tree view: `t`
- Filter processes by name: `\`
- Settings: `S`

### strace

`strace` traces system calls and signals.

- Trace a process: Use `-p ${PID}`.
- Write the output to a file: Use `-o`. Because the output is written to stderr, `>` doesn't work.
- Try `strace echo "hello"` and check each system call with `man`.
- File descriptors for standard streams: 0 for stdin, 1 for stdout, 2 for stderr

## Metrics

### /proc/{pid}/schedstat

1. time spent on the cpu
2. time spent waiting on a runqueue
3. `#` of timeslices run on this cpu

https://www.kernel.org/doc/Documentation/scheduler/sched-stats.txt
