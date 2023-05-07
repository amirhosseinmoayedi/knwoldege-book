#docker #orchestration #swarm
**<font color="#92cddc">Orchestrations</font>** **are layers on container managment applications like Docker**, they will handle:
- Scheduling services across **distributed nodes**
- Maintaining **high availability**
- Implementing reconciliation
- **Scaling**
- **Logging**

in <font color="#e36c09">Swarm</font> there is **Manager** and **Workers**, **only manager can run commands**.

To start manager node:
```
docker swarm init --advertise-addr newtwork_interface
```
To **join swarm node** run the given command by master node.

To run containers on a Docker Swarm, you need to create a **service**:
	**A service is an abstraction that represents multiple containers of the same image deployed across a distributed cluster.**

![[Pasted image 20230131125530.png]]
Create/Monitor service:
```bash
docker service create [options for creation container like -p or --name ]
docker service ls
docker service ps name_of_service
```

To **update the service and increase the number of replicas**:
```
docker service update --replicas=5 -d name_of_service
```

All the replicas are on same port so if request are sent to them, **the routing mesh has multiple containers in which to route requests to. The routing mesh acts as a <font color="#ffff00">load balancer</font> for these containers, alternating where it routes requests to**.

To see <font color="#ffff00">Logs</font>:
```
docker service logs name_of_service
```

For<font color="#ffff00"> updating running images</font>:
```
docker service update --image new_image --detach=true name_of_service
```
- `--update-parallelism`: specifies the number of containers to update immediately (defaults to 1).
- `--update-delay:` specifies the delay between finishing updating a set of containers before moving on to the next set.

The **inspect-and-then-adapt model of Docker Swarm enables it to perform reconciliation when something goes wrong**. For example, when a node in the swarm goes down, it might take down running containers with it. The swarm will recognize this loss of containers and will attempt to reschedule containers on available nodes to achieve the desired state for that service.

To leave a node in worker:
```
docker swarm leave
```

> The manager node contains the necessary information to manage the cluster, but if this node goes down, **the cluster will cease to function**.

**You should have at least three manager nodes but typically no more than seven**. Manager nodes implement the raft **consensus algorithm,** which requires that more than 50% of the nodes agree on the state that is being stored for the cluster. **If you don't achieve more than 50% agreement, the swarm will cease to operate correctly**. For this reason, note the following guidance for node failure tolerance:
- Three manager nodes tolerate one node failure.
- Five manager nodes tolerate two node failures.
- Seven manager nodes tolerate three node failures.

> the more manager nodes you have, the harder it is to achieve a consensus on the state of a cluster.
