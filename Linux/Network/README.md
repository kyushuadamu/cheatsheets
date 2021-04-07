
# Linux Networking

- https://gist.github.com/tuxfight3r/9ac030cb0d707bb446c7
- https://www.tecmint.com/ss-command-examples-in-linux/
- Other: dig, host, ip

Commands
````powershell
ip a l                      # show ip config
ip addr OR ip addr show     # show ip config
ifconfig                    # show ip config
````

## Check open ports quick
````powershell
sudo lsof -nP -iTCP -sTCP:LISTEN
sudo ss -tulpn
sudo netstat -tulpn
sudo netstat -peanut
sudo netstat -peanut | grep ":80 "
````
## Change IP/MAC address
````powershell
ip link set dev eth0 down
macchanger -m 11:22:33:44:55:66 eth0
ip link set dev eth0 up
````
## Set Static IP
````powershell
ip addr add 10.10.0.2/24 dev eth0
````

### SS - investigate sockets
- [Examples of Linux ss command to monitor network connections](https://www.binarytides.com/linux-ss-command/)
````powershell
ss --help
man ss                      # Displays SS's help manual
sudo ss -lntup              # List TCP/UDP  with Pid's
sudo ss -u -a               # Display all UDP sockets
sudo ss -w -a               # Display all raw sockets
sudo ss -x -a               # Display all Unix sockets
sudo ss -4 state closing    # See closing sockets on IPv4
sudo ss -o state established '( dport = :smtp or sport = :smtp )'       # Display all established SMTP connections
sudo ss -o state established '( dport = :http or sport = :http )'       # Display all established HTTP connections
sudo ss dst 192.168.1.2                                                 # Show all ports connected from remote IP 192.168.1.2
sudo ss dst 192.168.1.10:http                                           # Find connections made by remote IP 192.168.1.10:http to our server
sudo ss -x src /tmp/.X11-unix/*                                         # Find all local processor connected to X Server
````

#### SS - filters
| Key/Command | Description |
| ----------- | ----------- |
| sudo ss -4 state FILTER-NAME-HERE | Filters TCP IPv4 |
| sudo ss -6 state FILTER-NAME-HERE | Filters TCP IPv6 |
| established | |
| syn-sent | |
| syn-recv | |
| fin-wait-1 | |
| fin-wait-2 | |
| time-wait | |
| closed | |
| close-wait | |
| last-ack | |
| listen | |
| closing | |
| all | All of the above states |
| connected | All the states except for listen and closed |
| synchronized | All the connected states except for syn-sent |
| bucket | Show states, which are maintained as minisockets, i.e. time-wait and syn-recv |
| big | Opposite to bucket state |

#### SS - filters
| Key/Command | Description |
| ----------- | ----------- |
| sudo ss  sport = :http |
| sudo ss  dport = :http |
| sudo ss  dport \> :1024 |
| sudo ss  sport \> :1024 |
| sudo ss sport \< :32000 |
| sudo ss  sport eq :22 |
| sudo ss  dport != :22 |
| sudo ss  state connected sport = :http |
| sudo ss \( sport = :http or sport = :https \) |
| sudo ss -o state fin-wait-1 \( sport = :http or sport = :https \) dst 192.168.1/24 |

#### NetStat
Commands
````powershell
sudo netstat -tulpn
sudo netstat -peanut
sudo netstat -peanut | grep ":8000 "
````

#### Nmap - Network Mapper
````powershell
nmap -v IP
nmap -v 192.168.1.1/24
nmap 192.168.1.1-254-p22,80 --open -oG - | awk '/22\/open.*80\/open/{print $2}'
nmap --open -p 22,80 192.168.1.1-254 -oG - | grep "/open" | awk '{ print $2 }'
nmap -Pn -oG -p22,80,443,445 - 100.100.100.100 | awk '/open/{ s = ""; for (i = 5; i <= NF-4; i++) s = s substr($i,1,length($i)-4) "\n"; print $2 " " $3 "\n" s}'
````

#### TCPDump
- [Tcpdump Examples](https://hackertarget.com/tcpdump-examples)
- [A tcpdump Tutorial with Examples — 50 Ways to Isolate Traffic](https://danielmiessler.com/study/tcpdump/)
````powershell
tcpdump -r capture_file
sudo tcpdump -i eth0 -nn -s0 -v port 80
sudo tcpdump -A -s0 port 80
sudo tcpdump -i eth0 udp
sudo tcpdump -i eth0 proto 17
udo tcpdump -i eth0 dst 10.10.1.20
sudo tcpdump -i eth0 host 10.10.1.1
sudo tcpdump -i eth0 -s0 -w test.pcap
sudo tcpdump -i eth0 -s0 -l port 80 | grep 'Server:'
````
Remember
````
and or &&
or or ||
not or !
````
Size
````powershell
tcpdump less 32
tcpdump greater 64
tcpdump <= 128
````
