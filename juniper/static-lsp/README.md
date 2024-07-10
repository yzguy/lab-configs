Static LSP
---------

SiteA

```
SiteA (198.51.100.1) -> 203.0.113.1 (203.0.113.1)      2024-07-10T02:13:57+0000
Keys:  Help   Display mode   Restart statistics   Order of fields   quit
                                       Packets               Pings
 Host                                Loss%   Snt   Last   Avg  Best  Wrst StDev
 1. 10.1.101.1                        0.0%     6    1.3   1.6   1.1   2.3   0.4
 2. 10.1.2.2                          0.0%     6    5.8   7.9   5.8  13.4   2.8
    [MPLS: Lbl 1001212 TC 0 S 1 TTL 1]
 3. 10.2.3.3                          0.0%     6    6.1   5.9   5.2   6.8   0.6
    [MPLS: Lbl 1002323 TC 0 S 1 TTL 1]
 4. 10.3.4.4                          0.0%     6    5.7   6.0   5.7   6.8   0.4
    [MPLS: Lbl 1003434 TC 0 S 1 TTL 1]
 5. 10.4.5.5                          0.0%     6    5.0   5.2   4.8   6.0   0.5
 6. 203.0.113.1                       0.0%     5    6.4   6.0   5.2   7.1   0.8
```

SiteB

```
SiteB (203.0.113.1) -> 198.51.100.1 (198.51.100.1)     2024-07-10T02:15:10+0000
Keys:  Help   Display mode   Restart statistics   Order of fields   quit
                                       Packets               Pings
 Host                                Loss%   Snt   Last   Avg  Best  Wrst StDev
 1. 10.1.102.1                        0.0%     5    1.2   1.9   1.2   2.8   0.7
 2. 10.4.5.4                          0.0%     5   10.2   6.8   5.3  10.2   2.0
    [MPLS: Lbl 1004545 TC 0 S 1 TTL 1]
 3. 10.3.4.3                          0.0%     5    7.3   6.2   5.5   7.3   0.8
    [MPLS: Lbl 1003434 TC 0 S 1 TTL 1]
 4. 10.2.3.2                          0.0%     5    6.0   5.5   4.8   6.0   0.5
    [MPLS: Lbl 1002323 TC 0 S 1 TTL 1]
 5. 10.1.2.1                          0.0%     4    5.6   6.0   4.1   7.2   1.5
 6. 198.51.100.1                      0.0%     4    4.9   5.4   4.9   6.5   0.7
```

R1

```
root@R1> show route 203.0.113.0/24

inet.0: 29 destinations, 29 routes (29 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both

203.0.113.0/24     *[BGP/170] 22:21:57, MED 0, localpref 100, from 192.168.1.5
                      AS path: 65002 I, validation-state: unverified
                    >  to 10.1.2.2 via ge-0/0/2.0, Push 1001212

root@R1> show route table inet.3

inet.3: 1 destinations, 1 routes (1 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both

192.168.1.5/32     *[MPLS/6/1] 23:12:40, metric 0
                    >  to 10.1.2.2 via ge-0/0/2.0, Push 1001212

root@R1> show route table mpls.0

mpls.0: 6 destinations, 6 routes (6 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both

0                  *[MPLS/0] 1d 00:04:23, metric 1
                       to table inet.0
0(S=0)             *[MPLS/0] 1d 00:04:23, metric 1
                       to table mpls.0
1                  *[MPLS/0] 1d 00:04:23, metric 1
                       Receive
2                  *[MPLS/0] 1d 00:04:23, metric 1
                       to table inet6.0
2(S=0)             *[MPLS/0] 1d 00:04:23, metric 1
                       to table mpls.0
13                 *[MPLS/0] 1d 00:04:23, metric 1
                       Receive
```

R2-R3

```
root@R2> show route 203.0.113.0/24

root@R2> show route table inet.3

root@R2> show route table mpls.0

mpls.0: 9 destinations, 9 routes (9 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both

0                  *[MPLS/0] 23:58:07, metric 1
                       to table inet.0
0(S=0)             *[MPLS/0] 23:58:07, metric 1
                       to table mpls.0
1                  *[MPLS/0] 23:58:07, metric 1
                       Receive
2                  *[MPLS/0] 23:58:07, metric 1
                       to table inet6.0
2(S=0)             *[MPLS/0] 23:58:07, metric 1
                       to table mpls.0
13                 *[MPLS/0] 23:58:07, metric 1
                       Receive
1001212            *[MPLS/6] 23:01:39, metric 1
                    >  to 10.2.3.3 via ge-0/0/3.0, Swap 1002323
1002323            *[MPLS/6] 22:49:41, metric 1
                    >  to 10.1.2.1 via ge-0/0/1.0, Pop
1002323(S=0)       *[MPLS/6] 22:49:41, metric 1
                    >  to 10.1.2.1 via ge-0/0/1.0, Pop
```

R4

```
root@R4> show route 203.0.113.0/24

root@R4> show route table inet.3

root@R4> show route table mpls.0

mpls.0: 9 destinations, 9 routes (9 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both

0                  *[MPLS/0] 23:50:12, metric 1
                       to table inet.0
0(S=0)             *[MPLS/0] 23:50:12, metric 1
                       to table mpls.0
1                  *[MPLS/0] 23:50:12, metric 1
                       Receive
2                  *[MPLS/0] 23:50:12, metric 1
                       to table inet6.0
2(S=0)             *[MPLS/0] 23:50:12, metric 1
                       to table mpls.0
13                 *[MPLS/0] 23:50:12, metric 1
                       Receive
1003434            *[MPLS/6] 22:56:35, metric 1
                    >  to 10.4.5.5 via ge-0/0/5.0, Pop
1003434(S=0)       *[MPLS/6] 22:56:35, metric 1
                    >  to 10.4.5.5 via ge-0/0/5.0, Pop
1004545            *[MPLS/6] 22:51:38, metric 1
                    >  to 10.3.4.3 via ge-0/0/3.0, Swap 1003434
```
