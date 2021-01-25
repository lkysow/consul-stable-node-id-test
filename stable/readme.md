Install with ACLs and stable-node-id helm chart

    k exec consul-consul-server-0 -- consul members
    Node                                     Address           Status  Type    Build  Protocol  DC   Segment
    consul-consul-server-0                   10.32.9.122:8301  alive   server  1.9.2  2         dc1  <all>
    consul-consul-server-1                   10.32.4.59:8301   alive   server  1.9.2  2         dc1  <all>
    consul-consul-server-2                   10.32.3.60:8301   alive   server  1.9.2  2         dc1  <all>
    gke-consul-1-default-pool-51946b93-9n7d  10.32.4.58:8301   alive   client  1.9.2  2         dc1  <default>
    gke-consul-1-default-pool-51946b93-cohw  10.32.9.119:8301  alive   client  1.9.2  2         dc1  <default>
    gke-consul-1-default-pool-51946b93-dous  10.32.3.59:8301   alive   client  1.9.2  2         dc1  <default>
    gke-consul-1-default-pool-51946b93-qcpt  10.32.7.31:8301   alive   client  1.9.2  2         dc1  <default>

    k exec consul-consul-server-0 -- consul members -detailed
    Node                                     Address           Status  Tags
    consul-consul-server-0                   10.32.9.122:8301  alive   acls=1,build=1.9.2:6530cf37,dc=dc1,expect=3,ft_fs=1,ft_si=1,id=df0703c1-7018-4060-0533-43d6212fe493,port=8300,raft_vsn=3,role=consul,segment=<all>,vsn=2,vsn_max=3,vsn_min=2,wan_join_port=8302
    consul-consul-server-1                   10.32.4.59:8301   alive   acls=1,build=1.9.2:6530cf37,dc=dc1,expect=3,ft_fs=1,ft_si=1,id=a05729fb-0571-3a30-3b63-d1c587ddc071,port=8300,raft_vsn=3,role=consul,segment=<all>,vsn=2,vsn_max=3,vsn_min=2,wan_join_port=8302
    consul-consul-server-2                   10.32.3.60:8301   alive   acls=1,build=1.9.2:6530cf37,dc=dc1,expect=3,ft_fs=1,ft_si=1,id=9fd19fd0-9b95-99dd-7900-b2caae48600e,port=8300,raft_vsn=3,role=consul,segment=<all>,vsn=2,vsn_max=3,vsn_min=2,wan_join_port=8302
    gke-consul-1-default-pool-51946b93-9n7d  10.32.4.58:8301   alive   acls=1,build=1.9.2:6530cf37,dc=dc1,id=aecc44fb-6d90-d399-9f39-3a80a07b2ad4,role=node,segment=<default>,vsn=2,vsn_max=3,vsn_min=2
    gke-consul-1-default-pool-51946b93-cohw  10.32.9.119:8301  alive   acls=1,build=1.9.2:6530cf37,dc=dc1,id=2eeac890-edf8-8947-683a-56b125746442,role=node,segment=<default>,vsn=2,vsn_max=3,vsn_min=2
    gke-consul-1-default-pool-51946b93-dous  10.32.3.59:8301   alive   acls=1,build=1.9.2:6530cf37,dc=dc1,id=60f93122-543d-1c8a-6266-6cd9896afe59,role=node,segment=<default>,vsn=2,vsn_max=3,vsn_min=2
    gke-consul-1-default-pool-51946b93-qcpt  10.32.7.31:8301   alive   acls=1,build=1.9.2:6530cf37,dc=dc1,id=e163783c-4623-47cd-a13a-d4dbbed5a9b6,role=node,segment=<default>,vsn=2,vsn_max=3,vsn_min=2

## Force Delete A Client

    kubectl delete --force --grace-period=0 pod consul-consul-68jps

New client pod:

    2021-01-23T23:51:49.543Z [WARN]  agent.router.manager: No servers available
    2021-01-23T23:51:49.543Z [ERROR] agent.anti_entropy: failed to sync remote state: error="No known Consul servers"
    2021-01-23T23:51:49.687Z [INFO]  agent.client.serf.lan: serf: EventMemberJoin: consul-consul-server-1 10.32.4.59
    2021-01-23T23:51:49.687Z [INFO]  agent.client.serf.lan: serf: EventMemberJoin: consul-consul-server-0 10.32.9.122
    2021-01-23T23:51:49.687Z [INFO]  agent.client: adding server: server="consul-consul-server-1 (Addr: tcp/10.32.4.59:8300) (DC: dc1)"
    2021-01-23T23:51:49.687Z [INFO]  agent.client: adding server: server="consul-consul-server-0 (Addr: tcp/10.32.9.122:8300) (DC: dc1)"
    2021-01-23T23:51:49.687Z [INFO]  agent.client.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-9n7d 10.32.4.58
    2021-01-23T23:51:49.687Z [INFO]  agent.client.serf.lan: serf: EventMemberJoin: consul-consul-server-2 10.32.3.60
    2021-01-23T23:51:49.687Z [INFO]  agent.client.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-qcpt 10.32.7.31
    2021-01-23T23:51:49.687Z [INFO]  agent.client: adding server: server="consul-consul-server-2 (Addr: tcp/10.32.3.60:8300) (DC: dc1)"
    2021-01-23T23:51:49.687Z [INFO]  agent.client.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-cohw 10.32.9.119
    2021-01-23T23:51:49.687Z [WARN]  agent.client.memberlist.lan: memberlist: Refuting a suspect message (from: gke-consul-1-default-pool-51946b93-dous)
    2021-01-23T23:51:49.735Z [INFO]  agent.client.serf.lan: serf: EventMemberUpdate: gke-consul-1-default-pool-51946b93-dous
    2021-01-23T23:51:49.937Z [INFO]  agent: (LAN) joined: number_of_nodes=3
    2021-01-23T23:51:49.937Z [INFO]  agent: Join cluster completed. Synced with initial agents: cluster=LAN num_agents=3
    2021-01-23T23:51:50.457Z [INFO]  agent: Synced node info

Server:

    2021-01-23T23:51:49.686Z [ERROR] agent.server.memberlist.lan: memberlist: Conflicting address for gke-consul-1-default-pool-51946b93-dous. Mine: 10.32.3.59:8301 Theirs: 10.32.3.61:8301 Old state: 1
    2021-01-23T23:51:49.686Z [WARN]  agent.server.serf.lan: serf: Name conflict for 'gke-consul-1-default-pool-51946b93-dous' both 10.32.3.59:8301 and 10.32.3.61:8301 are claiming
    2021-01-23T23:51:51.609Z [INFO]  agent.server.serf.lan: serf: EventMemberFailed: gke-consul-1-default-pool-51946b93-dous 10.32.3.59
    2021-01-23T23:51:51.609Z [INFO]  agent.server: member failed, marking health critical: member=gke-consul-1-default-pool-51946b93-dous
    2021-01-23T23:52:22.930Z [INFO]  agent.server: member failed, marking health critical: member=gke-consul-1-default-pool-51946b93-dous
    2021-01-23T23:52:24.009Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-dous 10.32.3.61
    2021-01-23T23:52:24.009Z [INFO]  agent.server: member joined, marking health alive: member=gke-consul-1-default-pool-51946b93-dous

Other clients:

    2021-01-23T23:51:49.841Z [ERROR] agent.client.memberlist.lan: memberlist: Conflicting address for gke-consul-1-default-pool-51946b93-dous. Mine: 10.32.3.59:8301 Theirs: 10.32.3.61:8301 Old state: 1
    2021-01-23T23:51:49.841Z [WARN]  agent.client.serf.lan: serf: Name conflict for 'gke-consul-1-default-pool-51946b93-dous' both 10.32.3.59:8301 and 10.32.3.61:8301 are claiming
    2021-01-23T23:51:49.841Z [ERROR] agent.client.memberlist.lan: memberlist: Conflicting address for gke-consul-1-default-pool-51946b93-dous. Mine: 10.32.3.59:8301 Theirs: 10.32.3.61:8301 Old state: 1
    2021-01-23T23:51:49.841Z [WARN]  agent.client.serf.lan: serf: Name conflict for 'gke-consul-1-default-pool-51946b93-dous' both 10.32.3.59:8301 and 10.32.3.61:8301 are claiming
    2021-01-23T23:51:50.040Z [ERROR] agent.client.memberlist.lan: memberlist: Conflicting address for gke-consul-1-default-pool-51946b93-dous. Mine: 10.32.3.59:8301 Theirs: 10.32.3.61:8301 Old state: 1
    2021-01-23T23:51:50.040Z [WARN]  agent.client.serf.lan: serf: Name conflict for 'gke-consul-1-default-pool-51946b93-dous' both 10.32.3.59:8301 and 10.32.3.61:8301 are claiming
    2021-01-23T23:51:50.040Z [ERROR] agent.client.memberlist.lan: memberlist: Conflicting address for gke-consul-1-default-pool-51946b93-dous. Mine: 10.32.3.59:8301 Theirs: 10.32.3.61:8301 Old state: 1
    2021-01-23T23:51:50.040Z [WARN]  agent.client.serf.lan: serf: Name conflict for 'gke-consul-1-default-pool-51946b93-dous' both 10.32.3.59:8301 and 10.32.3.61:8301 are claiming
    2021-01-23T23:51:51.610Z [INFO]  agent.client.serf.lan: serf: EventMemberFailed: gke-consul-1-default-pool-51946b93-dous 10.32.3.59
    2021-01-23T23:52:23.748Z [INFO]  agent.client.memberlist.lan: memberlist: Updating address for left or failed node gke-consul-1-default-pool-51946b93-dous from 10.32.3.59:8301 to 10.32.3.61:8301
    2021-01-23T23:52:23.749Z [INFO]  agent.client.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-dous 10.32.3.61

## Graceful Delete A Client

New pod

    2021-01-23T23:58:51.436Z [INFO]  agent: Joining cluster...: cluster=LAN
    2021-01-23T23:58:51.436Z [INFO]  agent: (LAN) joining: lan_addresses=[consul-consul-server-0.consul-consul-server.default.svc:8301, consul-consul-server-1.consul-consul-server.default.svc:8301, consul-consul-server-2.consul-consul-server.default.svc:8301]
    2021-01-23T23:58:51.474Z [INFO]  agent.client.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-9n7d 10.32.4.58
    2021-01-23T23:58:51.474Z [INFO]  agent.client.serf.lan: serf: EventMemberJoin: consul-consul-server-0 10.32.9.122
    2021-01-23T23:58:51.474Z [WARN]  agent.client.memberlist.lan: memberlist: Refuting a dead message (from: gke-consul-1-default-pool-51946b93-dous)
    2021-01-23T23:58:51.474Z [INFO]  agent.client.serf.lan: serf: EventMemberJoin: consul-consul-server-1 10.32.4.59
    2021-01-23T23:58:51.474Z [INFO]  agent.client.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-cohw 10.32.9.119
    2021-01-23T23:58:51.534Z [INFO]  agent.client: adding server: server="consul-consul-server-0 (Addr: tcp/10.32.9.122:8300) (DC: dc1)"
    2021-01-23T23:58:51.535Z [INFO]  agent.client: adding server: server="consul-consul-server-1 (Addr: tcp/10.32.4.59:8300) (DC: dc1)"
    2021-01-23T23:58:51.535Z [INFO]  agent.client.serf.lan: serf: EventMemberJoin: consul-consul-server-2 10.32.3.60
    2021-01-23T23:58:51.535Z [INFO]  agent.client.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-qcpt 10.32.7.31
    2021-01-23T23:58:51.536Z [INFO]  agent.client: adding server: server="consul-consul-server-2 (Addr: tcp/10.32.3.60:8300) (DC: dc1)"
    2021-01-23T23:58:51.536Z [INFO]  agent.client.serf.lan: serf: EventMemberUpdate: gke-consul-1-default-pool-51946b93-dous
    2021-01-23T23:58:51.697Z [INFO]  agent: (LAN) joined: number_of_nodes=3
    2021-01-23T23:58:51.697Z [INFO]  agent: Join cluster completed. Synced with initial agents: cluster=LAN num_agents=3
    2021-01-23T23:58:52.571Z [INFO]  agent: Synced node info

Server logs

    2021-01-23T23:58:33.093Z [INFO]  agent.server.serf.lan: serf: EventMemberLeave: gke-consul-1-default-pool-51946b93-dous 10.32.3.61
    2021-01-23T23:58:33.093Z [INFO]  agent.server: deregistering member: member=gke-consul-1-default-pool-51946b93-dous reason=left
    2021-01-23T23:58:51.473Z [INFO]  agent.server.memberlist.lan: memberlist: Updating address for left or failed node gke-consul-1-default-pool-51946b93-dous from 10.32.3.61:8301 to 10.32.3.62:8301
    2021-01-23T23:58:51.473Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-dous 10.32.3.62
    2021-01-23T23:58:51.473Z [INFO]  agent.server: member joined, marking health alive: member=gke-consul-1-default-pool-51946b93-dous
    2021-01-23T23:58:51.548Z [INFO]  agent.server.serf.lan: serf: EventMemberUpdate: gke-consul-1-default-pool-51946b93-dous

Other client logs

    2021-01-23T23:58:33.210Z [INFO]  agent.client.serf.lan: serf: EventMemberLeave: gke-consul-1-default-pool-51946b93-dous 10.32.3.61
    2021-01-23T23:58:51.611Z [INFO]  agent.client.memberlist.lan: memberlist: Updating address for left or failed node gke-consul-1-default-pool-51946b93-dous from 10.32.3.61:8301 to 10.32.3.62:8301
    2021-01-23T23:58:51.611Z [INFO]  agent.client.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-dous 10.32.3.62

## Gracefully Delete Server

server-1

    2021-01-24T00:02:16.695Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.3.60:8300 leader=10.32.9.122:8300
    2021-01-24T00:02:18.340Z [INFO]  agent.server.serf.lan: serf: EventMemberFailed: consul-consul-server-0 10.32.9.122
    2021-01-24T00:02:18.340Z [INFO]  agent.server: Removing LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.122:8300) (DC: dc1)"
    2021-01-24T00:02:20.213Z [WARN]  agent.server.raft: heartbeat timeout reached, starting election: last-leader=10.32.9.122:8300
    2021-01-24T00:02:20.214Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.4.59:8300 [Candidate]" term=3
    2021-01-24T00:02:20.315Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=df0703c1-7018-4060-0533-43d6212fe493 fallback=10.32.9.122:8300 error="Could not find address for server id df0703c1-7018-4060-0533-43d6212fe493"
    2021-01-24T00:02:22.438Z [INFO]  agent.server.raft: entering follower state: follower="Node at 10.32.4.59:8300 [Follower]" leader=
    2021-01-24T00:02:22.610Z [INFO]  agent.server: New leader elected: payload=consul-consul-server-2
    2021-01-24T00:02:25.618Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-24T00:02:27.914Z [ERROR] agent.server.raft: failed to make requestVote RPC: target="{Voter df0703c1-7018-4060-0533-43d6212fe493 10.32.9.122:8300}" error="dial tcp <nil>->10.32.9.122:8300: connect: no route to host"
    2021-01-24T00:02:27.914Z [ERROR] agent.server.memberlist.wan: memberlist: Push/Pull with consul-consul-server-0.dc1 failed: dial tcp 10.32.9.122:8302: connect: no route to host
    2021-01-24T00:02:34.624Z [INFO]  agent.server.serf.lan: serf: EventMemberLeave (forced): consul-consul-server-0 10.32.9.122
    2021-01-24T00:02:34.624Z [INFO]  agent.server: Removing LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.122:8300) (DC: dc1)"
    2021-01-24T00:02:35.618Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-24T00:02:36.814Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.122:8302 Theirs: 10.32.9.123:8302 Old state: 1
    2021-01-24T00:02:36.814Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.122:8302 and 10.32.9.123:8302 are claiming
    2021-01-24T00:02:37.114Z [ERROR] agent.server.memberlist.lan: memberlist: Conflicting address for consul-consul-server-0. Mine: 10.32.9.122:8301 Theirs: 10.32.9.123:8301 Old state: 2
    2021-01-24T00:02:37.114Z [WARN]  agent.server.serf.lan: serf: Name conflict for 'consul-consul-server-0' both 10.32.9.122:8301 and 10.32.9.123:8301 are claiming
    2021-01-24T00:02:37.314Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.122:8302 Theirs: 10.32.9.123:8302 Old state: 1
    2021-01-24T00:02:37.314Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.122:8302 and 10.32.9.123:8302 are claiming
    2021-01-24T00:02:37.315Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.122:8302 Theirs: 10.32.9.123:8302 Old state: 1
    2021-01-24T00:02:37.315Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.122:8302 and 10.32.9.123:8302 are claiming
    2021-01-24T00:02:37.814Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.122:8302 Theirs: 10.32.9.123:8302 Old state: 1
    2021-01-24T00:02:37.814Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.122:8302 and 10.32.9.123:8302 are claiming
    2021-01-24T00:02:37.814Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.122:8302 Theirs: 10.32.9.123:8302 Old state: 1
    2021-01-24T00:02:37.814Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.122:8302 and 10.32.9.123:8302 are claiming
    2021-01-24T00:02:42.415Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.123:8300 leader=10.32.3.60:8300
    2021-01-24T00:02:46.549Z [INFO]  agent.server.memberlist.wan: memberlist: Marking consul-consul-server-0.dc1 as failed, suspect timeout reached (0 peer confirmations)
    2021-01-24T00:02:46.549Z [INFO]  agent.server.serf.wan: serf: EventMemberFailed: consul-consul-server-0.dc1 10.32.9.122
    2021-01-24T00:02:46.549Z [INFO]  agent.server: Handled event for server in area: event=member-failed server=consul-consul-server-0.dc1 area=wan
    2021-01-24T00:02:48.963Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.123:8300 leader=10.32.3.60:8300
    2021-01-24T00:02:50.618Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-24T00:02:55.747Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.123:8300 leader=10.32.3.60:8300
    2021-01-24T00:03:05.720Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.123:8300 leader=10.32.3.60:8300
    2021-01-24T00:03:13.256Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.123:8300 leader=10.32.3.60:8300
    2021-01-24T00:03:15.814Z [INFO]  agent.server.serf.wan: serf: attempting reconnect to consul-consul-server-0.dc1 10.32.9.122:8302
    2021-01-24T00:03:17.010Z [INFO]  agent.server: New leader elected: payload=consul-consul-server-2
    2021-01-24T00:03:27.117Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: consul-consul-server-0 10.32.9.123
    2021-01-24T00:03:27.117Z [INFO]  agent.server: Adding LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.123:8300) (DC: dc1)"
    2021-01-24T00:03:28.114Z [INFO]  agent.server.serf.lan: serf: EventMemberUpdate: consul-consul-server-0
    2021-01-24T00:03:28.114Z [INFO]  agent.server: Updating LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.123:8300) (DC: dc1)"
    2021-01-24T00:03:28.315Z [INFO]  agent.server.memberlist.wan: memberlist: Updating address for left or failed node consul-consul-server-0.dc1 from 10.32.9.122:8302 to 10.32.9.123:8302
    2021-01-24T00:03:28.315Z [INFO]  agent.server.serf.wan: serf: EventMemberJoin: consul-consul-server-0.dc1 10.32.9.123
    2021-01-24T00:03:28.315Z [INFO]  agent.server: Handled event for server in area: event=member-join server=consul-consul-server-0.dc1 area=wan

server-2

    2021-01-24T00:02:16.547Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-24T00:02:16.691Z [WARN]  agent.server.raft: heartbeat timeout reached, starting election: last-leader=10.32.9.122:8300
    2021-01-24T00:02:16.691Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.3.60:8300 [Candidate]" term=3
    2021-01-24T00:02:18.419Z [INFO]  agent.server.serf.lan: serf: EventMemberFailed: consul-consul-server-0 10.32.9.122
    2021-01-24T00:02:18.420Z [INFO]  agent.server: Removing LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.122:8300) (DC: dc1)"
    2021-01-24T00:02:18.550Z [INFO]  agent.server.memberlist.lan: memberlist: Suspect consul-consul-server-0 has failed, no acks received
    2021-01-24T00:02:20.315Z [INFO]  agent.server.raft: duplicate requestVote for same term: term=3
    2021-01-24T00:02:22.427Z [WARN]  agent.server.raft: Election timeout reached, restarting election
    2021-01-24T00:02:22.427Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.3.60:8300 [Candidate]" term=4
    2021-01-24T00:02:22.431Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=df0703c1-7018-4060-0533-43d6212fe493 fallback=10.32.9.122:8300 error="Could not find address for server id df0703c1-7018-4060-0533-43d6212fe493"
    2021-01-24T00:02:22.438Z [INFO]  agent.server.raft: election won: tally=2
    2021-01-24T00:02:22.438Z [INFO]  agent.server.raft: entering leader state: leader="Node at 10.32.3.60:8300 [Leader]"
    2021-01-24T00:02:22.438Z [INFO]  agent.server.raft: added peer, starting replication: peer=a05729fb-0571-3a30-3b63-d1c587ddc071
    2021-01-24T00:02:22.438Z [INFO]  agent.server.raft: added peer, starting replication: peer=df0703c1-7018-4060-0533-43d6212fe493
    2021-01-24T00:02:22.439Z [INFO]  agent.server: cluster leadership acquired
    2021-01-24T00:02:22.439Z [INFO]  agent.server: New leader elected: payload=consul-consul-server-2
    2021-01-24T00:02:22.439Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=df0703c1-7018-4060-0533-43d6212fe493 fallback=10.32.9.122:8300 error="Could not find address for server id df0703c1-7018-4060-0533-43d6212fe493"
    2021-01-24T00:02:22.443Z [INFO]  agent.server.raft: pipelining replication: peer="{Voter a05729fb-0571-3a30-3b63-d1c587ddc071 10.32.4.59:8300}"
    2021-01-24T00:02:22.446Z [INFO]  agent.server: initializing acls
    2021-01-24T00:02:22.446Z [INFO]  agent.leader: started routine: routine="legacy ACL token upgrade"
    2021-01-24T00:02:22.446Z [INFO]  agent.leader: started routine: routine="acl token reaping"
    2021-01-24T00:02:23.051Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=df0703c1-7018-4060-0533-43d6212fe493 fallback=10.32.9.122:8300 error="Could not find address for server id df0703c1-7018-4060-0533-43d6212fe493"
    2021-01-24T00:02:23.446Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="context deadline exceeded"
    2021-01-24T00:02:23.446Z [ERROR] agent.server.autopilot: Error when computing next state: error="context deadline exceeded"
    2021-01-24T00:02:23.446Z [INFO]  agent.leader: started routine: routine="federation state anti-entropy"
    2021-01-24T00:02:23.446Z [INFO]  agent.leader: started routine: routine="federation state pruning"
    2021-01-24T00:02:23.447Z [INFO]  agent.leader: started routine: routine="intermediate cert renew watch"
    2021-01-24T00:02:23.447Z [INFO]  agent.leader: started routine: routine="CA root pruning"
    2021-01-24T00:02:23.447Z [INFO]  agent.server: member failed, marking health critical: member=consul-consul-server-0
    2021-01-24T00:02:24.940Z [WARN]  agent.server.raft: failed to contact: server-id=df0703c1-7018-4060-0533-43d6212fe493 time=2.501968559s
    2021-01-24T00:02:26.447Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="context deadline exceeded"
    2021-01-24T00:02:26.547Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-24T00:02:26.694Z [ERROR] agent.server.raft: failed to make requestVote RPC: target="{Voter df0703c1-7018-4060-0533-43d6212fe493 10.32.9.122:8300}" error="dial tcp <nil>->10.32.9.122:8300: i/o timeout"
    2021-01-24T00:02:27.413Z [WARN]  agent.server.raft: failed to contact: server-id=df0703c1-7018-4060-0533-43d6212fe493 time=4.974148836s
    2021-01-24T00:02:27.857Z [ERROR] agent.server.raft: failed to heartbeat to: peer=10.32.9.122:8300 error="dial tcp <nil>->10.32.9.122:8300: connect: no route to host"
    2021-01-24T00:02:27.857Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Voter df0703c1-7018-4060-0533-43d6212fe493 10.32.9.122:8300}" error="dial tcp <nil>->10.32.9.122:8300: connect: no route to host"
    2021-01-24T00:02:27.857Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="rpc error getting client: failed to get conn: dial tcp <nil>->10.32.9.122:8300: connect: no route to host"
    2021-01-24T00:02:27.857Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="rpc error getting client: failed to get conn: rpc error: lead thread didn't get connection"
    2021-01-24T00:02:27.857Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="rpc error getting client: failed to get conn: rpc error: lead thread didn't get connection"
    2021-01-24T00:02:27.857Z [ERROR] agent.server.raft: failed to make requestVote RPC: target="{Voter df0703c1-7018-4060-0533-43d6212fe493 10.32.9.122:8300}" error="dial tcp <nil>->10.32.9.122:8300: connect: no route to host"
    2021-01-24T00:02:27.867Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=df0703c1-7018-4060-0533-43d6212fe493 fallback=10.32.9.122:8300 error="Could not find address for server id df0703c1-7018-4060-0533-43d6212fe493"
    2021-01-24T00:02:27.867Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=df0703c1-7018-4060-0533-43d6212fe493 fallback=10.32.9.122:8300 error="Could not find address for server id df0703c1-7018-4060-0533-43d6212fe493"
    2021-01-24T00:02:28.447Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="context deadline exceeded"
    2021-01-24T00:02:29.906Z [WARN]  agent.server.raft: failed to contact: server-id=df0703c1-7018-4060-0533-43d6212fe493 time=7.468073749s
    2021-01-24T00:02:30.447Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="context deadline exceeded"
    2021-01-24T00:02:30.927Z [ERROR] agent.server.raft: failed to heartbeat to: peer=10.32.9.122:8300 error="dial tcp <nil>->10.32.9.122:8300: connect: no route to host"
    2021-01-24T00:02:31.665Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=df0703c1-7018-4060-0533-43d6212fe493 fallback=10.32.9.122:8300 error="Could not find address for server id df0703c1-7018-4060-0533-43d6212fe493"
    2021-01-24T00:02:32.447Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="context deadline exceeded"
    2021-01-24T00:02:34.001Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Voter df0703c1-7018-4060-0533-43d6212fe493 10.32.9.122:8300}" error="dial tcp <nil>->10.32.9.122:8300: connect: no route to host"
    2021-01-24T00:02:34.001Z [ERROR] agent.server.raft: failed to heartbeat to: peer=10.32.9.122:8300 error="dial tcp <nil>->10.32.9.122:8300: connect: no route to host"
    2021-01-24T00:02:34.011Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=df0703c1-7018-4060-0533-43d6212fe493 fallback=10.32.9.122:8300 error="Could not find address for server id df0703c1-7018-4060-0533-43d6212fe493"
    2021-01-24T00:02:34.447Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="context deadline exceeded"
    2021-01-24T00:02:34.447Z [INFO]  agent.server.autopilot: Attempting removal of failed server node: id=df0703c1-7018-4060-0533-43d6212fe493 name=consul-consul-server-0 address=10.32.9.122:8300
    2021-01-24T00:02:34.447Z [INFO]  agent.server.serf.lan: serf: EventMemberLeave (forced): consul-consul-server-0 10.32.9.122
    2021-01-24T00:02:34.447Z [INFO]  agent.server: Removing LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.122:8300) (DC: dc1)"
    2021-01-24T00:02:34.447Z [INFO]  agent.server.raft: updating configuration: command=RemoveServer server-id=df0703c1-7018-4060-0533-43d6212fe493 server-addr= servers="[{Suffrage:Voter ID:a05729fb-0571-3a30-3b63-d1c587ddc071 Address:10.32.4.59:8300} {Suffrage:Voter ID:9fd19fd0-9b95-99dd-7900-b2caae48600e Address:10.32.3.60:8300}]"
    2021-01-24T00:02:34.449Z [INFO]  agent.server.raft: removed peer, stopping replication: peer=df0703c1-7018-4060-0533-43d6212fe493 last-index=187
    2021-01-24T00:02:34.452Z [INFO]  agent.server.autopilot: removed server: id=df0703c1-7018-4060-0533-43d6212fe493
    2021-01-24T00:02:34.452Z [INFO]  agent.server: deregistering member: member=consul-consul-server-0 reason=left
    2021-01-24T00:02:35.003Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=df0703c1-7018-4060-0533-43d6212fe493 fallback=10.32.9.122:8300 error="Could not find address for server id df0703c1-7018-4060-0533-43d6212fe493"
    2021-01-24T00:02:36.817Z [ERROR] agent.server.memberlist.lan: memberlist: Conflicting address for consul-consul-server-0. Mine: 10.32.9.122:8301 Theirs: 10.32.9.123:8301 Old state: 2
    2021-01-24T00:02:36.817Z [WARN]  agent.server.serf.lan: serf: Name conflict for 'consul-consul-server-0' both 10.32.9.122:8301 and 10.32.9.123:8301 are claiming
    2021-01-24T00:02:36.907Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.122:8302 Theirs: 10.32.9.123:8302 Old state: 1
    2021-01-24T00:02:36.907Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.122:8302 and 10.32.9.123:8302 are claiming
    2021-01-24T00:02:37.071Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Voter df0703c1-7018-4060-0533-43d6212fe493 10.32.9.122:8300}" error="dial tcp <nil>->10.32.9.122:8300: connect: no route to host"
    2021-01-24T00:02:37.092Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=df0703c1-7018-4060-0533-43d6212fe493 fallback=10.32.9.122:8300 error="Could not find address for server id df0703c1-7018-4060-0533-43d6212fe493"
    2021-01-24T00:02:37.129Z [ERROR] agent.server.memberlist.lan: memberlist: Conflicting address for consul-consul-server-0. Mine: 10.32.9.122:8301 Theirs: 10.32.9.123:8301 Old state: 2
    2021-01-24T00:02:37.129Z [WARN]  agent.server.serf.lan: serf: Name conflict for 'consul-consul-server-0' both 10.32.9.122:8301 and 10.32.9.123:8301 are claiming
    2021-01-24T00:02:37.310Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.122:8302 Theirs: 10.32.9.123:8302 Old state: 1
    2021-01-24T00:02:37.310Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.122:8302 and 10.32.9.123:8302 are claiming
    2021-01-24T00:02:37.310Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.122:8302 Theirs: 10.32.9.123:8302 Old state: 1
    2021-01-24T00:02:37.310Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.122:8302 and 10.32.9.123:8302 are claiming
    2021-01-24T00:02:37.808Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.122:8302 Theirs: 10.32.9.123:8302 Old state: 1
    2021-01-24T00:02:37.809Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.122:8302 and 10.32.9.123:8302 are claiming
    2021-01-24T00:02:37.809Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.122:8302 Theirs: 10.32.9.123:8302 Old state: 1
    2021-01-24T00:02:37.809Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.122:8302 and 10.32.9.123:8302 are claiming
    2021-01-24T00:02:39.447Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="rpc error getting client: failed to get conn: dial tcp <nil>->10.32.9.122:8300: i/o timeout"
    2021-01-24T00:02:39.447Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="rpc error getting client: failed to get conn: rpc error: lead thread didn't get connection"
    2021-01-24T00:02:39.447Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="rpc error getting client: failed to get conn: rpc error: lead thread didn't get connection"
    2021-01-24T00:02:40.143Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Voter df0703c1-7018-4060-0533-43d6212fe493 10.32.9.122:8300}" error="dial tcp <nil>->10.32.9.122:8300: connect: no route to host"
    2021-01-24T00:02:40.184Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=df0703c1-7018-4060-0533-43d6212fe493 fallback=10.32.9.122:8300 error="Could not find address for server id df0703c1-7018-4060-0533-43d6212fe493"
    2021-01-24T00:02:41.547Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-24T00:02:42.419Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.123:8300 leader=10.32.3.60:8300
    2021-01-24T00:02:43.215Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Voter df0703c1-7018-4060-0533-43d6212fe493 10.32.9.122:8300}" error="dial tcp <nil>->10.32.9.122:8300: connect: no route to host"
    2021-01-24T00:02:45.003Z [ERROR] agent.server.raft: failed to heartbeat to: peer=10.32.9.122:8300 error="dial tcp <nil>->10.32.9.122:8300: i/o timeout"
    2021-01-24T00:02:46.548Z [INFO]  agent.server.memberlist.wan: memberlist: Marking consul-consul-server-0.dc1 as failed, suspect timeout reached (0 peer confirmations)
    2021-01-24T00:02:46.548Z [INFO]  agent.server.serf.wan: serf: EventMemberFailed: consul-consul-server-0.dc1 10.32.9.122
    2021-01-24T00:02:46.548Z [INFO]  agent.server: Handled event for server in area: event=member-failed server=consul-consul-server-0.dc1 area=wan
    2021-01-24T00:02:48.968Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.123:8300 leader=10.32.3.60:8300
    2021-01-24T00:02:51.547Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-24T00:02:55.750Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.123:8300 leader=10.32.3.60:8300
    2021-01-24T00:03:05.724Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.123:8300 leader=10.32.3.60:8300
    2021-01-24T00:03:06.550Z [INFO]  agent.server.serf.wan: serf: attempting reconnect to consul-consul-server-0.dc1 10.32.9.122:8302
    2021-01-24T00:03:07.339Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: consul-consul-server-0 10.32.9.123
    2021-01-24T00:03:07.339Z [INFO]  agent.server: Adding LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.123:8300) (DC: dc1)"
    2021-01-24T00:03:07.340Z [INFO]  agent.server.raft: updating configuration: command=AddNonvoter server-id=df0703c1-7018-4060-0533-43d6212fe493 server-addr=10.32.9.123:8300 servers="[{Suffrage:Voter ID:a05729fb-0571-3a30-3b63-d1c587ddc071 Address:10.32.4.59:8300} {Suffrage:Voter ID:9fd19fd0-9b95-99dd-7900-b2caae48600e Address:10.32.3.60:8300} {Suffrage:Nonvoter ID:df0703c1-7018-4060-0533-43d6212fe493 Address:10.32.9.123:8300}]"
    2021-01-24T00:03:07.344Z [INFO]  agent.server.raft: added peer, starting replication: peer=df0703c1-7018-4060-0533-43d6212fe493
    2021-01-24T00:03:07.346Z [ERROR] agent.server.raft: peer has newer term, stopping replication: peer="{Nonvoter df0703c1-7018-4060-0533-43d6212fe493 10.32.9.123:8300}"
    2021-01-24T00:03:07.346Z [INFO]  agent.server.raft: entering follower state: follower="Node at 10.32.3.60:8300 [Follower]" leader=
    2021-01-24T00:03:07.346Z [INFO]  agent.server.raft: aborting pipeline replication: peer="{Voter a05729fb-0571-3a30-3b63-d1c587ddc071 10.32.4.59:8300}"
    2021-01-24T00:03:07.346Z [ERROR] agent.server.autopilot: failed to add raft non-voting peer: id=df0703c1-7018-4060-0533-43d6212fe493 address=10.32.9.123:8300 error="leadership lost while committing log"
    2021-01-24T00:03:07.346Z [ERROR] agent.server: failed to reconcile member: member="{consul-consul-server-0 10.32.9.123 8301 map[acls:2 build:1.9.2:6530cf37 dc:dc1 expect:3 ft_fs:1 ft_si:1 id:df0703c1-7018-4060-0533-43d6212fe493 port:8300 raft_vsn:3 role:consul segment: vsn:2 vsn_max:3 vsn_min:2 wan_join_port:8302] alive 1 5 2 2 5 4}" error="leadership lost while committing log"
    2021-01-24T00:03:07.347Z [INFO]  agent.server: cluster leadership lost
    2021-01-24T00:03:13.262Z [WARN]  agent.server.raft: rejecting vote request since our last term is greater: candidate=10.32.9.123:8300 last-term=4 last-candidate-term=2
    2021-01-24T00:03:16.854Z [WARN]  agent.server.raft: heartbeat timeout reached, starting election: last-leader=
    2021-01-24T00:03:16.854Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.3.60:8300 [Candidate]" term=9
    2021-01-24T00:03:16.864Z [INFO]  agent.server.raft: election won: tally=2
    2021-01-24T00:03:16.864Z [INFO]  agent.server.raft: entering leader state: leader="Node at 10.32.3.60:8300 [Leader]"
    2021-01-24T00:03:16.864Z [INFO]  agent.server.raft: added peer, starting replication: peer=a05729fb-0571-3a30-3b63-d1c587ddc071
    2021-01-24T00:03:16.864Z [INFO]  agent.server.raft: added peer, starting replication: peer=df0703c1-7018-4060-0533-43d6212fe493
    2021-01-24T00:03:16.865Z [INFO]  agent.server: cluster leadership acquired
    2021-01-24T00:03:16.865Z [INFO]  agent.server: New leader elected: payload=consul-consul-server-2
    2021-01-24T00:03:16.865Z [INFO]  agent.server.raft: pipelining replication: peer="{Voter a05729fb-0571-3a30-3b63-d1c587ddc071 10.32.4.59:8300}"
    2021-01-24T00:03:16.869Z [WARN]  agent.server.raft: appendEntries rejected, sending older logs: peer="{Nonvoter df0703c1-7018-4060-0533-43d6212fe493 10.32.9.123:8300}" next=182
    2021-01-24T00:03:16.870Z [INFO]  agent.server: initializing acls
    2021-01-24T00:03:16.870Z [INFO]  agent.leader: started routine: routine="legacy ACL token upgrade"
    2021-01-24T00:03:16.870Z [INFO]  agent.leader: started routine: routine="acl token reaping"
    2021-01-24T00:03:16.872Z [INFO]  agent.leader: started routine: routine="federation state anti-entropy"
    2021-01-24T00:03:16.872Z [INFO]  agent.leader: started routine: routine="federation state pruning"
    2021-01-24T00:03:16.873Z [INFO]  agent.leader: started routine: routine="intermediate cert renew watch"
    2021-01-24T00:03:16.873Z [INFO]  agent.leader: started routine: routine="CA root pruning"
    2021-01-24T00:03:16.873Z [INFO]  agent.server: member joined, marking health alive: member=consul-consul-server-0
    2021-01-24T00:03:16.934Z [INFO]  agent.server.raft: pipelining replication: peer="{Nonvoter df0703c1-7018-4060-0533-43d6212fe493 10.32.9.123:8300}"
    2021-01-24T00:03:26.874Z [INFO]  agent.server.autopilot: Promoting server: id=df0703c1-7018-4060-0533-43d6212fe493 address=10.32.9.123:8300 name=consul-consul-server-0
    2021-01-24T00:03:26.874Z [INFO]  agent.server.raft: updating configuration: command=AddStaging server-id=df0703c1-7018-4060-0533-43d6212fe493 server-addr=10.32.9.123:8300 servers="[{Suffrage:Voter ID:a05729fb-0571-3a30-3b63-d1c587ddc071 Address:10.32.4.59:8300} {Suffrage:Voter ID:9fd19fd0-9b95-99dd-7900-b2caae48600e Address:10.32.3.60:8300} {Suffrage:Voter ID:df0703c1-7018-4060-0533-43d6212fe493 Address:10.32.9.123:8300}]"
    2021-01-24T00:03:28.014Z [INFO]  agent.server.serf.lan: serf: EventMemberUpdate: consul-consul-server-0
    2021-01-24T00:03:28.014Z [INFO]  agent.server: Updating LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.123:8300) (DC: dc1)"
    2021-01-24T00:03:28.309Z [INFO]  agent.server.memberlist.wan: memberlist: Updating address for left or failed node consul-consul-server-0.dc1 from 10.32.9.122:8302 to 10.32.9.123:8302
    2021-01-24T00:03:28.310Z [INFO]  agent.server.serf.wan: serf: EventMemberJoin: consul-consul-server-0.dc1 10.32.9.123
    2021-01-24T00:03:28.310Z [INFO]  agent.server: Handled event for server in area: event=member-join server=consul-consul-server-0.dc1 area=wan

server-0 (coming up)

    2021-01-24T00:02:36.505Z [WARN]  agent: bootstrap_expect > 0: expecting 3 servers
    2021-01-24T00:02:36.605Z [WARN]  agent.auto_config: bootstrap_expect > 0: expecting 3 servers
    2021-01-24T00:02:36.804Z [INFO]  agent.server.raft: initial configuration: index=1 servers="[{Suffrage:Voter ID:a05729fb-0571-3a30-3b63-d1c587ddc071 Address:10.32.4.59:8300} {Suffrage:Voter ID:df0703c1-7018-4060-0533-43d6212fe493 Address:10.32.9.122:8300} {Suffrage:Voter ID:9fd19fd0-9b95-99dd-7900-b2caae48600e Address:10.32.3.60:8300}]"
    2021-01-24T00:02:36.806Z [INFO]  agent.server.raft: entering follower state: follower="Node at 10.32.9.123:8300 [Follower]" leader=
    2021-01-24T00:02:36.807Z [INFO]  agent.server.serf.wan: serf: EventMemberJoin: consul-consul-server-0.dc1 10.32.9.123
    2021-01-24T00:02:36.807Z [INFO]  agent.server.serf.wan: serf: Attempting re-join to previously known node: consul-consul-server-1.dc1: 10.32.4.59:8302
    2021-01-24T00:02:36.812Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: consul-consul-server-0 10.32.9.123
    2021-01-24T00:02:36.812Z [INFO]  agent.router: Initializing LAN area manager
    2021-01-24T00:02:36.812Z [INFO]  agent.server: Handled event for server in area: event=member-join server=consul-consul-server-0.dc1 area=wan
    2021-01-24T00:02:36.812Z [INFO]  agent: Started DNS server: address=0.0.0.0:8600 network=udp
    2021-01-24T00:02:36.813Z [INFO]  agent: Started DNS server: address=0.0.0.0:8600 network=tcp
    2021-01-24T00:02:36.813Z [INFO]  agent.server: Adding LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.123:8300) (DC: dc1)"
    2021-01-24T00:02:36.813Z [INFO]  agent.server: Raft data found, disabling bootstrap mode
    2021-01-24T00:02:36.813Z [INFO]  agent.server.serf.lan: serf: Attempting re-join to previously known node: consul-consul-server-2: 10.32.3.60:8301
    2021-01-24T00:02:36.814Z [INFO]  agent.server.serf.wan: serf: EventMemberJoin: consul-consul-server-1.dc1 10.32.4.59
    2021-01-24T00:02:36.814Z [INFO]  agent: Starting server: address=[::]:8500 network=tcp protocol=http
    2021-01-24T00:02:36.902Z [WARN]  agent: DEPRECATED Backwards compatibility with pre-1.9 metrics enabled. These metrics will be removed in a future version of Consul. Set `telemetry { disable_compat_1.9 = true }` to disable them.
    2021-01-24T00:02:36.902Z [WARN]  agent.server.memberlist.wan: memberlist: Refuting a suspect message (from: consul-consul-server-0.dc1)
    2021-01-24T00:02:36.902Z [INFO]  agent: started state syncer
    ==> Consul agent running!
    2021-01-24T00:02:36.817Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-dous 10.32.3.62
    2021-01-24T00:02:36.903Z [WARN]  agent.server.memberlist.lan: memberlist: Refuting a suspect message (from: consul-consul-server-0)
    2021-01-24T00:02:36.903Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-qcpt 10.32.7.31
    2021-01-24T00:02:36.903Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-9n7d 10.32.4.58
    2021-01-24T00:02:36.903Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: consul-consul-server-1 10.32.4.59
    2021-01-24T00:02:36.903Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-cohw 10.32.9.119
    2021-01-24T00:02:36.904Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: consul-consul-server-2 10.32.3.60
    2021-01-24T00:02:36.904Z [INFO]  agent.server.serf.lan: serf: Re-joined to previously known node: consul-consul-server-2: 10.32.3.60:8301
    2021-01-24T00:02:36.903Z [INFO]  agent: Retry join is supported for the following discovery methods: cluster=LAN discovery_methods="aliyun aws azure digitalocean gce k8s linode mdns os packet scaleway softlayer tencentcloud triton vsphere"
    2021-01-24T00:02:36.904Z [INFO]  agent: Joining cluster...: cluster=LAN
    2021-01-24T00:02:36.904Z [INFO]  agent: (LAN) joining: lan_addresses=[consul-consul-server-0.consul-consul-server.default.svc:8301, consul-consul-server-1.consul-consul-server.default.svc:8301, consul-consul-server-2.consul-consul-server.default.svc:8301]
    2021-01-24T00:02:36.904Z [INFO]  agent.server: Adding LAN server: server="consul-consul-server-1 (Addr: tcp/10.32.4.59:8300) (DC: dc1)"
    2021-01-24T00:02:36.904Z [INFO]  agent.server: Adding LAN server: server="consul-consul-server-2 (Addr: tcp/10.32.3.60:8300) (DC: dc1)"
    2021-01-24T00:02:36.904Z [INFO]  agent.server: New leader elected: payload=consul-consul-server-2
    2021-01-24T00:02:36.903Z [INFO]  agent.server: Handled event for server in area: event=member-join server=consul-consul-server-1.dc1 area=wan
    2021-01-24T00:02:36.904Z [INFO]  agent.server.serf.wan: serf: EventMemberJoin: consul-consul-server-2.dc1 10.32.3.60
    2021-01-24T00:02:36.905Z [INFO]  agent.server.serf.wan: serf: Re-joined to previously known node: consul-consul-server-1.dc1: 10.32.4.59:8302
    2021-01-24T00:02:36.905Z [INFO]  agent.server: Handled event for server in area: event=member-join server=consul-consul-server-2.dc1 area=wan
    2021-01-24T00:02:37.129Z [INFO]  agent: (LAN) joined: number_of_nodes=3
    2021-01-24T00:02:37.129Z [INFO]  agent: Join cluster completed. Synced with initial agents: cluster=LAN num_agents=3
    2021-01-24T00:02:42.409Z [WARN]  agent.server.raft: heartbeat timeout reached, starting election: last-leader=
    2021-01-24T00:02:42.409Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.9.123:8300 [Candidate]" term=3
    2021-01-24T00:02:42.419Z [INFO]  agent.server.raft: entering follower state: follower="Node at 10.32.9.123:8300 [Follower]" leader=
    2021-01-24T00:02:43.952Z [ERROR] agent.anti_entropy: failed to sync remote state: error="No cluster leader"
    2021-01-24T00:02:48.958Z [WARN]  agent.server.raft: heartbeat timeout reached, starting election: last-leader=
    2021-01-24T00:02:48.958Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.9.123:8300 [Candidate]" term=5
    2021-01-24T00:02:53.525Z [ERROR] agent.anti_entropy: failed to sync remote state: error="No cluster leader"
    2021-01-24T00:02:55.742Z [WARN]  agent.server.raft: Election timeout reached, restarting election
    2021-01-24T00:02:55.742Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.9.123:8300 [Candidate]" term=6
    2021-01-24T00:03:05.715Z [WARN]  agent.server.raft: Election timeout reached, restarting election
    2021-01-24T00:03:05.715Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.9.123:8300 [Candidate]" term=7
    2021-01-24T00:03:13.251Z [WARN]  agent.server.raft: Election timeout reached, restarting election
    2021-01-24T00:03:13.251Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.9.123:8300 [Candidate]" term=8
    2021-01-24T00:03:13.907Z [ERROR] agent: Coordinate update error: error="No cluster leader"
    2021-01-24T00:03:16.867Z [WARN]  agent.server.raft: failed to get previous log: previous-index=195 last-index=181 error="log not found"
    2021-01-24T00:03:16.868Z [INFO]  agent.server.raft: entering follower state: follower="Node at 10.32.9.123:8300 [Follower]" leader=10.32.3.60:8300
    2021-01-24T00:03:17.102Z [INFO]  agent.server: New leader elected: payload=consul-consul-server-2
    2021-01-24T00:03:17.114Z [INFO]  agent: Synced node info
    2021-01-24T00:03:28.004Z [INFO]  agent.server.serf.lan: serf: EventMemberUpdate: consul-consul-server-0
    2021-01-24T00:03:28.005Z [INFO]  agent.server: Updating LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.123:8300) (DC: dc1)"
    2021-01-24T00:03:28.212Z [INFO]  agent.server.serf.wan: serf: EventMemberUpdate: consul-consul-server-0.dc1
    2021-01-24T00:03:28.213Z [INFO]  agent.server: Handled event for server in area: event=member-update server=consul-consul-server-0.dc1 area=wan

client logs

    2021-01-24T00:02:18.410Z [INFO]  agent.client.serf.lan: serf: EventMemberFailed: consul-consul-server-0 10.32.9.122
    2021-01-24T00:02:18.410Z [INFO]  agent.client: removing server: server="consul-consul-server-0 (Addr: tcp/10.32.9.122:8300) (DC: dc1)"
    2021-01-24T00:02:22.548Z [INFO]  agent.client: New leader elected: payload=consul-consul-server-2
    2021-01-24T00:02:34.622Z [INFO]  agent.client.serf.lan: serf: EventMemberLeave (forced): consul-consul-server-0 10.32.9.122
    2021-01-24T00:02:34.622Z [INFO]  agent.client: removing server: server="consul-consul-server-0 (Addr: tcp/10.32.9.122:8300) (DC: dc1)"
    2021-01-24T00:03:07.004Z [INFO]  agent.client.serf.lan: serf: EventMemberJoin: consul-consul-server-0 10.32.9.123
    2021-01-24T00:03:07.004Z [INFO]  agent.client: adding server: server="consul-consul-server-0 (Addr: tcp/10.32.9.123:8300) (DC: dc1)"
    2021-01-24T00:03:16.948Z [INFO]  agent.client: New leader elected: payload=consul-consul-server-2
    2021-01-24T00:03:28.339Z [INFO]  agent.client.serf.lan: serf: EventMemberUpdate: consul-consul-server-0
    2021-01-24T00:03:28.339Z [INFO]  agent.client: updating server: server="consul-consul-server-0 (Addr: tcp/10.32.9.123:8300) (DC: dc1)"

## Force Delete Server

Server-0 logs (coming up)

    2021-01-24T00:06:36.807Z [INFO]  agent: Retry join is supported for the following discovery methods: cluster=LAN discovery_methods="aliyun aws azure digitalocean gce k8s linode mdns os packet scaleway softlayer tencentcloud triton vsphere"
    2021-01-24T00:06:36.807Z [INFO]  agent: Joining cluster...: cluster=LAN
    2021-01-24T00:06:36.807Z [INFO]  agent: (LAN) joining: lan_addresses=[consul-consul-server-0.consul-consul-server.default.svc:8301, consul-consul-server-1.consul-consul-server.default.svc:8301, consul-consul-server-2.consul-consul-server.default.svc:8301]
    2021-01-24T00:06:36.815Z [INFO]  agent.server.serf.wan: serf: EventMemberJoin: consul-consul-server-2.dc1 10.32.3.60
    2021-01-24T00:06:36.816Z [INFO]  agent.server: Handled event for server in area: event=member-join server=consul-consul-server-2.dc1 area=wan
    2021-01-24T00:06:36.816Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.124:8302 Theirs: 10.32.9.123:8302 Old state: 0
    2021-01-24T00:06:36.816Z [ERROR] agent.server.serf.wan: serf: Node name conflicts with another node at 10.32.9.123:8302. Names must be unique! (Resolution enabled: false)
    2021-01-24T00:06:36.816Z [INFO]  agent.server.serf.wan: serf: EventMemberJoin: consul-consul-server-1.dc1 10.32.4.59
    2021-01-24T00:06:36.816Z [INFO]  agent.server: Handled event for server in area: event=member-join server=consul-consul-server-1.dc1 area=wan
    2021-01-24T00:06:36.816Z [INFO]  agent.server.serf.wan: serf: Re-joined to previously known node: consul-consul-server-1.dc1: 10.32.4.59:8302
    2021-01-24T00:06:36.818Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-qcpt 10.32.7.31
    2021-01-24T00:06:36.818Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: consul-consul-server-1 10.32.4.59
    2021-01-24T00:06:36.818Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-cohw 10.32.9.119
    2021-01-24T00:06:36.818Z [INFO]  agent.server: Adding LAN server: server="consul-consul-server-1 (Addr: tcp/10.32.4.59:8300) (DC: dc1)"
    2021-01-24T00:06:36.818Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: consul-consul-server-2 10.32.3.60
    2021-01-24T00:06:36.902Z [WARN]  agent.server.memberlist.lan: memberlist: Refuting a suspect message (from: consul-consul-server-0)
    2021-01-24T00:06:36.902Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-dous 10.32.3.62
    2021-01-24T00:06:36.902Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: gke-consul-1-default-pool-51946b93-9n7d 10.32.4.58
    2021-01-24T00:06:36.902Z [INFO]  agent.server.serf.lan: serf: Re-joined to previously known node: consul-consul-server-1: 10.32.4.59:8301
    2021-01-24T00:06:36.903Z [INFO]  agent.server: Adding LAN server: server="consul-consul-server-2 (Addr: tcp/10.32.3.60:8300) (DC: dc1)"
    2021-01-24T00:06:37.107Z [INFO]  agent: (LAN) joined: number_of_nodes=3
    2021-01-24T00:06:37.107Z [INFO]  agent: Join cluster completed. Synced with initial agents: cluster=LAN num_agents=3
    2021-01-24T00:06:43.835Z [ERROR] agent.anti_entropy: failed to sync remote state: error="No cluster leader"
    2021-01-24T00:06:45.407Z [WARN]  agent.server.raft: heartbeat timeout reached, starting election: last-leader=
    2021-01-24T00:06:45.407Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.9.124:8300 [Candidate]" term=10
    2021-01-24T00:06:53.138Z [WARN]  agent.server.raft: Election timeout reached, restarting election
    2021-01-24T00:06:53.138Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.9.124:8300 [Candidate]" term=11
    2021-01-24T00:07:01.342Z [WARN]  agent.server.raft: Election timeout reached, restarting election
    2021-01-24T00:07:01.342Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.9.124:8300 [Candidate]" term=12
    2021-01-24T00:07:11.040Z [WARN]  agent.server.raft: Election timeout reached, restarting election
    2021-01-24T00:07:11.040Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.9.124:8300 [Candidate]" term=13
    2021-01-24T00:07:12.902Z [ERROR] agent: Coordinate update error: error="No cluster leader"
    2021-01-24T00:07:14.904Z [ERROR] agent.anti_entropy: failed to sync remote state: error="No cluster leader"
    2021-01-24T00:07:20.297Z [WARN]  agent.server.raft: Election timeout reached, restarting election
    2021-01-24T00:07:20.297Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.9.124:8300 [Candidate]" term=14
    2021-01-24T00:07:29.596Z [WARN]  agent.server.raft: Election timeout reached, restarting election
    2021-01-24T00:07:29.596Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.9.124:8300 [Candidate]" term=15
    2021-01-24T00:07:38.386Z [WARN]  agent.server.raft: Election timeout reached, restarting election
    2021-01-24T00:07:38.386Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.9.124:8300 [Candidate]" term=16
    2021-01-24T00:07:40.237Z [ERROR] agent.anti_entropy: failed to sync remote state: error="No cluster leader"
    2021-01-24T00:07:44.736Z [WARN]  agent.server.raft: Election timeout reached, restarting election
    2021-01-24T00:07:44.736Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.9.124:8300 [Candidate]" term=17
    2021-01-24T00:07:46.417Z [ERROR] agent: Coordinate update error: error="No cluster leader"
    2021-01-24T00:07:46.807Z [INFO]  agent.server: New leader elected: payload=consul-consul-server-2
    2021-01-24T00:07:46.809Z [WARN]  agent.server.raft: failed to get previous log: previous-index=251 last-index=237 error="log not found"
    2021-01-24T00:07:46.809Z [INFO]  agent.server.raft: entering follower state: follower="Node at 10.32.9.124:8300 [Follower]" leader=10.32.3.60:8300
    2021-01-24T00:07:47.202Z [INFO]  agent: Synced node info
    2021-01-24T00:07:58.009Z [INFO]  agent.server.serf.lan: serf: EventMemberUpdate: consul-consul-server-0
    2021-01-24T00:07:58.010Z [INFO]  agent.server: Updating LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.124:8300) (DC: dc1)"
    2021-01-24T00:07:58.405Z [INFO]  agent.server.serf.wan: serf: EventMemberUpdate: consul-consul-server-0.dc1
    2021-01-24T00:07:58.405Z [INFO]  agent.server: Handled event for server in area: event=member-update server=consul-consul-server-0.dc1 area=wan

server-1 logs

    2021-01-24T00:06:34.619Z [INFO]  agent.server.memberlist.lan: memberlist: Suspect consul-consul-server-0 has failed, no acks received
    2021-01-24T00:06:36.816Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.123:8302 Theirs: 10.32.9.124:8302 Old state: 0
    2021-01-24T00:06:36.816Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.123:8302 and 10.32.9.124:8302 are claiming
    2021-01-24T00:06:36.818Z [ERROR] agent.server.memberlist.lan: memberlist: Conflicting address for consul-consul-server-0. Mine: 10.32.9.123:8301 Theirs: 10.32.9.124:8301 Old state: 1
    2021-01-24T00:06:36.818Z [WARN]  agent.server.serf.lan: serf: Name conflict for 'consul-consul-server-0' both 10.32.9.123:8301 and 10.32.9.124:8301 are claiming
    2021-01-24T00:06:37.014Z [ERROR] agent.server.memberlist.lan: memberlist: Conflicting address for consul-consul-server-0. Mine: 10.32.9.123:8301 Theirs: 10.32.9.124:8301 Old state: 1
    2021-01-24T00:06:37.014Z [WARN]  agent.server.serf.lan: serf: Name conflict for 'consul-consul-server-0' both 10.32.9.123:8301 and 10.32.9.124:8301 are claiming
    2021-01-24T00:06:37.014Z [ERROR] agent.server.memberlist.lan: memberlist: Conflicting address for consul-consul-server-0. Mine: 10.32.9.123:8301 Theirs: 10.32.9.124:8301 Old state: 1
    2021-01-24T00:06:37.014Z [WARN]  agent.server.serf.lan: serf: Name conflict for 'consul-consul-server-0' both 10.32.9.123:8301 and 10.32.9.124:8301 are claiming
    2021-01-24T00:06:37.015Z [ERROR] agent.server.memberlist.lan: memberlist: Conflicting address for consul-consul-server-0. Mine: 10.32.9.123:8301 Theirs: 10.32.9.124:8301 Old state: 1
    2021-01-24T00:06:37.015Z [WARN]  agent.server.serf.lan: serf: Name conflict for 'consul-consul-server-0' both 10.32.9.123:8301 and 10.32.9.124:8301 are claiming
    2021-01-24T00:06:37.314Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.123:8302 Theirs: 10.32.9.124:8302 Old state: 0
    2021-01-24T00:06:37.314Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.123:8302 and 10.32.9.124:8302 are claiming
    2021-01-24T00:06:37.814Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.123:8302 Theirs: 10.32.9.124:8302 Old state: 0
    2021-01-24T00:06:37.814Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.123:8302 and 10.32.9.124:8302 are claiming
    2021-01-24T00:06:38.539Z [INFO]  agent.server.memberlist.lan: memberlist: Marking consul-consul-server-0 as failed, suspect timeout reached (2 peer confirmations)
    2021-01-24T00:06:38.540Z [INFO]  agent.server.serf.lan: serf: EventMemberFailed: consul-consul-server-0 10.32.9.123
    2021-01-24T00:06:38.543Z [INFO]  agent.server: Removing LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.123:8300) (DC: dc1)"
    2021-01-24T00:06:40.618Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-24T00:06:45.414Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.124:8300 leader=10.32.3.60:8300
    2021-01-24T00:06:45.819Z [INFO]  agent.server.serf.lan: serf: attempting reconnect to consul-consul-server-0 10.32.9.123:8301
    2021-01-24T00:06:48.149Z [INFO]  agent.server.serf.lan: serf: EventMemberLeave (forced): consul-consul-server-0 10.32.9.123
    2021-01-24T00:06:48.149Z [INFO]  agent.server: Removing LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.123:8300) (DC: dc1)"
    2021-01-24T00:06:50.618Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-24T00:06:53.143Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.124:8300 leader=10.32.3.60:8300
    2021-01-24T00:07:00.619Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-24T00:07:01.350Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.124:8300 leader=10.32.3.60:8300
    2021-01-24T00:07:10.619Z [INFO]  agent.server.memberlist.wan: memberlist: Marking consul-consul-server-0.dc1 as failed, suspect timeout reached (0 peer confirmations)
    2021-01-24T00:07:10.619Z [INFO]  agent.server.serf.wan: serf: EventMemberLeave: consul-consul-server-0.dc1 10.32.9.123
    2021-01-24T00:07:10.619Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-24T00:07:10.714Z [INFO]  agent.server: Handled event for server in area: event=member-leave server=consul-consul-server-0.dc1 area=wan
    2021-01-24T00:07:11.046Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.124:8300 leader=10.32.3.60:8300
    2021-01-24T00:07:20.305Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.124:8300 leader=10.32.3.60:8300
    2021-01-24T00:07:29.600Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.124:8300 leader=10.32.3.60:8300
    2021-01-24T00:07:37.620Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: consul-consul-server-0 10.32.9.124
    2021-01-24T00:07:37.620Z [INFO]  agent.server: Adding LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.124:8300) (DC: dc1)"
    2021-01-24T00:07:38.391Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.124:8300 leader=10.32.3.60:8300
    2021-01-24T00:07:44.741Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.124:8300 leader=10.32.3.60:8300
    2021-01-24T00:07:47.006Z [INFO]  agent.server: New leader elected: payload=consul-consul-server-2
    2021-01-24T00:07:58.206Z [INFO]  agent.server.serf.lan: serf: EventMemberUpdate: consul-consul-server-0
    2021-01-24T00:07:58.206Z [INFO]  agent.server: Updating LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.124:8300) (DC: dc1)"
    2021-01-24T00:07:58.805Z [INFO]  agent.server.memberlist.wan: memberlist: Updating address for left or failed node consul-consul-server-0.dc1 from 10.32.9.123:8302 to 10.32.9.124:8302
    2021-01-24T00:07:58.805Z [INFO]  agent.server.serf.wan: serf: EventMemberJoin: consul-consul-server-0.dc1 10.32.9.124
    2021-01-24T00:07:58.806Z [INFO]  agent.server: Handled event for server in area: event=member-join server=consul-consul-server-0.dc1 area=wan

server-2 logs

    2021-01-24T00:06:33.406Z [INFO]  agent.server.raft: aborting pipeline replication: peer="{Nonvoter df0703c1-7018-4060-0533-43d6212fe493 10.32.9.123:8300}"
    2021-01-24T00:06:33.490Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Nonvoter df0703c1-7018-4060-0533-43d6212fe493 10.32.9.123:8300}" error=EOF
    2021-01-24T00:06:33.501Z [ERROR] agent.server.raft: failed to heartbeat to: peer=10.32.9.123:8300 error="dial tcp <nil>->10.32.9.123:8300: connect: connection refused"
    2021-01-24T00:06:33.589Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Nonvoter df0703c1-7018-4060-0533-43d6212fe493 10.32.9.123:8300}" error="dial tcp <nil>->10.32.9.123:8300: connect: connection refused"
    2021-01-24T00:06:33.652Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Nonvoter df0703c1-7018-4060-0533-43d6212fe493 10.32.9.123:8300}" error="dial tcp <nil>->10.32.9.123:8300: connect: connection refused"
    2021-01-24T00:06:33.723Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Nonvoter df0703c1-7018-4060-0533-43d6212fe493 10.32.9.123:8300}" error="dial tcp <nil>->10.32.9.123:8300: connect: connection refused"
    2021-01-24T00:06:33.849Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Nonvoter df0703c1-7018-4060-0533-43d6212fe493 10.32.9.123:8300}" error="dial tcp <nil>->10.32.9.123:8300: connect: connection refused"
    2021-01-24T00:06:34.000Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Nonvoter df0703c1-7018-4060-0533-43d6212fe493 10.32.9.123:8300}" error="dial tcp <nil>->10.32.9.123:8300: connect: connection refused"
    2021-01-24T00:06:34.548Z [INFO]  agent.server.memberlist.lan: memberlist: Suspect consul-consul-server-0 has failed, no acks received
    2021-01-24T00:06:34.874Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="rpc error making call: stream closed"
    2021-01-24T00:06:35.873Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="context deadline exceeded"
    2021-01-24T00:06:35.906Z [WARN]  agent.server.raft: failed to contact: server-id=df0703c1-7018-4060-0533-43d6212fe493 time=2.50020128s
    2021-01-24T00:06:37.011Z [ERROR] agent.server.memberlist.lan: memberlist: Conflicting address for consul-consul-server-0. Mine: 10.32.9.123:8301 Theirs: 10.32.9.124:8301 Old state: 1
    2021-01-24T00:06:37.011Z [WARN]  agent.server.serf.lan: serf: Name conflict for 'consul-consul-server-0' both 10.32.9.123:8301 and 10.32.9.124:8301 are claiming
    2021-01-24T00:06:37.108Z [ERROR] agent.server.memberlist.lan: memberlist: Conflicting address for consul-consul-server-0. Mine: 10.32.9.123:8301 Theirs: 10.32.9.124:8301 Old state: 1
    2021-01-24T00:06:37.108Z [WARN]  agent.server.serf.lan: serf: Name conflict for 'consul-consul-server-0' both 10.32.9.123:8301 and 10.32.9.124:8301 are claiming
    2021-01-24T00:06:37.305Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.123:8302 Theirs: 10.32.9.124:8302 Old state: 0
    2021-01-24T00:06:37.305Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.123:8302 and 10.32.9.124:8302 are claiming
    2021-01-24T00:06:37.808Z [ERROR] agent.server.memberlist.wan: memberlist: Conflicting address for consul-consul-server-0.dc1. Mine: 10.32.9.123:8302 Theirs: 10.32.9.124:8302 Old state: 0
    2021-01-24T00:06:37.808Z [WARN]  agent.server.serf.wan: serf: Name conflict for 'consul-consul-server-0.dc1' both 10.32.9.123:8302 and 10.32.9.124:8302 are claiming
    2021-01-24T00:06:37.874Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="context deadline exceeded"
    2021-01-24T00:06:38.329Z [WARN]  agent.server.raft: failed to contact: server-id=df0703c1-7018-4060-0533-43d6212fe493 time=4.923360803s
    2021-01-24T00:06:38.548Z [INFO]  agent.server.memberlist.lan: memberlist: Marking consul-consul-server-0 as failed, suspect timeout reached (2 peer confirmations)
    2021-01-24T00:06:38.548Z [INFO]  agent.server.serf.lan: serf: EventMemberFailed: consul-consul-server-0 10.32.9.123
    2021-01-24T00:06:38.548Z [INFO]  agent.server: Removing LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.123:8300) (DC: dc1)"
    2021-01-24T00:06:38.549Z [INFO]  agent.server: member failed, marking health critical: member=consul-consul-server-0
    2021-01-24T00:06:39.548Z [INFO]  agent.server.memberlist.lan: memberlist: Suspect consul-consul-server-0 has failed, no acks received
    2021-01-24T00:06:39.873Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="context deadline exceeded"
    2021-01-24T00:06:40.825Z [WARN]  agent.server.raft: failed to contact: server-id=df0703c1-7018-4060-0533-43d6212fe493 time=7.418950267s
    2021-01-24T00:06:41.547Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-24T00:06:41.873Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="context deadline exceeded"
    2021-01-24T00:06:43.873Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="context deadline exceeded"
    2021-01-24T00:06:44.135Z [ERROR] agent.server.raft: failed to heartbeat to: peer=10.32.9.123:8300 error="dial tcp <nil>->10.32.9.123:8300: i/o timeout"
    2021-01-24T00:06:44.145Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=df0703c1-7018-4060-0533-43d6212fe493 fallback=10.32.9.123:8300 error="Could not find address for server id df0703c1-7018-4060-0533-43d6212fe493"
    2021-01-24T00:06:44.295Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Nonvoter df0703c1-7018-4060-0533-43d6212fe493 10.32.9.123:8300}" error="dial tcp <nil>->10.32.9.123:8300: i/o timeout"
    2021-01-24T00:06:44.615Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=df0703c1-7018-4060-0533-43d6212fe493 fallback=10.32.9.123:8300 error="Could not find address for server id df0703c1-7018-4060-0533-43d6212fe493"
    2021-01-24T00:06:45.413Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.124:8300 leader=10.32.3.60:8300
    2021-01-24T00:06:45.873Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="context deadline exceeded"
    2021-01-24T00:06:46.874Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="rpc error getting client: failed to get conn: rpc error: lead thread didn't get connection"
    2021-01-24T00:06:46.874Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="rpc error getting client: failed to get conn: dial tcp <nil>->10.32.9.123:8300: i/o timeout"
    2021-01-24T00:06:46.874Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="rpc error getting client: failed to get conn: rpc error: lead thread didn't get connection"
    2021-01-24T00:06:46.874Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="rpc error getting client: failed to get conn: rpc error: lead thread didn't get connection"
    2021-01-24T00:06:46.874Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="rpc error getting client: failed to get conn: rpc error: lead thread didn't get connection"
    2021-01-24T00:06:46.874Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="rpc error getting client: failed to get conn: rpc error: lead thread didn't get connection"
    2021-01-24T00:06:47.873Z [WARN]  agent: error getting server health from server: server=consul-consul-server-0 error="context deadline exceeded"
    2021-01-24T00:06:47.874Z [INFO]  agent.server.autopilot: Attempting removal of failed server node: id=df0703c1-7018-4060-0533-43d6212fe493 name=consul-consul-server-0 address=10.32.9.123:8300
    2021-01-24T00:06:47.874Z [INFO]  agent.server.serf.lan: serf: EventMemberLeave (forced): consul-consul-server-0 10.32.9.123
    2021-01-24T00:06:47.874Z [INFO]  agent.server: Removing LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.123:8300) (DC: dc1)"
    2021-01-24T00:06:47.874Z [INFO]  agent.server.raft: updating configuration: command=RemoveServer server-id=df0703c1-7018-4060-0533-43d6212fe493 server-addr= servers="[{Suffrage:Voter ID:a05729fb-0571-3a30-3b63-d1c587ddc071 Address:10.32.4.59:8300} {Suffrage:Voter ID:9fd19fd0-9b95-99dd-7900-b2caae48600e Address:10.32.3.60:8300}]"
    2021-01-24T00:06:47.881Z [INFO]  agent.server.raft: removed peer, stopping replication: peer=df0703c1-7018-4060-0533-43d6212fe493 last-index=241
    2021-01-24T00:06:47.885Z [INFO]  agent.server.autopilot: removed server: id=df0703c1-7018-4060-0533-43d6212fe493
    2021-01-24T00:06:47.885Z [INFO]  agent.server: deregistering member: member=consul-consul-server-0 reason=left
    2021-01-24T00:06:53.143Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.124:8300 leader=10.32.3.60:8300
    2021-01-24T00:06:54.146Z [ERROR] agent.server.raft: failed to heartbeat to: peer=10.32.9.123:8300 error="dial tcp <nil>->10.32.9.123:8300: i/o timeout"
    2021-01-24T00:06:54.615Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Nonvoter df0703c1-7018-4060-0533-43d6212fe493 10.32.9.123:8300}" error="dial tcp <nil>->10.32.9.123:8300: i/o timeout"
    2021-01-24T00:06:55.048Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=df0703c1-7018-4060-0533-43d6212fe493 fallback=10.32.9.123:8300 error="Could not find address for server id df0703c1-7018-4060-0533-43d6212fe493"
    2021-01-24T00:06:55.256Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=df0703c1-7018-4060-0533-43d6212fe493 fallback=10.32.9.123:8300 error="Could not find address for server id df0703c1-7018-4060-0533-43d6212fe493"
    2021-01-24T00:06:56.547Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-24T00:06:57.617Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Nonvoter df0703c1-7018-4060-0533-43d6212fe493 10.32.9.123:8300}" error="dial tcp <nil>->10.32.9.123:8300: connect: no route to host"
    2021-01-24T00:06:57.617Z [ERROR] agent.server.raft: failed to heartbeat to: peer=10.32.9.123:8300 error="dial tcp <nil>->10.32.9.123:8300: connect: no route to host"
    2021-01-24T00:06:58.530Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=df0703c1-7018-4060-0533-43d6212fe493 fallback=10.32.9.123:8300 error="Could not find address for server id df0703c1-7018-4060-0533-43d6212fe493"
    2021-01-24T00:06:58.897Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=df0703c1-7018-4060-0533-43d6212fe493 fallback=10.32.9.123:8300 error="Could not find address for server id df0703c1-7018-4060-0533-43d6212fe493"
    2021-01-24T00:07:00.688Z [ERROR] agent.server.raft: failed to heartbeat to: peer=10.32.9.123:8300 error="dial tcp <nil>->10.32.9.123:8300: connect: no route to host"
    2021-01-24T00:07:01.349Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.124:8300 leader=10.32.3.60:8300
    2021-01-24T00:07:01.591Z [WARN]  agent.server.raft: unable to get address for server, using fallback address: id=df0703c1-7018-4060-0533-43d6212fe493 fallback=10.32.9.123:8300 error="Could not find address for server id df0703c1-7018-4060-0533-43d6212fe493"
    2021-01-24T00:07:03.759Z [ERROR] agent.server.raft: failed to appendEntries to: peer="{Nonvoter df0703c1-7018-4060-0533-43d6212fe493 10.32.9.123:8300}" error="dial tcp <nil>->10.32.9.123:8300: connect: no route to host"
    2021-01-24T00:07:03.759Z [ERROR] agent.server.raft: failed to heartbeat to: peer=10.32.9.123:8300 error="dial tcp <nil>->10.32.9.123:8300: connect: no route to host"
    2021-01-24T00:07:06.548Z [INFO]  agent.server.memberlist.wan: memberlist: Suspect consul-consul-server-0.dc1 has failed, no acks received
    2021-01-24T00:07:10.619Z [INFO]  agent.server.memberlist.wan: memberlist: Marking consul-consul-server-0.dc1 as failed, suspect timeout reached (0 peer confirmations)
    2021-01-24T00:07:10.619Z [INFO]  agent.server.serf.wan: serf: EventMemberLeave: consul-consul-server-0.dc1 10.32.9.123
    2021-01-24T00:07:10.619Z [INFO]  agent.server: Handled event for server in area: event=member-leave server=consul-consul-server-0.dc1 area=wan
    2021-01-24T00:07:11.046Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.124:8300 leader=10.32.3.60:8300
    2021-01-24T00:07:20.305Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.124:8300 leader=10.32.3.60:8300
    2021-01-24T00:07:29.600Z [WARN]  agent.server.raft: rejecting vote request since we have a leader: from=10.32.9.124:8300 leader=10.32.3.60:8300
    2021-01-24T00:07:37.302Z [INFO]  agent.server.serf.lan: serf: EventMemberJoin: consul-consul-server-0 10.32.9.124
    2021-01-24T00:07:37.303Z [INFO]  agent.server: Adding LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.124:8300) (DC: dc1)"
    2021-01-24T00:07:37.303Z [INFO]  agent.server.raft: updating configuration: command=AddNonvoter server-id=df0703c1-7018-4060-0533-43d6212fe493 server-addr=10.32.9.124:8300 servers="[{Suffrage:Voter ID:a05729fb-0571-3a30-3b63-d1c587ddc071 Address:10.32.4.59:8300} {Suffrage:Voter ID:9fd19fd0-9b95-99dd-7900-b2caae48600e Address:10.32.3.60:8300} {Suffrage:Nonvoter ID:df0703c1-7018-4060-0533-43d6212fe493 Address:10.32.9.124:8300}]"
    2021-01-24T00:07:37.305Z [INFO]  agent.server.raft: added peer, starting replication: peer=df0703c1-7018-4060-0533-43d6212fe493
    2021-01-24T00:07:37.307Z [ERROR] agent.server.raft: peer has newer term, stopping replication: peer="{Nonvoter df0703c1-7018-4060-0533-43d6212fe493 10.32.9.124:8300}"
    2021-01-24T00:07:37.307Z [INFO]  agent.server.raft: entering follower state: follower="Node at 10.32.3.60:8300 [Follower]" leader=
    2021-01-24T00:07:37.307Z [INFO]  agent.server.raft: aborting pipeline replication: peer="{Voter a05729fb-0571-3a30-3b63-d1c587ddc071 10.32.4.59:8300}"
    2021-01-24T00:07:37.307Z [ERROR] agent.server.autopilot: failed to add raft non-voting peer: id=df0703c1-7018-4060-0533-43d6212fe493 address=10.32.9.124:8300 error="leadership lost while committing log"
    2021-01-24T00:07:37.307Z [ERROR] agent.server: failed to reconcile member: member="{consul-consul-server-0 10.32.9.124 8301 map[acls:2 build:1.9.2:6530cf37 dc:dc1 expect:3 ft_fs:1 ft_si:1 id:df0703c1-7018-4060-0533-43d6212fe493 port:8300 raft_vsn:3 role:consul segment: vsn:2 vsn_max:3 vsn_min:2 wan_join_port:8302] alive 1 5 2 2 5 4}" error="leadership lost while committing log"
    2021-01-24T00:07:37.307Z [INFO]  agent.server: cluster leadership lost
    2021-01-24T00:07:38.392Z [WARN]  agent.server.raft: rejecting vote request since our last index is greater: candidate=10.32.9.124:8300 last-index=251 last-candidate-index=237
    2021-01-24T00:07:38.495Z [ERROR] agent.server: error performing anti-entropy sync of federation state: error="context canceled"
    2021-01-24T00:07:44.743Z [WARN]  agent.server.raft: rejecting vote request since our last index is greater: candidate=10.32.9.124:8300 last-index=251 last-candidate-index=237
    2021-01-24T00:07:46.795Z [WARN]  agent.server.raft: heartbeat timeout reached, starting election: last-leader=
    2021-01-24T00:07:46.795Z [INFO]  agent.server.raft: entering candidate state: node="Node at 10.32.3.60:8300 [Candidate]" term=18
    2021-01-24T00:07:46.804Z [INFO]  agent.server.raft: election won: tally=2
    2021-01-24T00:07:46.804Z [INFO]  agent.server.raft: entering leader state: leader="Node at 10.32.3.60:8300 [Leader]"
    2021-01-24T00:07:46.804Z [INFO]  agent.server.raft: added peer, starting replication: peer=a05729fb-0571-3a30-3b63-d1c587ddc071
    2021-01-24T00:07:46.804Z [INFO]  agent.server.raft: added peer, starting replication: peer=df0703c1-7018-4060-0533-43d6212fe493
    2021-01-24T00:07:46.804Z [INFO]  agent.server: cluster leadership acquired
    2021-01-24T00:07:46.804Z [INFO]  agent.server: New leader elected: payload=consul-consul-server-2
    2021-01-24T00:07:46.805Z [INFO]  agent.server.raft: pipelining replication: peer="{Voter a05729fb-0571-3a30-3b63-d1c587ddc071 10.32.4.59:8300}"
    2021-01-24T00:07:46.811Z [WARN]  agent.server.raft: appendEntries rejected, sending older logs: peer="{Nonvoter df0703c1-7018-4060-0533-43d6212fe493 10.32.9.124:8300}" next=238
    2021-01-24T00:07:46.817Z [INFO]  agent.server: initializing acls
    2021-01-24T00:07:46.817Z [INFO]  agent.leader: started routine: routine="legacy ACL token upgrade"
    2021-01-24T00:07:46.817Z [INFO]  agent.leader: started routine: routine="acl token reaping"
    2021-01-24T00:07:46.818Z [INFO]  agent.leader: started routine: routine="federation state anti-entropy"
    2021-01-24T00:07:46.818Z [INFO]  agent.leader: started routine: routine="federation state pruning"
    2021-01-24T00:07:46.819Z [INFO]  agent.leader: started routine: routine="intermediate cert renew watch"
    2021-01-24T00:07:46.819Z [INFO]  agent.leader: started routine: routine="CA root pruning"
    2021-01-24T00:07:46.819Z [INFO]  agent.server: member joined, marking health alive: member=consul-consul-server-0
    2021-01-24T00:07:47.003Z [INFO]  agent.server.raft: pipelining replication: peer="{Nonvoter df0703c1-7018-4060-0533-43d6212fe493 10.32.9.124:8300}"
    2021-01-24T00:07:56.914Z [INFO]  agent.server.autopilot: Promoting server: id=df0703c1-7018-4060-0533-43d6212fe493 address=10.32.9.124:8300 name=consul-consul-server-0
    2021-01-24T00:07:56.914Z [INFO]  agent.server.raft: updating configuration: command=AddStaging server-id=df0703c1-7018-4060-0533-43d6212fe493 server-addr=10.32.9.124:8300 servers="[{Suffrage:Voter ID:a05729fb-0571-3a30-3b63-d1c587ddc071 Address:10.32.4.59:8300} {Suffrage:Voter ID:9fd19fd0-9b95-99dd-7900-b2caae48600e Address:10.32.3.60:8300} {Suffrage:Voter ID:df0703c1-7018-4060-0533-43d6212fe493 Address:10.32.9.124:8300}]"
    2021-01-24T00:07:58.206Z [INFO]  agent.server.serf.lan: serf: EventMemberUpdate: consul-consul-server-0
    2021-01-24T00:07:58.206Z [INFO]  agent.server: Updating LAN server: server="consul-consul-server-0 (Addr: tcp/10.32.9.124:8300) (DC: dc1)"
    2021-01-24T00:07:58.805Z [INFO]  agent.server.memberlist.wan: memberlist: Updating address for left or failed node consul-consul-server-0.dc1 from 10.32.9.123:8302 to 10.32.9.124:8302
    2021-01-24T00:07:58.805Z [INFO]  agent.server.serf.wan: serf: EventMemberJoin: consul-consul-server-0.dc1 10.32.9.124
    2021-01-24T00:07:58.805Z [INFO]  agent.server: Handled event for server in area: event=member-join server=consul-consul-server-0.dc1 area=wan
