# Linux

## Network Monitoring

### Stats

[prometheus/node_exporter](https://github.com/prometheus/node_exporter) checks `/proc/net/snmp` and `/proc/net/netstat`. They have almost same information as `netstat -s`, but names are different. Their mapping is in the source code of [netstat -s](https://github.com/ecki/net-tools/blob/master/statistics.c).

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

### TCP Connection Status

https://www.ibm.com/support/knowledgecenter/en/SSLTBW_2.1.0/com.ibm.zos.v2r1.halu101/constatus.htm

- ESTABLISHED: Normal working sockets.
- TIME_WAIT: Almost closed but waiting to be actually closed. If this is many, it indicates that lots of connections are opened and closed. For example, it happens when HTTP Keep-Alive is not used.
  - https://serverfault.com/questions/10852/what-limits-the-maximum-number-of-connections-on-a-linux-server
