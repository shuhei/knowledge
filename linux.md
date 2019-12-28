# Linux

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

### man

```sh
man man
```

- To specify a section - `man ${SECTION} ${NAME}` For example, `man 2 write` for the `write` system call
- To search man pages in their titles and short descriptions - `man -k ${KEYWORD}`
- To search man pages in their full contents - `man -K ${KEYWORD}`

#### Sections

1. Executable programs or shell commands
2. System calls (functions provided by the kernel)
3. Library calls (functions within program libraries)
4. Special files (usually found in /dev)
5. File formats and conventions eg /etc/passwd
6. Games
7. Miscellaneous (including macro packages and conventions), e.g. man(7), groff(7)
8. System administration commands (usually only for root)
9. Kernel routines [Non standard]

### du

- [Why does the “du” command count in 4KB steps?](https://unix.stackexchange.com/questions/34331/why-does-the-du-command-count-in-4kb-steps)
- Get summary of top-level files and directories including hidden ones: `du -hs $(ls -A)`
  - sort the result on Mac: `du -hs $(ls -A) | gsort -hr`
  - sort the result on Linux: `du -hs $(ls -A) | sort -hr`

### htop

- Sort: `P` for CPU, `M` for memory, `T` for time
- Toggle threads: `H` for user threads, `K` for kernel threads
- Tree view: `t`
- Filter processes by name: `\`
- Settings: `S`

### strace

`strace` traces system calls and signals. [It can cause severe performance slow-down](http://www.brendangregg.com/blog/2014-05-11/strace-wow-much-syscall.html).

- To trace a process - Use `-p ${PID}`.
- To write the output to a file - Use `-o`. Because the output is written to stderr, `>` doesn't work.
- Try `strace echo "hello"` and check each system call with `man`.
- File descriptors for standard streams - 0 for stdin, 1 for stdout, 2 for stderr

### kill

- Send `USR2` to process ID 1234: `kill -12 1234`
- List available signals: `kill -l`
- Signal name -> number: `kill -l USR2`
- Number -> signal name: `kill -l 12`
- To read more about signals: `man 7 signal`

### realpath

- Get an absolute path from a relative path: `realpath ./foo`
  - `readlink -f` also works

### xxd

- Make a hexdump of a file: `xxd ${file}`
- Make a hexdump of the first 100 bytes of a file: `xxd -l 100 ${file}`
- Print hex bytes as ASCII: `echo 696e6465782e6a73 | xxd -r -p`

### sudo

- [Use `PATH` of a user](https://unix.stackexchange.com/questions/83191/how-to-make-sudo-preserve-path): `sudo env "PATH=$PATH" ${some_command_here}`

### grep

- Escape `{}` of regular expression like `\{\}`

### tldr

```sh
# Install
npm i -g tldr

# Use
tldr ln
```

### curl

Wrap URL in a quote. Otherwise `&` in GET query parameters messes up your command.

```
curl -X PUT -H "application/json" -H "Authorization: Bearer $(some_command_to_get_token)" -v "https://foo.bar/baz?a=123&b=234" -d '{"ja":"nein"}'
```

### figlet

Display large ASCII-art characters.

```sh
echo 'Hello, World!' | figlet
```

## Debugging

### gdb

- Attach to a process: `sudo gdb -p ${pid}`
- Show stack traces: `bt`
- Show stack traces of all threads: `thread apply all bt`
- Show registers: `i registers`
- Examine an address: `x ${address}`
- Show an instruction at an address: `x/i ${address}`
- Show 10 instructions starting from an address: `x/10i ${address}`
- Show information about a symbol: `p ${symbol}`

https://cs.brown.edu/courses/cs033/docs/guides/gdb.pdf

### lldb

- Attach to a process: `lldb -p ${pid}`
- Show stack traces: `bt`
- Show stack traces of all threads: `bt all`
- Show registers: `reg read`
- Show an instruction at an address: `x/i ${address}`
- Show 10 instructions starting from an address: `x/10i ${address}`

## Proc Files

### `/proc/${pid}/schedstat`

1. time spent on the cpu
2. time spent waiting on a runqueue
3. `#` of timeslices run on this cpu

https://www.kernel.org/doc/Documentation/scheduler/sched-stats.txt

### `/proc/${pid}/status`

This has various process-level info and metrics.

Memory-related:

```
VmPeak: Peak virtual memory size
VmSize: Current virtual memory size
VmLck: ???
VmPin: ???
VmHWM: Peak (High Water Mark) RSS (Resident Set Size, the size of allocated main memory pages)
VmRSS: Current RSS
RssAnon: RSS of anonymous pages (mapped to nothing)
RssFile: RSS of pages mapped to files
RssShmem: RSS of pages mapped to shared memory
VmData: Size of data segment
VmStk: Size of stack segment
VmExe: Size of text segment
VmLib: Size of shared library code
VmPTE: Size of Page Table Entries
VmSwap: Size of swapped pages (paged-out anonymous pages)
```

### `/proc/${pid}/cmdline`

Commandline args separated and terminated by NULL

## Cookbook

### Replace string in multiple files

```sh
git grep --name-only ${from} | xargs perl -0777 -i -pe "s/${from}/${to}/g"

# Multiple line pattern
git grep --name-only ${from1} | xargs perl -0777 -i -pe "s/${from1}\n${from2}/${to}/gm"
```
