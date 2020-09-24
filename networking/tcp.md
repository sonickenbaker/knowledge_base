# TCP


## TCP finite state machine
http://www.tcpipguide.com/free/t_TCPOperationalOverviewandtheTCPFiniteStateMachineF-2.html

## Userland TCP
Source: https://arxiv.org/pdf/1901.01863.pdf
Transport protocols such as TCP can be implemented insidethe  operating  systems’  kernel  [66]  or  as  a  library  inside  theapplication.  The  main  motivation  of  kernel  stacks  is  that  asingle  stack  can  support  all  applications,  ensure  that  they  donot interfere and achieve high performance [21]. A drawbackof in-kernel implementations is that they are more difficult toextend  than  user  space  ones.  On  the  other  hand,  user  spaceimplementations  are  more  flexible,  but  they  are  often  lessmature than the in-kernel ones. Recent advances have enableduser space implementations to reach higher performance [42].