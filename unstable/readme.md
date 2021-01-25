Install with ACLs and latest helm chart

    helm install consul hashicorp/consul --set global.acls.manageSystemACLs=true --version 0.29.0

    k exec consul-consul-server-0 -- consul members
    Node                                     Address           Status  Type    Build  Protocol  DC   Segment
    consul-consul-server-0                   10.32.9.116:8301  alive   server  1.9.2  2         dc1  <all>
    consul-consul-server-1                   10.32.3.56:8301   alive   server  1.9.2  2         dc1  <all>
    consul-consul-server-2                   10.32.4.57:8301   alive   server  1.9.2  2         dc1  <all>
    gke-consul-1-default-pool-51946b93-9n7d  10.32.4.56:8301   alive   client  1.9.2  2         dc1  <default>
    gke-consul-1-default-pool-51946b93-cohw  10.32.9.113:8301  alive   client  1.9.2  2         dc1  <default>
    gke-consul-1-default-pool-51946b93-dous  10.32.3.55:8301   alive   client  1.9.2  2         dc1  <default>
    gke-consul-1-default-pool-51946b93-qcpt  10.32.7.30:8301   alive   client  1.9.2  2         dc1  <default>

    k exec consul-consul-server-0 -- consul members -detailed
    Node                                     Address           Status  Tags
    consul-consul-server-0                   10.32.9.116:8301  alive   acls=1,build=1.9.2:6530cf37,dc=dc1,expect=3,ft_fs=1,ft_si=1,id=1f444005-168e-6c57-4312-7817a9e595b8,port=8300,raft_vsn=3,role=consul,segment=<all>,vsn=2,vsn_max=3,vsn_min=2,wan_join_port=8302
    consul-consul-server-1                   10.32.3.56:8301   alive   acls=1,build=1.9.2:6530cf37,dc=dc1,expect=3,ft_fs=1,ft_si=1,id=c7a7e211-cee1-f11b-7487-4d83b6fb8430,port=8300,raft_vsn=3,role=consul,segment=<all>,vsn=2,vsn_max=3,vsn_min=2,wan_join_port=8302
    consul-consul-server-2                   10.32.4.57:8301   alive   acls=1,build=1.9.2:6530cf37,dc=dc1,expect=3,ft_fs=1,ft_si=1,id=121a7669-ac80-c43c-ae04-427c0cea9135,port=8300,raft_vsn=3,role=consul,segment=<all>,vsn=2,vsn_max=3,vsn_min=2,wan_join_port=8302
    gke-consul-1-default-pool-51946b93-9n7d  10.32.4.56:8301   alive   acls=1,build=1.9.2:6530cf37,dc=dc1,id=5d7577e4-c4c4-d735-7c48-a58092340dc2,role=node,segment=<default>,vsn=2,vsn_max=3,vsn_min=2
    gke-consul-1-default-pool-51946b93-cohw  10.32.9.113:8301  alive   acls=1,build=1.9.2:6530cf37,dc=dc1,id=a92bb49c-b064-9b13-32a5-84688cbf91c2,role=node,segment=<default>,vsn=2,vsn_max=3,vsn_min=2
    gke-consul-1-default-pool-51946b93-dous  10.32.3.55:8301   alive   acls=1,build=1.9.2:6530cf37,dc=dc1,id=ca5405b8-688c-44ea-48e0-8ca2a9316249,role=node,segment=<default>,vsn=2,vsn_max=3,vsn_min=2
    gke-consul-1-default-pool-51946b93-qcpt  10.32.7.30:8301   alive   acls=1,build=1.9.2:6530cf37,dc=dc1,id=6250d63a-432b-77cd-8649-6234dcc7de10,role=node,segment=<default>,vsn=2,vsn_max=3,vsn_min=2

## Force Delete A Client

    kubectl delete --force --grace-period=0 pod consul-consul-t6w7m

Marked as failed

    kubectl exec consul-consul-server-0 -- consul members
    Node                                     Address           Status  Type    Build  Protocol  DC   Segment
    consul-consul-server-0                   10.32.9.116:8301  alive   server  1.9.2  2         dc1  <all>
    consul-consul-server-1                   10.32.3.56:8301   alive   server  1.9.2  2         dc1  <all>
    consul-consul-server-2                   10.32.4.57:8301   alive   server  1.9.2  2         dc1  <all>
    gke-consul-1-default-pool-51946b93-9n7d  10.32.4.56:8301   alive   client  1.9.2  2         dc1  <default>
    gke-consul-1-default-pool-51946b93-cohw  10.32.9.113:8301  alive   client  1.9.2  2         dc1  <default>
    gke-consul-1-default-pool-51946b93-dous  10.32.3.55:8301   failed  client  1.9.2  2         dc1  <default>
    gke-consul-1-default-pool-51946b93-qcpt  10.32.7.30:8301   alive   client  1.9.2  2         dc1  <default>

New client pod complains about joining:

    2021-01-23T23:18:04.134Z [ERROR] agent.client: RPC failed to server: method=Catalog.Register server=10.32.3.56:8300 error="rpc error making call: rpc error making call: failed inserting node: Error while renaming Node ID: "2bf40850-3088-eb8c-fa61-8541d44e4aaa": Node name gke-consul-1-default-pool-51946b93-dous is reserved by node ca5405b8-688c-44ea-48e0-8ca2a9316249 with name gke-consul-1-default-pool-51946b93-dous (10.32.3.55)"
    2021-01-23T23:18:04.135Z [WARN]  agent: Syncing node info failed.: error="rpc error making call: rpc error making call: failed inserting node: Error while renaming Node ID: "2bf40850-3088-eb8c-fa61-8541d44e4aaa": Node name gke-consul-1-default-pool-51946b93-dous is reserved by node ca5405b8-688c-44ea-48e0-8ca2a9316249 with name gke-consul-1-default-pool-51946b93-dous (10.32.3.55)"
    2021-01-23T23:18:04.135Z [ERROR] agent.anti_entropy: failed to sync remote state: error="rpc error making call: rpc error making call: failed inserting node: Error while renaming Node ID: "2bf40850-3088-eb8c-fa61-8541d44e4aaa": Node name gke-consul-1-default-pool-51946b93-dous is reserved by node ca5405b8-688c-44ea-48e0-8ca2a9316249 with name gke-consul-1-default-pool-51946b93-dous (10.32.3.55)"
    2021-01-23T23:18:06.616Z [INFO]  agent: Synced node info


Server complains about conflicting address node ID renaming, eventually marks old node as failed, accepts the join. Takes about 60s.

    2021-01-23T23:18:03.741Z [ERROR] agent.server.memberlist.lan: memberlist: Conflicting address for gke-consul-1-default-pool-51946b93-dous. Mine: 10.32.3.55:8301 Theirs: 10.32.3.57:8301 Old state: 1
    2021-01-23T23:18:03.741Z [WARN]  agent.server.serf.lan: serf: Name conflict for 'gke-consul-1-default-pool-51946b93-dous' both 10.32.3.55:8301 and 10.32.3.57:8301 are claiming
    2021-01-23T23:18:03.838Z [ERROR] agent.server.memberlist.lan: memberlist: Conflicting address for gke-consul-1-default-pool-51946b93-dous. Mine: 10.32.3.55:8301 Theirs: 10.32.3.57:8301 Old state: 1
    2021-01-23T23:18:03.839Z [WARN]  agent.server.serf.lan: serf: Name conflict for 'gke-consul-1-default-pool-51946b93-dous' both 10.32.3.55:8301 and 10.32.3.57:8301 are claiming
    2021-01-23T23:18:04.037Z [ERROR] agent.server.memberlist.lan: memberlist: Conflicting address for gke-consul-1-default-pool-51946b93-dous. Mine: 10.32.3.55:8301 Theirs: 10.32.3.57:8301 Old state: 1
    2021-01-23T23:18:04.037Z [WARN]  agent.server.serf.lan: serf: Name conflict for 'gke-consul-1-default-pool-51946b93-dous' both 10.32.3.55:8301 and 10.32.3.57:8301 are claiming
    2021-01-23T23:18:04.109Z [WARN]  agent.fsm: EnsureRegistration failed: error="failed inserting node: Error while renaming Node ID: "2bf40850-3088-eb8c-fa61-8541d44e4aaa": Node name gke-consul-1-default-pool-51946b93-dous is reserved by node ca5405b8-688c-44ea-48e0-8ca2a9316249 with name gke-consul-1-default-pool-51946b93-dous (10.32.3.55)"
    2021-01-23T23:18:05.688Z [INFO]  agent.server.serf.lan: serf: EventMemberFailed: gke-consul-1-default-pool-51946b93-dous 10.32.3.55
    2021-01-23T23:18:05.688Z [INFO]  agent.server: member failed, marking health critical: member=gke-consul-1-default-pool-51946b93-dous
    2021-01-23T23:18:05.823Z [INFO]  agent.server.memberlist.lan: memberlist: Suspect gke-consul-1-default-pool-51946b93-dous has failed, no acks received
    2021-01-23T23:18:15.902Z [INFO]  agent.server.serf.lan: serf: attempting reconnect to gke-consul-1-default-pool-51946b93-dous 10.32.3.55:8301
    2021-01-23T23:18:54.216Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-dous 10.32.3.57
    2021-01-23T23:18:54.216Z [INFO]  agent.server: member joined, marking health alive: member=gke-consul-1-default-pool-51946b93-dous

Marked as alive

    kubectl exec consul-consul-server-0 -- consul members
    Node                                     Address           Status  Type    Build  Protocol  DC   Segment
    consul-consul-server-0                   10.32.9.116:8301  alive   server  1.9.2  2         dc1  <all>
    consul-consul-server-1                   10.32.3.56:8301   alive   server  1.9.2  2         dc1  <all>
    consul-consul-server-2                   10.32.4.57:8301   alive   server  1.9.2  2         dc1  <all>
    gke-consul-1-default-pool-51946b93-9n7d  10.32.4.56:8301   alive   client  1.9.2  2         dc1  <default>
    gke-consul-1-default-pool-51946b93-cohw  10.32.9.113:8301  alive   client  1.9.2  2         dc1  <default>
    gke-consul-1-default-pool-51946b93-dous  10.32.3.57:8301   alive   client  1.9.2  2         dc1  <default>
    gke-consul-1-default-pool-51946b93-qcpt  10.32.7.30:8301   alive   client  1.9.2  2         dc1  <default>

## Graceful Delete A Client

    k delete po consul-consul-h96h4

Marked as left

    Node                                     Address           Status  Type    Build  Protocol  DC   Segment
    consul-consul-server-0                   10.32.9.116:8301  alive   server  1.9.2  2         dc1  <all>
    consul-consul-server-1                   10.32.3.56:8301   alive   server  1.9.2  2         dc1  <all>
    consul-consul-server-2                   10.32.4.57:8301   alive   server  1.9.2  2         dc1  <all>
    gke-consul-1-default-pool-51946b93-9n7d  10.32.4.56:8301   alive   client  1.9.2  2         dc1  <default>
    gke-consul-1-default-pool-51946b93-cohw  10.32.9.113:8301  alive   client  1.9.2  2         dc1  <default>
    gke-consul-1-default-pool-51946b93-dous  10.32.3.57:8301   left    client  1.9.2  2         dc1  <default>
    gke-consul-1-default-pool-51946b93-qcpt  10.32.7.30:8301   alive   client  1.9.2  2         dc1  <default>

New pod comes up fine

    ==> Consul agent running!
    2021-01-23T23:24:11.940Z [WARN]  agent.router.manager: No servers available
    2021-01-23T23:24:11.940Z [ERROR] agent.anti_entropy: failed to sync remote state: error="No known Consul servers"
    2021-01-23T23:24:12.042Z [INFO]  agent.client.serf.lan: serf: EventMemberJoin: consul-consul-server-0 10.32.9.116
    2021-01-23T23:24:12.042Z [INFO]  agent.client.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-qcpt 10.32.7.30
    2021-01-23T23:24:12.042Z [WARN]  agent.client.memberlist.lan: memberlist: Refuting a dead message (from: gke-consul-1-default-pool-51946b93-dous)
    2021-01-23T23:24:12.042Z [INFO]  agent.client.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-9n7d 10.32.4.56
    2021-01-23T23:24:12.042Z [INFO]  agent.client.serf.lan: serf: EventMemberJoin: consul-consul-server-2 10.32.4.57
    2021-01-23T23:24:12.042Z [INFO]  agent.client.serf.lan: serf: EventMemberJoin: consul-consul-server-1 10.32.3.56
    2021-01-23T23:24:12.042Z [INFO]  agent.client.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-cohw 10.32.9.113
    2021-01-23T23:24:12.042Z [INFO]  agent.client: adding server: server="consul-consul-server-0 (Addr: tcp/10.32.9.116:8300) (DC: dc1)"
    2021-01-23T23:24:12.042Z [INFO]  agent.client: adding server: server="consul-consul-server-2 (Addr: tcp/10.32.4.57:8300) (DC: dc1)"
    2021-01-23T23:24:12.043Z [INFO]  agent.client: adding server: server="consul-consul-server-1 (Addr: tcp/10.32.3.56:8300) (DC: dc1)"
    2021-01-23T23:24:12.061Z [INFO]  agent: (LAN) joined: number_of_nodes=3
    2021-01-23T23:24:12.061Z [INFO]  agent: Join cluster completed. Synced with initial agents: cluster=LAN num_agents=3
    2021-01-23T23:24:12.089Z [INFO]  agent.client.serf.lan: serf: EventMemberUpdate: gke-consul-1-default-pool-51946b93-dous
    2021-01-23T23:24:14.671Z [INFO]  agent: Synced node info

Server logs

    2021-01-23T23:23:51.103Z [INFO]  agent.server.serf.lan: serf: EventMemberLeave: gke-consul-1-default-pool-51946b93-dous 10.32.3.57
    2021-01-23T23:23:51.107Z [INFO]  agent.server: deregistering member: member=gke-consul-1-default-pool-51946b93-dous reason=left
    2021-01-23T23:24:12.041Z [INFO]  agent.server.memberlist.lan: memberlist: Updating address for left or failed node gke-consul-1-default-pool-51946b93-dous from 10.32.3.57:8301 to 10.32.3.58:8301
    2021-01-23T23:24:12.041Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-dous 10.32.3.58
    2021-01-23T23:24:12.041Z [INFO]  agent.server: member joined, marking health alive: member=gke-consul-1-default-pool-51946b93-dous
    2021-01-23T23:24:12.215Z [INFO]  agent.server.serf.lan: serf: EventMemberUpdate: gke-consul-1-default-pool-51946b93-dous

Other client logs

    2021-01-23T23:23:51.089Z [INFO]  agent.client.serf.lan: serf: EventMemberLeave: gke-consul-1-default-pool-51946b93-dous 10.32.3.57
    2021-01-23T23:24:12.054Z [INFO]  agent.client.memberlist.lan: memberlist: Updating address for left or failed node gke-consul-1-default-pool-51946b93-dous from 10.32.3.57:8301 to 10.32.3.58:8301
    2021-01-23T23:24:12.054Z [INFO]  agent.client.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-dous 10.32.3.58
    2021-01-23T23:24:12.139Z [INFO]  agent.client.serf.lan: serf: EventMemberUpdate: gke-consul-1-default-pool-51946b93-dous

## Gracefully Delete Server

Server-1 logs

    2021-01-23T23:26:56.835Z [ERROR] agent: Coordinate update error: error="rpc error making call: stream closed"
    2021-01-23T23:27:02.107Z [INFO]  agent.server.serf.lan: serf: EventMemberFailed: consul-consul-server-0 10.32.9.116
    2021-01-23T23:27:02.108Z [INFO]  agent.server: Removing LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.116:8300) (DC: dc1)"
    2021-01-23T23:27:05.280Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.4.57:8300 leader=10.32.9.116:8300
    2021-01-23T23:27:05.653Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-23T23:27:06.140Z [WARN]  agent.server: Raft has a leader but other tracking of the node would indicate that the node is unhealthy or does not exist. The network may be misconfigured.: leader=10.32.9.116:8300
    2021-01-23T23:27:06.145Z [WARN]  agent.server: Raft has a leader but other tracking of the node would indicate that the node is unhealthy or does not exist. The network may be misconfigured.: leader=10.32.9.116:8300
    2021-01-23T23:27:06.460Z [WARN]  agent.server.raft: heartbeat timeout reached, starting election: last-leader=10.32.9.116:8300
    2021-01-23T23:27:06.460Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.3.56:8300 [Candidate]" term=3
    2021-01-23T23:27:06.465Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=1f444005-168e-6c57-4312-7817a9e595b8 fallback=10.32.9.116:8300 error="Could not find address for server id 1f444005-168e-6c57-4312-7817a9e595b8"
    2021-01-23T23:27:10.393Z [INFO]  agent.server.raft: entering follower state: follower="Node at 10.32.3.56:8300 [Follower]" leader=
    2021-01-23T23:27:10.615Z [INFO]  agent.server: New leader elected: payload=consul-consul-server-2
    2021-01-23T23:27:15.653Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-23T23:27:16.465Z [ERROR] agent.server.raft: failed to make requestVote RPC: target="{Voter 1f444005-168e-6c57-4312-7817a9e595b8 10.32.9.116:8300}" error="dial tcp <nil>->10.32.9.116:8300: i/o timeout"
    2021-01-23T23:27:19.235Z [ERROR] agent.server.memberlist.lan: memberlist: Conflicting address for consul-consul-server-0. Mine: 10.32.9.116:8301 Theirs: 10.32.9.117:8301 Old state: 2
    2021-01-23T23:27:19.235Z [WARN]  agent.server.serf.lan: serf: Name conflict for 'consul-consul-server-0' both 10.32.9.116:8301 and 10.32.9.117:8301 are claiming
    2021-01-23T23:27:19.503Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.116:8302 Theirs: 10.32.9.117:8302 Old state: 1
    2021-01-23T23:27:19.504Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.116:8302 and 10.32.9.117:8302 are claiming
    2021-01-23T23:27:19.504Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.116:8302 Theirs: 10.32.9.117:8302 Old state: 1
    2021-01-23T23:27:19.504Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.116:8302 and 10.32.9.117:8302 are claiming
    2021-01-23T23:27:20.003Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.116:8302 Theirs: 10.32.9.117:8302 Old state: 1
    2021-01-23T23:27:20.003Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.116:8302 and 10.32.9.117:8302 are claiming
    2021-01-23T23:27:20.003Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.116:8302 Theirs: 10.32.9.117:8302 Old state: 1
    2021-01-23T23:27:20.003Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.116:8302 and 10.32.9.117:8302 are claiming
    2021-01-23T23:27:22.416Z [INFO]  agent.server.serf.lan: serf: EventMemberLeave (forced): consul-consul-server-0 10.32.9.116
    2021-01-23T23:27:22.416Z [INFO]  agent.server: Removing LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.116:8300) (DC: dc1)"
    2021-01-23T23:27:25.653Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-23T23:27:27.473Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.117:8300 leader=10.32.4.57:8300
    2021-01-23T23:27:35.653Z [INFO]  agent.server.memberlist.wan: memberlist: Marking consul-consul-server-0.dc1 as failed, suspect timeout reached (0 peer confirmations)
    2021-01-23T23:27:35.653Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-23T23:27:35.653Z [INFO]  agent.server.serf.wan: serf: EventMemberLeave: consul-consul-server-0.dc1 10.32.9.116
    2021-01-23T23:27:35.653Z [INFO]  agent.server: Handled event for server in area: event=member-leave server=consul-consul-server-0.dc1 area=wan
    2021-01-23T23:27:36.088Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.117:8300 leader=10.32.4.57:8300
    2021-01-23T23:27:43.788Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.117:8300 leader=10.32.4.57:8300
    2021-01-23T23:27:50.557Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.117:8300 leader=10.32.4.57:8300
    2021-01-23T23:27:52.616Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: consul-consul-server-0 10.32.9.117
    2021-01-23T23:27:52.616Z [INFO]  agent.server: Adding LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.117:8300) (DC: dc1)"
    2021-01-23T23:27:58.234Z [INFO]  agent.server: New leader elected: payload=consul-consul-server-2
    2021-01-23T23:27:59.170Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.117:8300 leader=10.32.4.57:8300
    2021-01-23T23:28:06.234Z [INFO]  agent.server: New leader elected: payload=consul-consul-server-2
    2021-01-23T23:28:10.404Z [INFO]  agent.server.serf.lan: serf: EventMemberUpdate: consul-consul-server-0
    2021-01-23T23:28:10.404Z [INFO]  agent.server: Updating LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.117:8300) (DC: dc1)"
    2021-01-23T23:28:10.972Z [INFO]  agent.server.memberlist.wan: memberlist: Updating address for left or failed node consul-consul-server-0.dc1 from 10.32.9.116:8302 to 10.32.9.117:8302
    2021-01-23T23:28:10.973Z [INFO]  agent.server.serf.wan: serf: EventMemberJoin: consul-consul-server-0.dc1 10.32.9.117
    2021-01-23T23:28:10.973Z [INFO]  agent.server: Handled event for server in area: event=member-join server=consul-consul-server-0.dc1 area=wan

Server-2 logs

    2021-01-23T23:27:02.107Z [INFO]  agent.server.serf.lan: serf: EventMemberFailed: consul-consul-server-0 10.32.9.116
    2021-01-23T23:27:02.107Z [INFO]  agent.server: Removing LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.116:8300) (DC: dc1)"
    2021-01-23T23:27:05.271Z [WARN]  agent.server.raft: heartbeat timeout reached, starting election: last-leader=10.32.9.116:8300
    2021-01-23T23:27:05.271Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.4.57:8300 [Candidate]" term=3
    2021-01-23T23:27:05.278Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=1f444005-168e-6c57-4312-7817a9e595b8 fallback=10.32.9.116:8300 error="Could not find address for server id 1f444005-168e-6c57-4312-7817a9e595b8"
    2021-01-23T23:27:06.470Z [INFO]  agent.server.raft: duplicate requestVote for same term: term=3
    2021-01-23T23:27:09.014Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-23T23:27:10.381Z [WARN]  agent.server.raft: Election timeout reached, restarting election
    2021-01-23T23:27:10.381Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.4.57:8300 [Candidate]" term=4
    2021-01-23T23:27:10.387Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=1f444005-168e-6c57-4312-7817a9e595b8 fallback=10.32.9.116:8300 error="Could not find address for server id 1f444005-168e-6c57-4312-7817a9e595b8"
    2021-01-23T23:27:10.393Z [INFO]  agent.server.raft: election won: tally=2
    2021-01-23T23:27:10.393Z [INFO]  agent.server.raft: entering leader state: leader="Node at 10.32.4.57:8300 [Leader]"
    2021-01-23T23:27:10.393Z [INFO]  agent.server.raft: added peer, starting replication: peer=1f444005-168e-6c57-4312-7817a9e595b8
    2021-01-23T23:27:10.393Z [INFO]  agent.server.raft: added peer, starting replication: peer=c7a7e211-cee1-f11b-7487-4d83b6fb8430
    2021-01-23T23:27:10.393Z [INFO]  agent.server: cluster leadership acquired
    2021-01-23T23:27:10.394Z [INFO]  agent.server: New leader elected: payload=consul-consul-server-2
    2021-01-23T23:27:10.394Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=1f444005-168e-6c57-4312-7817a9e595b8 fallback=10.32.9.116:8300 error="Could not find address for server id 1f444005-168e-6c57-4312-7817a9e595b8"
    2021-01-23T23:27:10.395Z [INFO]  agent.server.raft: pipelining replication: peer="{Voter c7a7e211-cee1-f11b-7487-4d83b6fb8430 10.32.3.56:8300}"
    2021-01-23T23:27:10.402Z [INFO]  agent.server: initializing acls
    2021-01-23T23:27:10.402Z [INFO]  agent.leader: started routine: routine="legacy ACL token upgrade"
    2021-01-23T23:27:10.402Z [INFO]  agent.leader: started routine: routine="acl token reaping"
    2021-01-23T23:27:10.403Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="rpc error making call: stream closed"
    2021-01-23T23:27:11.128Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=1f444005-168e-6c57-4312-7817a9e595b8 fallback=10.32.9.116:8300 error="Could not find address for server id 1f444005-168e-6c57-4312-7817a9e595b8"
    2021-01-23T23:27:11.402Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="context deadline exceeded"
    2021-01-23T23:27:11.402Z [ERROR] agent.server.autopilot: Error when computing next state: error="context deadline exceeded"
    2021-01-23T23:27:11.402Z [INFO]  agent.leader: started routine: routine="federation state anti-entropy"
    2021-01-23T23:27:11.402Z [INFO]  agent.leader: started routine: routine="federation state pruning"
    2021-01-23T23:27:11.404Z [INFO]  agent.leader: started routine: routine="intermediate cert renew watch"
    2021-01-23T23:27:11.404Z [INFO]  agent.leader: started routine: routine="CA root pruning"
    2021-01-23T23:27:11.405Z [INFO]  agent.server: member failed, marking health critical: member=consul-consul-server-0
    2021-01-23T23:27:12.896Z [WARN]  agent.server.raft: failed to contact: server-id=1f444005-168e-6c57-4312-7817a9e595b8 time=2.502525064s
    2021-01-23T23:27:14.403Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="context deadline exceeded"
    2021-01-23T23:27:15.278Z [ERROR] agent.server.raft: failed to make requestVote RPC: target="{Voter 1f444005-168e-6c57-4312-7817a9e595b8 10.32.9.116:8300}" error="dial tcp <nil>->10.32.9.116:8300: i/o timeout"
    2021-01-23T23:27:15.384Z [WARN]  agent.server.raft: failed to contact: server-id=1f444005-168e-6c57-4312-7817a9e595b8 time=4.990154022s
    2021-01-23T23:27:16.403Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="context deadline exceeded"
    2021-01-23T23:27:17.855Z [WARN]  agent.server.raft: failed to contact: server-id=1f444005-168e-6c57-4312-7817a9e595b8 time=7.461303371s
    2021-01-23T23:27:18.403Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="context deadline exceeded"
    2021-01-23T23:27:19.003Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.116:8302 Theirs: 10.32.9.117:8302 Old state: 1
    2021-01-23T23:27:19.003Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.116:8302 and 10.32.9.117:8302 are claiming
    2021-01-23T23:27:19.007Z [ERROR] agent.server.memberlist.lan: memberlist: Conflicting address for consul-consul-server-0. Mine: 10.32.9.116:8301 Theirs: 10.32.9.117:8301 Old state: 2
    2021-01-23T23:27:19.007Z [WARN]  agent.server.serf.lan: serf: Name conflict for 'consul-consul-server-0' both 10.32.9.116:8301 and 10.32.9.117:8301 are claiming
    2021-01-23T23:27:19.015Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-23T23:27:19.244Z [ERROR] agent.server.memberlist.lan: memberlist: Conflicting address for consul-consul-server-0. Mine: 10.32.9.116:8301 Theirs: 10.32.9.117:8301 Old state: 2
    2021-01-23T23:27:19.244Z [WARN]  agent.server.serf.lan: serf: Name conflict for 'consul-consul-server-0' both 10.32.9.116:8301 and 10.32.9.117:8301 are claiming
    2021-01-23T23:27:19.504Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.116:8302 Theirs: 10.32.9.117:8302 Old state: 1
    2021-01-23T23:27:19.504Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.116:8302 and 10.32.9.117:8302 are claiming
    2021-01-23T23:27:19.504Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.116:8302 Theirs: 10.32.9.117:8302 Old state: 1
    2021-01-23T23:27:19.505Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.116:8302 and 10.32.9.117:8302 are claiming
    2021-01-23T23:27:20.003Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.116:8302 Theirs: 10.32.9.117:8302 Old state: 1
    2021-01-23T23:27:20.004Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.116:8302 and 10.32.9.117:8302 are claiming
    2021-01-23T23:27:20.004Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.116:8302 Theirs: 10.32.9.117:8302 Old state: 1
    2021-01-23T23:27:20.004Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.116:8302 and 10.32.9.117:8302 are claiming
    2021-01-23T23:27:20.388Z [ERROR] agent.server.raft: failed to make requestVote RPC: target="{Voter 1f444005-168e-6c57-4312-7817a9e595b8 10.32.9.116:8300}" error="dial tcp <nil>->10.32.9.116:8300: i/o timeout"
    2021-01-23T23:27:20.394Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Voter 1f444005-168e-6c57-4312-7817a9e595b8 10.32.9.116:8300}" error="dial tcp <nil>->10.32.9.116:8300: i/o timeout"
    2021-01-23T23:27:20.403Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="context deadline exceeded"
    2021-01-23T23:27:20.404Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=1f444005-168e-6c57-4312-7817a9e595b8 fallback=10.32.9.116:8300 error="Could not find address for server id 1f444005-168e-6c57-4312-7817a9e595b8"
    2021-01-23T23:27:21.128Z [ERROR] agent.server.raft: failed to heartbeat to: peer=10.32.9.116:8300 error="dial tcp <nil>->10.32.9.116:8300: i/o timeout"
    2021-01-23T23:27:21.138Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=1f444005-168e-6c57-4312-7817a9e595b8 fallback=10.32.9.116:8300 error="Could not find address for server id 1f444005-168e-6c57-4312-7817a9e595b8"
    2021-01-23T23:27:22.403Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="context deadline exceeded"
    2021-01-23T23:27:22.403Z [INFO]  agent.server.autopilot: Attempting removal of failed server node: id=1f444005-168e-6c57-4312-7817a9e595b8 name=consul-consul-server-0 address=10.32.9.116:8300
    2021-01-23T23:27:22.403Z [INFO]  agent.server.serf.lan: serf: EventMemberLeave (forced): consul-consul-server-0 10.32.9.116
    2021-01-23T23:27:22.403Z [INFO]  agent.server: Removing LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.116:8300) (DC: dc1)"
    2021-01-23T23:27:22.403Z [INFO]  agent.server.raft: updating configuration: command=RemoveServer server-id=1f444005-168e-6c57-4312-7817a9e595b8 server-addr= servers="[{Suffrage:Voter ID:121a7669-ac80-c43c-ae04-427c0cea9135 Address:10.32.4.57:8300} {Suffrage:Voter ID:c7a7e211-cee1-f11b-7487-4d83b6fb8430 Address:10.32.3.56:8300}]"
    2021-01-23T23:27:22.408Z [INFO]  agent.server.raft: removed peer, stopping replication: peer=1f444005-168e-6c57-4312-7817a9e595b8 last-index=172
    2021-01-23T23:27:22.413Z [INFO]  agent.server.autopilot: removed server: id=1f444005-168e-6c57-4312-7817a9e595b8
    2021-01-23T23:27:22.413Z [INFO]  agent.server: deregistering member: member=consul-consul-server-0 reason=left
    2021-01-23T23:27:22.512Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Voter 1f444005-168e-6c57-4312-7817a9e595b8 10.32.9.116:8300}" error="dial tcp <nil>->10.32.9.116:8300: connect: no route to host"
    2021-01-23T23:27:22.512Z [ERROR] agent.server.raft: failed to heartbeat to: peer=10.32.9.116:8300 error="dial tcp <nil>->10.32.9.116:8300: connect: no route to host"
    2021-01-23T23:27:22.512Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="rpc error getting client: failed to get conn: dial tcp <nil>->10.32.9.116:8300: connect: no route to host"
    2021-01-23T23:27:22.512Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="rpc error getting client: failed to get conn: rpc error: lead thread didn't get connection"
    2021-01-23T23:27:22.512Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="rpc error getting client: failed to get conn: rpc error: lead thread didn't get connection"
    2021-01-23T23:27:22.512Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="rpc error getting client: failed to get conn: rpc error: lead thread didn't get connection"
    2021-01-23T23:27:22.512Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="rpc error getting client: failed to get conn: rpc error: lead thread didn't get connection"
    2021-01-23T23:27:22.523Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=1f444005-168e-6c57-4312-7817a9e595b8 fallback=10.32.9.116:8300 error="Could not find address for server id 1f444005-168e-6c57-4312-7817a9e595b8"
    2021-01-23T23:27:23.186Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=1f444005-168e-6c57-4312-7817a9e595b8 fallback=10.32.9.116:8300 error="Could not find address for server id 1f444005-168e-6c57-4312-7817a9e595b8"
    2021-01-23T23:27:25.583Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Voter 1f444005-168e-6c57-4312-7817a9e595b8 10.32.9.116:8300}" error="dial tcp <nil>->10.32.9.116:8300: connect: no route to host"
    2021-01-23T23:27:25.604Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=1f444005-168e-6c57-4312-7817a9e595b8 fallback=10.32.9.116:8300 error="Could not find address for server id 1f444005-168e-6c57-4312-7817a9e595b8"
    2021-01-23T23:27:27.473Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.117:8300 leader=10.32.4.57:8300
    2021-01-23T23:27:28.655Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Voter 1f444005-168e-6c57-4312-7817a9e595b8 10.32.9.116:8300}" error="dial tcp <nil>->10.32.9.116:8300: connect: no route to host"
    2021-01-23T23:27:28.655Z [ERROR] agent.server.raft: failed to heartbeat to: peer=10.32.9.116:8300 error="dial tcp <nil>->10.32.9.116:8300: connect: no route to host"
    2021-01-23T23:27:29.014Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-23T23:27:35.654Z [INFO]  agent.server.serf.wan: serf: EventMemberLeave: consul-consul-server-0.dc1 10.32.9.116
    2021-01-23T23:27:35.654Z [INFO]  agent.server: Handled event for server in area: event=member-leave server=consul-consul-server-0.dc1 area=wan
    2021-01-23T23:27:36.088Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.117:8300 leader=10.32.4.57:8300
    2021-01-23T23:27:39.014Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-23T23:27:43.788Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.117:8300 leader=10.32.4.57:8300
    2021-01-23T23:27:50.558Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.117:8300 leader=10.32.4.57:8300
    2021-01-23T23:27:52.508Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: consul-consul-server-0 10.32.9.117
    2021-01-23T23:27:52.509Z [INFO]  agent.server: Adding LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.117:8300) (DC: dc1)"
    2021-01-23T23:27:52.510Z [INFO]  agent.server.raft: updating configuration: command=AddNonvoter server-id=1f444005-168e-6c57-4312-7817a9e595b8 server-addr=10.32.9.117:8300 servers="[{Suffrage:Voter ID:121a7669-ac80-c43c-ae04-427c0cea9135 Address:10.32.4.57:8300} {Suffrage:Voter ID:c7a7e211-cee1-f11b-7487-4d83b6fb8430 Address:10.32.3.56:8300} {Suffrage:Nonvoter ID:1f444005-168e-6c57-4312-7817a9e595b8 Address:10.32.9.117:8300}]"
    2021-01-23T23:27:52.513Z [INFO]  agent.server.raft: added peer, starting replication: peer=1f444005-168e-6c57-4312-7817a9e595b8
    2021-01-23T23:27:52.515Z [ERROR] agent.server.raft: peer has newer term, stopping replication: peer="{Nonvoter 1f444005-168e-6c57-4312-7817a9e595b8 10.32.9.117:8300}"
    2021-01-23T23:27:52.516Z [INFO]  agent.server.raft: entering follower state: follower="Node at 10.32.4.57:8300 [Follower]" leader=
    2021-01-23T23:27:52.516Z [INFO]  agent.server.raft: aborting pipeline replication: peer="{Voter c7a7e211-cee1-f11b-7487-4d83b6fb8430 10.32.3.56:8300}"
    2021-01-23T23:27:52.516Z [ERROR] agent.server.autopilot: failed to add raft non-voting peer: id=1f444005-168e-6c57-4312-7817a9e595b8 address=10.32.9.117:8300 error="leadership lost while committing log"
    2021-01-23T23:27:52.517Z [ERROR] agent.server: failed to reconcile member: member="{consul-consul-server-0 10.32.9.117 8301 map[acls:2 build:1.9.2:6530cf37 dc:dc1 expect:3 ft_fs:1 ft_si:1 id:1f444005-168e-6c57-4312-7817a9e595b8 port:8300 raft_vsn:3 role:consul segment: vsn:2 vsn_max:3 vsn_min:2 wan_join_port:8302] alive 1 5 2 2 5 4}" error="leadership lost while committing log"
    2021-01-23T23:27:52.517Z [INFO]  agent.server: cluster leadership lost
    2021-01-23T23:27:54.137Z [WARN]  agent.server.coordinate: Batch update failed: error="node is not the leader"
    2021-01-23T23:27:57.922Z [WARN]  agent.server.raft: heartbeat timeout reached, starting election: last-leader=
    2021-01-23T23:27:57.922Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.4.57:8300 [Candidate]" term=5
    2021-01-23T23:27:58.036Z [INFO]  agent.server.raft: election won: tally=2
    2021-01-23T23:27:58.036Z [INFO]  agent.server.raft: entering leader state: leader="Node at 10.32.4.57:8300 [Leader]"
    2021-01-23T23:27:58.036Z [INFO]  agent.server.raft: added peer, starting replication: peer=c7a7e211-cee1-f11b-7487-4d83b6fb8430
    2021-01-23T23:27:58.036Z [INFO]  agent.server: cluster leadership acquired
    2021-01-23T23:27:58.036Z [INFO]  agent.server: New leader elected: payload=consul-consul-server-2
    2021-01-23T23:27:58.036Z [INFO]  agent.server.raft: added peer, starting replication: peer=1f444005-168e-6c57-4312-7817a9e595b8
    2021-01-23T23:27:58.036Z [INFO]  agent.server.raft: pipelining replication: peer="{Voter c7a7e211-cee1-f11b-7487-4d83b6fb8430 10.32.3.56:8300}"
    2021-01-23T23:27:58.038Z [ERROR] agent.server.raft: peer has newer term, stopping replication: peer="{Nonvoter 1f444005-168e-6c57-4312-7817a9e595b8 10.32.9.117:8300}"
    2021-01-23T23:27:58.038Z [INFO]  agent.server.raft: entering follower state: follower="Node at 10.32.4.57:8300 [Follower]" leader=
    2021-01-23T23:27:58.038Z [INFO]  agent.server.raft: aborting pipeline replication: peer="{Voter c7a7e211-cee1-f11b-7487-4d83b6fb8430 10.32.3.56:8300}"
    2021-01-23T23:27:58.038Z [ERROR] agent.server: failed to wait for barrier: error="node is not the leader"
    2021-01-23T23:27:58.039Z [INFO]  agent.server: cluster leadership lost
    2021-01-23T23:27:59.172Z [WARN]  agent.server.raft: rejecting vote request since our last term is greater: candidate=10.32.9.117:8300 last-term=5 last-candidate-term=2
    2021-01-23T23:28:06.019Z [WARN]  agent.server.raft: heartbeat timeout reached, starting election: last-leader=
    2021-01-23T23:28:06.019Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.4.57:8300 [Candidate]" term=9
    2021-01-23T23:28:06.135Z [INFO]  agent.server.raft: election won: tally=2
    2021-01-23T23:28:06.135Z [INFO]  agent.server.raft: entering leader state: leader="Node at 10.32.4.57:8300 [Leader]"
    2021-01-23T23:28:06.135Z [INFO]  agent.server.raft: added peer, starting replication: peer=c7a7e211-cee1-f11b-7487-4d83b6fb8430
    2021-01-23T23:28:06.135Z [INFO]  agent.server.raft: added peer, starting replication: peer=1f444005-168e-6c57-4312-7817a9e595b8
    2021-01-23T23:28:06.135Z [INFO]  agent.server: cluster leadership acquired
    2021-01-23T23:28:06.135Z [INFO]  agent.server: New leader elected: payload=consul-consul-server-2
    2021-01-23T23:28:06.139Z [INFO]  agent.server.raft: pipelining replication: peer="{Voter c7a7e211-cee1-f11b-7487-4d83b6fb8430 10.32.3.56:8300}"
    2021-01-23T23:28:06.140Z [WARN]  agent.server.raft: appendEntries rejected, sending older logs: peer="{Nonvoter 1f444005-168e-6c57-4312-7817a9e595b8 10.32.9.117:8300}" next=167
    2021-01-23T23:28:06.144Z [INFO]  agent.server: initializing acls
    2021-01-23T23:28:06.144Z [INFO]  agent.leader: started routine: routine="legacy ACL token upgrade"
    2021-01-23T23:28:06.144Z [INFO]  agent.leader: started routine: routine="acl token reaping"
    2021-01-23T23:28:06.145Z [INFO]  agent.leader: started routine: routine="federation state anti-entropy"
    2021-01-23T23:28:06.145Z [INFO]  agent.leader: started routine: routine="federation state pruning"
    2021-01-23T23:28:06.146Z [INFO]  agent.leader: started routine: routine="intermediate cert renew watch"
    2021-01-23T23:28:06.146Z [INFO]  agent.leader: started routine: routine="CA root pruning"
    2021-01-23T23:28:06.146Z [INFO]  agent.server: member joined, marking health alive: member=consul-consul-server-0
    2021-01-23T23:28:06.167Z [INFO]  agent.server.raft: pipelining replication: peer="{Nonvoter 1f444005-168e-6c57-4312-7817a9e595b8 10.32.9.117:8300}"
    2021-01-23T23:28:10.738Z [INFO]  agent.server.serf.lan: serf: EventMemberUpdate: consul-consul-server-0
    2021-01-23T23:28:10.738Z [INFO]  agent.server: Updating LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.117:8300) (DC: dc1)"
    2021-01-23T23:28:10.973Z [INFO]  agent.server.memberlist.wan: memberlist: Updating address for left or failed node consul-consul-server-0.dc1 from 10.32.9.116:8302 to 10.32.9.117:8302
    2021-01-23T23:28:10.973Z [INFO]  agent.server.serf.wan: serf: EventMemberJoin: consul-consul-server-0.dc1 10.32.9.117
    2021-01-23T23:28:10.973Z [INFO]  agent.server: Handled event for server in area: event=member-join server=consul-consul-server-0.dc1 area=wan
    2021-01-23T23:28:16.148Z [INFO]  agent.server.autopilot: Promoting server: id=1f444005-168e-6c57-4312-7817a9e595b8 address=10.32.9.117:8300 name=consul-consul-server-0
    2021-01-23T23:28:16.148Z [INFO]  agent.server.raft: updating configuration: command=AddStaging server-id=1f444005-168e-6c57-4312-7817a9e595b8 server-addr=10.32.9.117:8300 servers="[{Suffrage:Voter ID:121a7669-ac80-c43c-ae04-427c0cea9135 Address:10.32.4.57:8300} {Suffrage:Voter ID:c7a7e211-cee1-f11b-7487-4d83b6fb8430 Address:10.32.3.56:8300} {Suffrage:Voter ID:1f444005-168e-6c57-4312-7817a9e595b8 Address:10.32.9.117:8300}]"

Server-0 logs (coming up)

    2021-01-23T23:27:19.007Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-dous 10.32.3.58
    2021-01-23T23:27:19.007Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-cohw 10.32.9.113
    2021-01-23T23:27:19.007Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-qcpt 10.32.7.30
    2021-01-23T23:27:19.007Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-9n7d 10.32.4.56
    2021-01-23T23:27:19.007Z [WARN]  agent.server.memberlist.lan: memberlist: Refuting a suspect message (from: consul-consul-server-0)
    2021-01-23T23:27:19.007Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: consul-consul-server-2 10.32.4.57
    2021-01-23T23:27:19.007Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: consul-consul-server-1 10.32.3.56
    2021-01-23T23:27:19.008Z [INFO]  agent.server.serf.lan: serf: Re-joined to previously known node: consul-consul-server-2: 10.32.4.57:8301
    2021-01-23T23:27:19.008Z [INFO]  agent.server: Adding LAN server: server="consul-consul-server-2 (Addr: tcp/10.32.4.57:8300) (DC: dc1)"
    2021-01-23T23:27:19.008Z [INFO]  agent.server: Adding LAN server: server="consul-consul-server-1 (Addr: tcp/10.32.3.56:8300) (DC: dc1)"
    2021-01-23T23:27:19.243Z [INFO]  agent: (LAN) joined: number_of_nodes=3
    2021-01-23T23:27:19.243Z [INFO]  agent: Join cluster completed. Synced with initial agents: cluster=LAN num_agents=3
    2021-01-23T23:27:20.602Z [ERROR] agent.http: Request error: method=GET url=/v1/agent/members?segment=_all from=127.0.0.1:42968 error="ACL not found"
    2021-01-23T23:27:24.904Z [ERROR] agent.http: Request error: method=GET url=/v1/agent/members?segment=_all from=127.0.0.1:42982 error="ACL not found"
    2021-01-23T23:27:26.409Z [ERROR] agent.anti_entropy: failed to sync remote state: error="No cluster leader"
    2021-01-23T23:27:27.459Z [WARN]  agent.server.raft: heartbeat timeout reached, starting election: last-leader=
    2021-01-23T23:27:27.459Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.9.117:8300 [Candidate]" term=3
    2021-01-23T23:27:27.475Z [INFO]  agent.server.raft: entering follower state: follower="Node at 10.32.9.117:8300 [Follower]" leader=
    2021-01-23T23:27:28.904Z [ERROR] agent.http: Request error: method=GET url=/v1/agent/members?segment=_all from=127.0.0.1:42998 error="ACL not found"
    2021-01-23T23:27:32.905Z [ERROR] agent.http: Request error: method=GET url=/v1/agent/members?segment=_all from=127.0.0.1:43008 error="ACL not found"
    2021-01-23T23:27:36.080Z [WARN]  agent.server.raft: heartbeat timeout reached, starting election: last-leader=
    2021-01-23T23:27:36.080Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.9.117:8300 [Candidate]" term=5
    2021-01-23T23:27:38.003Z [ERROR] agent.http: Request error: method=GET url=/v1/agent/members?segment=_all from=127.0.0.1:43022 error="ACL not found"
    2021-01-23T23:27:41.704Z [ERROR] agent.http: Request error: method=GET url=/v1/agent/members?segment=_all from=127.0.0.1:43034 error="ACL not found"
    2021-01-23T23:27:43.780Z [WARN]  agent.server.raft: Election timeout reached, restarting election
    2021-01-23T23:27:43.780Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.9.117:8300 [Candidate]" term=6
    2021-01-23T23:27:45.902Z [ERROR] agent.http: Request error: method=GET url=/v1/agent/members?segment=_all from=127.0.0.1:43042 error="ACL not found"
    2021-01-23T23:27:50.549Z [WARN]  agent.server.raft: Election timeout reached, restarting election
    2021-01-23T23:27:50.549Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.9.117:8300 [Candidate]" term=7
    2021-01-23T23:27:52.085Z [ERROR] agent.anti_entropy: failed to sync remote state: error="No cluster leader"
    2021-01-23T23:27:52.965Z [ERROR] agent: Coordinate update error: error="No cluster leader"
    2021-01-23T23:27:58.215Z [INFO]  agent.server: New leader elected: payload=consul-consul-server-2
    2021-01-23T23:27:59.162Z [WARN]  agent.server.raft: Election timeout reached, restarting election
    2021-01-23T23:27:59.162Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.9.117:8300 [Candidate]" term=8
    2021-01-23T23:28:06.138Z [WARN]  agent.server.raft: failed to get previous log: previous-index=181 last-index=166 error="log not found"
    2021-01-23T23:28:06.139Z [INFO]  agent.server.raft: entering follower state: follower="Node at 10.32.9.117:8300 [Follower]" leader=10.32.4.57:8300
    2021-01-23T23:28:06.204Z [WARN]  agent.fsm: EnsureRegistration failed: error="failed inserting node: Error while renaming Node ID: "2bf40850-3088-eb8c-fa61-8541d44e4aaa": Node name gke-consul-1-default-pool-51946b93-dous is reserved by node ca5405b8-688c-44ea-48e0-8ca2a9316249 with name gke-consul-1-default-pool-51946b93-dous (10.32.3.55)"
    2021-01-23T23:28:06.404Z [INFO]  agent.server: New leader elected: payload=consul-consul-server-2
    2021-01-23T23:28:06.537Z [INFO]  agent: Synced node info
    2021-01-23T23:28:10.203Z [INFO]  agent.server.serf.lan: serf: EventMemberUpdate: consul-consul-server-0
    2021-01-23T23:28:10.203Z [INFO]  agent.server: Updating LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.117:8300) (DC: dc1)"
    2021-01-23T23:28:10.603Z [INFO]  agent.server.serf.wan: serf: EventMemberUpdate: consul-consul-server-0.dc1
    2021-01-23T23:28:10.603Z [INFO]  agent.server: Handled event for server in area: event=member-update server=consul-consul-server-0.dc1 area=wan

Client logs

    2021-01-23T23:27:01.938Z [INFO]  agent.client.memberlist.lan: memberlist: Suspect consul-consul-server-0 has failed, no acks received
    2021-01-23T23:27:02.215Z [INFO]  agent.client.serf.lan: serf: EventMemberFailed: consul-consul-server-0 10.32.9.116
    2021-01-23T23:27:02.215Z [INFO]  agent.client: removing server: server="consul-consul-server-0 (Addr: tcp/10.32.9.116:8300) (DC: dc1)"
    2021-01-23T23:27:10.415Z [INFO]  agent.client: New leader elected: payload=consul-consul-server-2
    2021-01-23T23:27:22.507Z [INFO]  agent.client.serf.lan: serf: EventMemberLeave (forced): consul-consul-server-0 10.32.9.116
    2021-01-23T23:27:22.507Z [INFO]  agent.client: removing server: server="consul-consul-server-0 (Addr: tcp/10.32.9.116:8300) (DC: dc1)"
    2021-01-23T23:27:52.815Z [INFO]  agent.client.serf.lan: serf: EventMemberJoin: consul-consul-server-0 10.32.9.117
    2021-01-23T23:27:52.816Z [INFO]  agent.client: adding server: server="consul-consul-server-0 (Addr: tcp/10.32.9.117:8300) (DC: dc1)"
    2021-01-23T23:27:58.415Z [INFO]  agent.client: New leader elected: payload=consul-consul-server-2
    2021-01-23T23:28:06.216Z [INFO]  agent.client: New leader elected: payload=consul-consul-server-2
    2021-01-23T23:28:10.416Z [INFO]  agent.client.serf.lan: serf: EventMemberUpdate: consul-consul-server-0
    2021-01-23T23:28:10.416Z [INFO]  agent.client: updating server: server="consul-consul-server-0 (Addr: tcp/10.32.9.117:8300) (DC: dc1)"

## Force Delete Server

Server-0 logs (coming up)

    2021-01-23T23:34:06.005Z [INFO]  agent: Joining cluster...: cluster=LAN
    2021-01-23T23:34:06.005Z [INFO]  agent: (LAN) joining: lan_addresses=[consul-consul-server-0.consul-consul-server.default.svc:8301, consul-consul-server-1.consul-consul-server.default.svc:8301, consul-consul-server-2.consul-consul-server.default.svc:8301]
    2021-01-23T23:34:06.006Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: consul-consul-server-2 10.32.4.57
    2021-01-23T23:34:06.006Z [WARN]  agent.server.memberlist.lan: memberlist: Refuting a suspect message (from: consul-consul-server-0)
    2021-01-23T23:34:06.006Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-9n7d 10.32.4.56
    2021-01-23T23:34:06.007Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-qcpt 10.32.7.30
    2021-01-23T23:34:06.007Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: consul-consul-server-1 10.32.3.56
    2021-01-23T23:34:06.007Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-dous 10.32.3.58
    2021-01-23T23:34:06.007Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-cohw 10.32.9.113
    2021-01-23T23:34:06.007Z [INFO]  agent.server.serf.lan: serf: Re-joined to previously known node: gke-consul-1-default-pool-51946b93-dous: 10.32.3.58:8301
    2021-01-23T23:34:06.007Z [INFO]  agent.server: Adding LAN server: server="consul-consul-server-2 (Addr: tcp/10.32.4.57:8300) (DC: dc1)"
    2021-01-23T23:34:06.007Z [INFO]  agent.server: Adding LAN server: server="consul-consul-server-1 (Addr: tcp/10.32.3.56:8300) (DC: dc1)"
    2021-01-23T23:34:06.007Z [INFO]  agent: started state syncer
    ==> Consul agent running!
    2021-01-23T23:34:06.010Z [INFO]  agent.server.serf.wan: serf: EventMemberJoin: consul-consul-server-1.dc1 10.32.3.56
    2021-01-23T23:34:06.010Z [INFO]  agent.server: Handled event for server in area: event=member-join server=consul-consul-server-1.dc1 area=wan
    2021-01-23T23:34:06.010Z [INFO]  agent.server.serf.wan: serf: EventMemberJoin: consul-consul-server-2.dc1 10.32.4.57
    2021-01-23T23:34:06.011Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.118:8302 Theirs: 10.32.9.117:8302 Old state: 0
    2021-01-23T23:34:06.011Z [ERROR] agent.server.serf.wan: serf: Node name conflicts with another node at 10.32.9.117:8302. Names must be unique! (Resolution enabled: false)
    2021-01-23T23:34:06.011Z [INFO]  agent.server: Handled event for server in area: event=member-join server=consul-consul-server-2.dc1 area=wan
    2021-01-23T23:34:06.102Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.118:8302 Theirs: 10.32.9.117:8302 Old state: 0
    2021-01-23T23:34:06.102Z [ERROR] agent.server.serf.wan: serf: Node name conflicts with another node at 10.32.9.117:8302. Names must be unique! (Resolution enabled: false)
    2021-01-23T23:34:06.103Z [INFO]  agent.server.serf.wan: serf: Re-joined to previously known node: consul-consul-server-1.dc1: 10.32.3.56:8302
    2021-01-23T23:34:06.202Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.118:8302 Theirs: 10.32.9.117:8302 Old state: 0
    2021-01-23T23:34:06.202Z [ERROR] agent.server.serf.wan: serf: Node name conflicts with another node at 10.32.9.117:8302. Names must be unique! (Resolution enabled: false)
    2021-01-23T23:34:06.302Z [INFO]  agent: (LAN) joined: number_of_nodes=3
    2021-01-23T23:34:06.302Z [INFO]  agent: Join cluster completed. Synced with initial agents: cluster=LAN num_agents=3
    2021-01-23T23:34:10.913Z [WARN]  agent.server.memberlist.wan: memberlist: Refuting a suspect message (from: consul-consul-server-1.dc1)
    2021-01-23T23:34:11.273Z [WARN]  agent.server.raft: heartbeat timeout reached, starting election: last-leader=
    2021-01-23T23:34:11.274Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.9.118:8300 [Candidate]" term=10
    2021-01-23T23:34:13.122Z [ERROR] agent.anti_entropy: failed to sync remote state: error="No cluster leader"
    2021-01-23T23:34:19.713Z [WARN]  agent.server.raft: Election timeout reached, restarting election
    2021-01-23T23:34:19.713Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.9.118:8300 [Candidate]" term=11
    2021-01-23T23:34:25.343Z [WARN]  agent.server.raft: Election timeout reached, restarting election
    2021-01-23T23:34:25.343Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.9.118:8300 [Candidate]" term=12
    2021-01-23T23:34:29.941Z [ERROR] agent: Coordinate update error: error="No cluster leader"
    2021-01-23T23:34:32.240Z [WARN]  agent.server.raft: Election timeout reached, restarting election
    2021-01-23T23:34:32.240Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.9.118:8300 [Candidate]" term=13
    2021-01-23T23:34:39.160Z [WARN]  agent.server.raft: Election timeout reached, restarting election
    2021-01-23T23:34:39.160Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.9.118:8300 [Candidate]" term=14
    2021-01-23T23:34:40.694Z [ERROR] agent.anti_entropy: failed to sync remote state: error="No cluster leader"
    2021-01-23T23:34:45.528Z [WARN]  agent.server.raft: Election timeout reached, restarting election
    2021-01-23T23:34:45.528Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.9.118:8300 [Candidate]" term=15
    2021-01-23T23:34:51.136Z [WARN]  agent.server.raft: failed to get previous log: previous-index=259 last-index=249 error="log not found"
    2021-01-23T23:34:51.136Z [INFO]  agent.server.raft: entering follower state: follower="Node at 10.32.9.118:8300 [Follower]" leader=10.32.4.57:8300
    2021-01-23T23:34:51.306Z [INFO]  agent.server: New leader elected: payload=consul-consul-server-2
    2021-01-23T23:34:51.307Z [WARN]  agent.fsm: EnsureRegistration failed: error="failed inserting node: Error while renaming Node ID: "2bf40850-3088-eb8c-fa61-8541d44e4aaa": Node name gke-consul-1-default-pool-51946b93-dous is reserved by node ca5405b8-688c-44ea-48e0-8ca2a9316249 with name gke-consul-1-default-pool-51946b93-dous (10.32.3.55)"
    2021-01-23T23:34:53.891Z [INFO]  agent: Synced node info
    2021-01-23T23:34:57.203Z [INFO]  agent.server.serf.lan: serf: EventMemberUpdate: consul-consul-server-0
    2021-01-23T23:34:57.204Z [INFO]  agent.server: Updating LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.118:8300) (DC: dc1)"
    2021-01-23T23:34:57.512Z [INFO]  agent.server.serf.wan: serf: EventMemberUpdate: consul-consul-server-0.dc1
    2021-01-23T23:34:57.512Z [INFO]  agent.server: Handled event for server in area: event=member-update server=consul-consul-server-0.dc1 area=wan

Server-1 logs

    2021-01-23T23:34:03.654Z [INFO]  agent.server.memberlist.lan: memberlist: Suspect consul-consul-server-0 has failed, no acks received
    2021-01-23T23:34:06.035Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.117:8302 Theirs: 10.32.9.118:8302 Old state: 0
    2021-01-23T23:34:06.035Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.117:8302 and 10.32.9.118:8302 are claiming
    2021-01-23T23:34:06.135Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.117:8302 Theirs: 10.32.9.118:8302 Old state: 0
    2021-01-23T23:34:06.135Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.117:8302 and 10.32.9.118:8302 are claiming
    2021-01-23T23:34:06.235Z [ERROR] agent.server.memberlist.lan: memberlist: Conflicting address for consul-consul-server-0. Mine: 10.32.9.117:8301 Theirs: 10.32.9.118:8301 Old state: 1
    2021-01-23T23:34:06.235Z [WARN]  agent.server.serf.lan: serf: Name conflict for 'consul-consul-server-0' both 10.32.9.117:8301 and 10.32.9.118:8301 are claiming
    2021-01-23T23:34:06.236Z [ERROR] agent.server.memberlist.lan: memberlist: Conflicting address for consul-consul-server-0. Mine: 10.32.9.117:8301 Theirs: 10.32.9.118:8301 Old state: 1
    2021-01-23T23:34:06.236Z [WARN]  agent.server.serf.lan: serf: Name conflict for 'consul-consul-server-0' both 10.32.9.117:8301 and 10.32.9.118:8301 are claiming
    2021-01-23T23:34:06.335Z [ERROR] agent.server.memberlist.lan: memberlist: Conflicting address for consul-consul-server-0. Mine: 10.32.9.117:8301 Theirs: 10.32.9.118:8301 Old state: 1
    2021-01-23T23:34:06.336Z [WARN]  agent.server.serf.lan: serf: Name conflict for 'consul-consul-server-0' both 10.32.9.117:8301 and 10.32.9.118:8301 are claiming
    2021-01-23T23:34:06.340Z [ERROR] agent.server.memberlist.lan: memberlist: Conflicting address for consul-consul-server-0. Mine: 10.32.9.117:8301 Theirs: 10.32.9.118:8301 Old state: 1
    2021-01-23T23:34:06.340Z [WARN]  agent.server.serf.lan: serf: Name conflict for 'consul-consul-server-0' both 10.32.9.117:8301 and 10.32.9.118:8301 are claiming
    2021-01-23T23:34:06.435Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.117:8302 Theirs: 10.32.9.118:8302 Old state: 0
    2021-01-23T23:34:06.435Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.117:8302 and 10.32.9.118:8302 are claiming
    2021-01-23T23:34:07.412Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.117:8302 Theirs: 10.32.9.118:8302 Old state: 0
    2021-01-23T23:34:07.412Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.117:8302 and 10.32.9.118:8302 are claiming
    2021-01-23T23:34:07.654Z [INFO]  agent.server.memberlist.lan: memberlist: Marking consul-consul-server-0 as failed, suspect timeout reached (2 peer confirmations)
    2021-01-23T23:34:07.654Z [INFO]  agent.server.serf.lan: serf: EventMemberFailed: consul-consul-server-0 10.32.9.117
    2021-01-23T23:34:07.654Z [INFO]  agent.server: Removing LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.117:8300) (DC: dc1)"
    2021-01-23T23:34:08.654Z [INFO]  agent.server.memberlist.lan: memberlist: Suspect consul-consul-server-0 has failed, no acks received
    2021-01-23T23:34:10.654Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-23T23:34:11.278Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.118:8300 leader=10.32.4.57:8300
    2021-01-23T23:34:11.412Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.117:8302 Theirs: 10.32.9.118:8302 Old state: 1
    2021-01-23T23:34:11.412Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.117:8302 and 10.32.9.118:8302 are claiming
    2021-01-23T23:34:11.912Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.117:8302 Theirs: 10.32.9.118:8302 Old state: 1
    2021-01-23T23:34:11.912Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.117:8302 and 10.32.9.118:8302 are claiming
    2021-01-23T23:34:17.289Z [INFO]  agent.server.serf.lan: serf: EventMemberLeave (forced): consul-consul-server-0 10.32.9.117
    2021-01-23T23:34:17.289Z [INFO]  agent.server: Removing LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.117:8300) (DC: dc1)"
    2021-01-23T23:34:19.718Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.118:8300 leader=10.32.4.57:8300
    2021-01-23T23:34:25.347Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.118:8300 leader=10.32.4.57:8300
    2021-01-23T23:34:25.653Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-23T23:34:32.244Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.118:8300 leader=10.32.4.57:8300
    2021-01-23T23:34:35.734Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-23T23:34:39.164Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.118:8300 leader=10.32.4.57:8300
    2021-01-23T23:34:40.654Z [INFO]  agent.server.memberlist.wan: memberlist: Marking consul-consul-server-0.dc1 as failed, suspect timeout reached (0 peer confirmations)
    2021-01-23T23:34:40.654Z [INFO]  agent.server.serf.wan: serf: EventMemberLeave: consul-consul-server-0.dc1 10.32.9.117
    2021-01-23T23:34:40.654Z [INFO]  agent.server: Handled event for server in area: event=member-leave server=consul-consul-server-0.dc1 area=wan
    2021-01-23T23:34:40.735Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-23T23:34:42.907Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: consul-consul-server-0 10.32.9.118
    2021-01-23T23:34:42.907Z [INFO]  agent.server: Adding LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.118:8300) (DC: dc1)"
    2021-01-23T23:34:45.533Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.118:8300 leader=10.32.4.57:8300
    2021-01-23T23:34:48.374Z [WARN]  agent.server.raft: heartbeat timeout reached, starting election: last-leader=10.32.4.57:8300
    2021-01-23T23:34:48.374Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.3.56:8300 [Candidate]" term=10
    2021-01-23T23:34:48.441Z [INFO]  agent.server.raft: entering follower state: follower="Node at 10.32.3.56:8300 [Follower]" leader=
    2021-01-23T23:34:51.507Z [INFO]  agent.server: New leader elected: payload=consul-consul-server-2
    2021-01-23T23:34:57.335Z [INFO]  agent.server.serf.lan: serf: EventMemberUpdate: consul-consul-server-0
    2021-01-23T23:34:57.336Z [INFO]  agent.server: Updating LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.118:8300) (DC: dc1)"
    2021-01-23T23:34:57.912Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.117:8302 Theirs: 10.32.9.118:8302 Old state: 2
    2021-01-23T23:34:57.912Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.117:8302 and 10.32.9.118:8302 are claiming
    2021-01-23T23:34:58.412Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.117:8302 Theirs: 10.32.9.118:8302 Old state: 2
    2021-01-23T23:34:58.412Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.117:8302 and 10.32.9.118:8302 are claiming
    2021-01-23T23:35:57.515Z [INFO]  agent.server.serf.wan: serf: EventMemberJoin: consul-consul-server-0.dc1 10.32.9.118
    2021-01-23T23:35:57.515Z [INFO]  agent.server: Handled event for server in area: event=member-join server=consul-consul-server-0.dc1 area=wan

Server-2 logs

    2021-01-23T23:34:02.290Z [INFO]  agent.server.raft: aborting pipeline replication: peer="{Nonvoter 1f444005-168e-6c57-4312-7817a9e595b8 10.32.9.117:8300}"
    2021-01-23T23:34:02.373Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Nonvoter 1f444005-168e-6c57-4312-7817a9e595b8 10.32.9.117:8300}" error=EOF
    2021-01-23T23:34:02.471Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Nonvoter 1f444005-168e-6c57-4312-7817a9e595b8 10.32.9.117:8300}" error="dial tcp <nil>->10.32.9.117:8300: connect: connection refused"
    2021-01-23T23:34:02.561Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Nonvoter 1f444005-168e-6c57-4312-7817a9e595b8 10.32.9.117:8300}" error="dial tcp <nil>->10.32.9.117:8300: connect: connection refused"
    2021-01-23T23:34:02.565Z [ERROR] agent.server.raft: failed to heartbeat to: peer=10.32.9.117:8300 error="dial tcp <nil>->10.32.9.117:8300: connect: connection refused"
    2021-01-23T23:34:02.661Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Nonvoter 1f444005-168e-6c57-4312-7817a9e595b8 10.32.9.117:8300}" error="dial tcp <nil>->10.32.9.117:8300: connect: connection refused"
    2021-01-23T23:34:02.754Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Nonvoter 1f444005-168e-6c57-4312-7817a9e595b8 10.32.9.117:8300}" error="dial tcp <nil>->10.32.9.117:8300: connect: connection refused"
    2021-01-23T23:34:04.147Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="rpc error making call: stream closed"
    2021-01-23T23:34:04.790Z [WARN]  agent.server.raft: failed to contact: server-id=1f444005-168e-6c57-4312-7817a9e595b8 time=2.500206811s
    2021-01-23T23:34:05.147Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="context deadline exceeded"
    2021-01-23T23:34:06.010Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.117:8302 Theirs: 10.32.9.118:8302 Old state: 0
    2021-01-23T23:34:06.011Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.117:8302 and 10.32.9.118:8302 are claiming
    2021-01-23T23:34:06.015Z [INFO]  agent.server.memberlist.lan: memberlist: Suspect consul-consul-server-0 has failed, no acks received
    2021-01-23T23:34:06.205Z [ERROR] agent.server.memberlist.lan: memberlist: Conflicting address for consul-consul-server-0. Mine: 10.32.9.117:8301 Theirs: 10.32.9.118:8301 Old state: 1
    2021-01-23T23:34:06.205Z [WARN]  agent.server.serf.lan: serf: Name conflict for 'consul-consul-server-0' both 10.32.9.117:8301 and 10.32.9.118:8301 are claiming
    2021-01-23T23:34:06.294Z [ERROR] agent.server.memberlist.lan: memberlist: Conflicting address for consul-consul-server-0. Mine: 10.32.9.117:8301 Theirs: 10.32.9.118:8301 Old state: 1
    2021-01-23T23:34:06.294Z [WARN]  agent.server.serf.lan: serf: Name conflict for 'consul-consul-server-0' both 10.32.9.117:8301 and 10.32.9.118:8301 are claiming
    2021-01-23T23:34:06.316Z [ERROR] agent.server.memberlist.lan: memberlist: Conflicting address for consul-consul-server-0. Mine: 10.32.9.117:8301 Theirs: 10.32.9.118:8301 Old state: 1
    2021-01-23T23:34:06.316Z [WARN]  agent.server.serf.lan: serf: Name conflict for 'consul-consul-server-0' both 10.32.9.117:8301 and 10.32.9.118:8301 are claiming
    2021-01-23T23:34:06.317Z [ERROR] agent.server.memberlist.lan: memberlist: Conflicting address for consul-consul-server-0. Mine: 10.32.9.117:8301 Theirs: 10.32.9.118:8301 Old state: 1
    2021-01-23T23:34:06.318Z [WARN]  agent.server.serf.lan: serf: Name conflict for 'consul-consul-server-0' both 10.32.9.117:8301 and 10.32.9.118:8301 are claiming
    2021-01-23T23:34:06.413Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.117:8302 Theirs: 10.32.9.118:8302 Old state: 0
    2021-01-23T23:34:06.413Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.117:8302 and 10.32.9.118:8302 are claiming
    2021-01-23T23:34:06.912Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.117:8302 Theirs: 10.32.9.118:8302 Old state: 0
    2021-01-23T23:34:06.912Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.117:8302 and 10.32.9.118:8302 are claiming
    2021-01-23T23:34:07.147Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="context deadline exceeded"
    2021-01-23T23:34:07.271Z [WARN]  agent.server.raft: failed to contact: server-id=1f444005-168e-6c57-4312-7817a9e595b8 time=4.981271902s
    2021-01-23T23:34:07.738Z [INFO]  agent.server.memberlist.lan: memberlist: Marking consul-consul-server-0 as failed, suspect timeout reached (2 peer confirmations)
    2021-01-23T23:34:07.738Z [INFO]  agent.server.serf.lan: serf: EventMemberFailed: consul-consul-server-0 10.32.9.117
    2021-01-23T23:34:07.739Z [INFO]  agent.server: Removing LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.117:8300) (DC: dc1)"
    2021-01-23T23:34:07.739Z [INFO]  agent.server: member failed, marking health critical: member=consul-consul-server-0
    2021-01-23T23:34:09.147Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="context deadline exceeded"
    2021-01-23T23:34:09.759Z [WARN]  agent.server.raft: failed to contact: server-id=1f444005-168e-6c57-4312-7817a9e595b8 time=7.469621887s
    2021-01-23T23:34:11.147Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="context deadline exceeded"
    2021-01-23T23:34:11.278Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.118:8300 leader=10.32.4.57:8300
    2021-01-23T23:34:11.412Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.117:8302 Theirs: 10.32.9.118:8302 Old state: 1
    2021-01-23T23:34:11.412Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.117:8302 and 10.32.9.118:8302 are claiming
    2021-01-23T23:34:11.912Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.117:8302 Theirs: 10.32.9.118:8302 Old state: 1
    2021-01-23T23:34:11.912Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.117:8302 and 10.32.9.118:8302 are claiming
    2021-01-23T23:34:12.922Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Nonvoter 1f444005-168e-6c57-4312-7817a9e595b8 10.32.9.117:8300}" error="dial tcp <nil>->10.32.9.117:8300: i/o timeout"
    2021-01-23T23:34:13.082Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=1f444005-168e-6c57-4312-7817a9e595b8 fallback=10.32.9.117:8300 error="Could not find address for server id 1f444005-168e-6c57-4312-7817a9e595b8"
    2021-01-23T23:34:13.147Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="context deadline exceeded"
    2021-01-23T23:34:13.279Z [ERROR] agent.server.raft: failed to heartbeat to: peer=10.32.9.117:8300 error="dial tcp <nil>->10.32.9.117:8300: i/o timeout"
    2021-01-23T23:34:13.289Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=1f444005-168e-6c57-4312-7817a9e595b8 fallback=10.32.9.117:8300 error="Could not find address for server id 1f444005-168e-6c57-4312-7817a9e595b8"
    2021-01-23T23:34:14.014Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-23T23:34:15.147Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="context deadline exceeded"
    2021-01-23T23:34:16.147Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="rpc error getting client: failed to get conn: dial tcp <nil>->10.32.9.117:8300: i/o timeout"
    2021-01-23T23:34:16.147Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="rpc error getting client: failed to get conn: rpc error: lead thread didn't get connection"
    2021-01-23T23:34:16.147Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="rpc error getting client: failed to get conn: rpc error: lead thread didn't get connection"
    2021-01-23T23:34:16.147Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="rpc error getting client: failed to get conn: rpc error: lead thread didn't get connection"
    2021-01-23T23:34:16.147Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="rpc error getting client: failed to get conn: rpc error: lead thread didn't get connection"
    2021-01-23T23:34:16.147Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="rpc error getting client: failed to get conn: rpc error: lead thread didn't get connection"
    2021-01-23T23:34:17.147Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="context deadline exceeded"
    2021-01-23T23:34:17.147Z [INFO]  agent.server.autopilot: Attempting removal of failed server node: id=1f444005-168e-6c57-4312-7817a9e595b8 name=consul-consul-server-0 address=10.32.9.117:8300
    2021-01-23T23:34:17.148Z [INFO]  agent.server.serf.lan: serf: EventMemberLeave (forced): consul-consul-server-0 10.32.9.117
    2021-01-23T23:34:17.148Z [INFO]  agent.server: Removing LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.117:8300) (DC: dc1)"
    2021-01-23T23:34:17.148Z [INFO]  agent.server.raft: updating configuration: command=RemoveServer server-id=1f444005-168e-6c57-4312-7817a9e595b8 server-addr= servers="[{Suffrage:Voter ID:121a7669-ac80-c43c-ae04-427c0cea9135 Address:10.32.4.57:8300} {Suffrage:Voter ID:c7a7e211-cee1-f11b-7487-4d83b6fb8430 Address:10.32.3.56:8300}]"
    2021-01-23T23:34:17.151Z [INFO]  agent.server.raft: removed peer, stopping replication: peer=1f444005-168e-6c57-4312-7817a9e595b8 last-index=254
    2021-01-23T23:34:17.154Z [INFO]  agent.server.autopilot: removed server: id=1f444005-168e-6c57-4312-7817a9e595b8
    2021-01-23T23:34:17.154Z [INFO]  agent.server: deregistering member: member=consul-consul-server-0 reason=left
    2021-01-23T23:34:19.718Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.118:8300 leader=10.32.4.57:8300
    2021-01-23T23:34:20.305Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Nonvoter 1f444005-168e-6c57-4312-7817a9e595b8 10.32.9.117:8300}" error="dial tcp <nil>->10.32.9.117:8300: connect: no route to host"
    2021-01-23T23:34:20.625Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=1f444005-168e-6c57-4312-7817a9e595b8 fallback=10.32.9.117:8300 error="Could not find address for server id 1f444005-168e-6c57-4312-7817a9e595b8"
    2021-01-23T23:34:23.290Z [ERROR] agent.server.raft: failed to heartbeat to: peer=10.32.9.117:8300 error="dial tcp <nil>->10.32.9.117:8300: i/o timeout"
    2021-01-23T23:34:23.503Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Nonvoter 1f444005-168e-6c57-4312-7817a9e595b8 10.32.9.117:8300}" error="dial tcp <nil>->10.32.9.117:8300: connect: no route to host"
    2021-01-23T23:34:24.015Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-23T23:34:25.347Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.118:8300 leader=10.32.4.57:8300
    2021-01-23T23:34:32.244Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.118:8300 leader=10.32.4.57:8300
    2021-01-23T23:34:34.014Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-23T23:34:39.164Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.118:8300 leader=10.32.4.57:8300
    2021-01-23T23:34:40.655Z [INFO]  agent.server.memberlist.wan: memberlist: Marking consul-consul-server-0.dc1 as failed, suspect timeout reached (0 peer confirmations)
    2021-01-23T23:34:40.655Z [INFO]  agent.server.serf.wan: serf: EventMemberLeave: consul-consul-server-0.dc1 10.32.9.117
    2021-01-23T23:34:40.656Z [INFO]  agent.server: Handled event for server in area: event=member-leave server=consul-consul-server-0.dc1 area=wan
    2021-01-23T23:34:42.939Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: consul-consul-server-0 10.32.9.118
    2021-01-23T23:34:42.939Z [INFO]  agent.server: Adding LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.118:8300) (DC: dc1)"
    2021-01-23T23:34:42.940Z [INFO]  agent.server.raft: updating configuration: command=AddNonvoter server-id=1f444005-168e-6c57-4312-7817a9e595b8 server-addr=10.32.9.118:8300 servers="[{Suffrage:Voter ID:121a7669-ac80-c43c-ae04-427c0cea9135 Address:10.32.4.57:8300} {Suffrage:Voter ID:c7a7e211-cee1-f11b-7487-4d83b6fb8430 Address:10.32.3.56:8300} {Suffrage:Nonvoter ID:1f444005-168e-6c57-4312-7817a9e595b8 Address:10.32.9.118:8300}]"
    2021-01-23T23:34:42.946Z [INFO]  agent.server.raft: added peer, starting replication: peer=1f444005-168e-6c57-4312-7817a9e595b8
    2021-01-23T23:34:42.948Z [ERROR] agent.server.raft: peer has newer term, stopping replication: peer="{Nonvoter 1f444005-168e-6c57-4312-7817a9e595b8 10.32.9.118:8300}"
    2021-01-23T23:34:42.948Z [INFO]  agent.server.raft: entering follower state: follower="Node at 10.32.4.57:8300 [Follower]" leader=
    2021-01-23T23:34:42.948Z [INFO]  agent.server.raft: aborting pipeline replication: peer="{Voter c7a7e211-cee1-f11b-7487-4d83b6fb8430 10.32.3.56:8300}"
    2021-01-23T23:34:42.948Z [ERROR] agent.server.autopilot: failed to add raft non-voting peer: id=1f444005-168e-6c57-4312-7817a9e595b8 address=10.32.9.118:8300 error="leadership lost while committing log"
    2021-01-23T23:34:42.948Z [ERROR] agent.server: failed to reconcile member: member="{consul-consul-server-0 10.32.9.118 8301 map[acls:2 build:1.9.2:6530cf37 dc:dc1 expect:3 ft_fs:1 ft_si:1 id:1f444005-168e-6c57-4312-7817a9e595b8 port:8300 raft_vsn:3 role:consul segment: vsn:2 vsn_max:3 vsn_min:2 wan_join_port:8302] alive 1 5 2 2 5 4}" error="leadership lost while committing log"
    2021-01-23T23:34:42.948Z [INFO]  agent.server: cluster leadership lost
    2021-01-23T23:34:44.014Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-23T23:34:45.534Z [WARN]  agent.server.raft: rejecting vote request since our last index is greater: candidate=10.32.9.118:8300 last-index=259 last-candidate-index=249
    2021-01-23T23:34:51.113Z [WARN]  agent.server.raft: heartbeat timeout reached, starting election: last-leader=
    2021-01-23T23:34:51.114Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.4.57:8300 [Candidate]" term=16
    2021-01-23T23:34:51.126Z [INFO]  agent.server.raft: election won: tally=2
    2021-01-23T23:34:51.126Z [INFO]  agent.server.raft: entering leader state: leader="Node at 10.32.4.57:8300 [Leader]"
    2021-01-23T23:34:51.126Z [INFO]  agent.server.raft: added peer, starting replication: peer=c7a7e211-cee1-f11b-7487-4d83b6fb8430
    2021-01-23T23:34:51.126Z [INFO]  agent.server.raft: added peer, starting replication: peer=1f444005-168e-6c57-4312-7817a9e595b8
    2021-01-23T23:34:51.127Z [INFO]  agent.server: cluster leadership acquired
    2021-01-23T23:34:51.127Z [INFO]  agent.server: New leader elected: payload=consul-consul-server-2
    2021-01-23T23:34:51.127Z [INFO]  agent.server.raft: pipelining replication: peer="{Voter c7a7e211-cee1-f11b-7487-4d83b6fb8430 10.32.3.56:8300}"
    2021-01-23T23:34:51.133Z [INFO]  agent.server: initializing acls
    2021-01-23T23:34:51.133Z [INFO]  agent.leader: started routine: routine="legacy ACL token upgrade"
    2021-01-23T23:34:51.133Z [INFO]  agent.leader: started routine: routine="acl token reaping"
    2021-01-23T23:34:51.135Z [INFO]  agent.leader: started routine: routine="federation state anti-entropy"
    2021-01-23T23:34:51.135Z [INFO]  agent.leader: started routine: routine="federation state pruning"
    2021-01-23T23:34:51.136Z [INFO]  agent.leader: started routine: routine="intermediate cert renew watch"
    2021-01-23T23:34:51.136Z [INFO]  agent.leader: started routine: routine="CA root pruning"
    2021-01-23T23:34:51.136Z [INFO]  agent.server: member joined, marking health alive: member=consul-consul-server-0
    2021-01-23T23:34:51.137Z [WARN]  agent.server.raft: appendEntries rejected, sending older logs: peer="{Nonvoter 1f444005-168e-6c57-4312-7817a9e595b8 10.32.9.118:8300}" next=250
    2021-01-23T23:34:51.213Z [INFO]  agent.server.raft: pipelining replication: peer="{Nonvoter 1f444005-168e-6c57-4312-7817a9e595b8 10.32.9.118:8300}"
    2021-01-23T23:34:57.314Z [INFO]  agent.server.serf.lan: serf: EventMemberUpdate: consul-consul-server-0
    2021-01-23T23:34:57.314Z [INFO]  agent.server: Updating LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.118:8300) (DC: dc1)"
    2021-01-23T23:34:57.912Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.117:8302 Theirs: 10.32.9.118:8302 Old state: 2
    2021-01-23T23:34:57.912Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.117:8302 and 10.32.9.118:8302 are claiming
    2021-01-23T23:34:58.412Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.117:8302 Theirs: 10.32.9.118:8302 Old state: 2
    2021-01-23T23:34:58.412Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.117:8302 and 10.32.9.118:8302 are claiming
    2021-01-23T23:35:01.137Z [INFO]  agent.server.autopilot: Promoting server: id=1f444005-168e-6c57-4312-7817a9e595b8 address=10.32.9.118:8300 name=consul-consul-server-0
    2021-01-23T23:35:01.138Z [INFO]  agent.server.raft: updating configuration: command=AddStaging server-id=1f444005-168e-6c57-4312-7817a9e595b8 server-addr=10.32.9.118:8300 servers="[{Suffrage:Voter ID:121a7669-ac80-c43c-ae04-427c0cea9135 Address:10.32.4.57:8300} {Suffrage:Voter ID:c7a7e211-cee1-f11b-7487-4d83b6fb8430 Address:10.32.3.56:8300} {Suffrage:Voter ID:1f444005-168e-6c57-4312-7817a9e595b8 Address:10.32.9.118:8300}]"
    2021-01-23T23:35:57.453Z [INFO]  agent.server.serf.wan: serf: EventMemberJoin: consul-consul-server-0.dc1 10.32.9.118
    2021-01-23T23:35:57.455Z [INFO]  agent.server: Handled event for server in area: event=member-join server=consul-consul-server-0.dc1 area=wan

Client logs

    2021-01-23T23:34:05.939Z [INFO]  agent.client.memberlist.lan: memberlist: Suspect consul-consul-server-0 has failed, no acks received
    2021-01-23T23:34:06.006Z [ERROR] agent.client.memberlist.lan: memberlist: Conflicting address for consul-consul-server-0. Mine: 10.32.9.117:8301 Theirs: 10.32.9.118:8301 Old state: 1
    2021-01-23T23:34:06.006Z [WARN]  agent.client.serf.lan: serf: Name conflict for 'consul-consul-server-0' both 10.32.9.117:8301 and 10.32.9.118:8301 are claiming
    2021-01-23T23:34:06.204Z [ERROR] agent.client.memberlist.lan: memberlist: Conflicting address for consul-consul-server-0. Mine: 10.32.9.117:8301 Theirs: 10.32.9.118:8301 Old state: 1
    2021-01-23T23:34:06.204Z [WARN]  agent.client.serf.lan: serf: Name conflict for 'consul-consul-server-0' both 10.32.9.117:8301 and 10.32.9.118:8301 are claiming
    2021-01-23T23:34:06.205Z [ERROR] agent.client.memberlist.lan: memberlist: Conflicting address for consul-consul-server-0. Mine: 10.32.9.117:8301 Theirs: 10.32.9.118:8301 Old state: 1
    2021-01-23T23:34:06.205Z [WARN]  agent.client.serf.lan: serf: Name conflict for 'consul-consul-server-0' both 10.32.9.117:8301 and 10.32.9.118:8301 are claiming
    2021-01-23T23:34:07.655Z [INFO]  agent.client.memberlist.lan: memberlist: Marking consul-consul-server-0 as failed, suspect timeout reached (2 peer confirmations)
    2021-01-23T23:34:07.656Z [INFO]  agent.client.serf.lan: serf: EventMemberFailed: consul-consul-server-0 10.32.9.117
    2021-01-23T23:34:07.656Z [INFO]  agent.client: removing server: server="consul-consul-server-0 (Addr: tcp/10.32.9.117:8300) (DC: dc1)"
    2021-01-23T23:34:17.216Z [INFO]  agent.client.serf.lan: serf: EventMemberLeave (forced): consul-consul-server-0 10.32.9.117
    2021-01-23T23:34:17.216Z [INFO]  agent.client: removing server: server="consul-consul-server-0 (Addr: tcp/10.32.9.117:8300) (DC: dc1)"
    2021-01-23T23:34:42.907Z [INFO]  agent.client.serf.lan: serf: EventMemberJoin: consul-consul-server-0 10.32.9.118
    2021-01-23T23:34:42.907Z [INFO]  agent.client: adding server: server="consul-consul-server-0 (Addr: tcp/10.32.9.118:8300) (DC: dc1)"
    2021-01-23T23:34:51.216Z [INFO]  agent.client: New leader elected: payload=consul-consul-server-2
    2021-01-23T23:34:57.313Z [INFO]  agent.client.serf.lan: serf: EventMemberUpdate: consul-consul-server-0
    2021-01-23T23:34:57.313Z [INFO]  agent.client: updating server: server="consul-consul-server-0 (Addr: tcp/10.32.9.118:8300) (DC: dc1)"
