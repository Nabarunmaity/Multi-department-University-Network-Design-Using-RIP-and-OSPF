**What this is:** A networking class project where you built a small simulated university network in Cisco Packet Tracer and compared two ways of making routers automatically find paths to each other — RIP and OSPF.

**The setup:** Imagine a university with three separate buildings — Academic, Administrative, and Research — each with its own local network (LAN) full of computers. Each building has one router connecting it to the outside. You connected these three routers together to form a small campus backbone, and gave every device an IP address (192.168.10.x for Academic, 192.168.20.x for Admin, 192.168.30.x for Research).

**Why two topologies:** You actually built the network twice, with slightly different router wiring each time, because RIP and OSPF each show their true behavior best under different conditions:

- **OSPF version** — the three routers were connected in a straight line (Academic — Admin — Research). OSPF is a "link-state" protocol: every router shares a map of the whole network with every other router, then each one calculates the best path itself using an algorithm (Dijkstra's SPF). It reacts to changes instantly.

- **RIP version** — you added an extra cable directly between Academic and Research, so now there were two routes between them: a direct 1-hop link, and a longer 2-hop route through Admin. RIP is a simpler "distance-vector" protocol — each router only tells its neighbors what it knows, gossip-style, and picks the shortest path by counting hops. Adding that extra link let you actually watch RIP choose the 1-hop path over the 2-hop path.

**What you configured:** IP addresses on every interface, then turned on RIP version 2 on one network and OSPF (single area) on the other, and let them converge (i.e., let all the routers learn about each other).

**What you observed:**
- OSPF converged almost instantly when a link went down, because it floods an update the moment something changes. RIP took much longer, since it only updates every 30 seconds and has extra "wait and see" timers before trusting a route is gone.
- RIP has a hard limit of 15 hops — anything farther is treated as unreachable, which caps how big a network it can handle. OSPF has no such ceiling.
- RIP picked routes purely by hop count (fewest routers in the way), even if that path wasn't actually faster. OSPF picks routes by cost, which is based on link bandwidth — a more realistic measure.
- RIP constantly broadcasts its entire routing table to everyone every 30 seconds, whether anything changed or not. OSPF only sends updates when something actually changes, so it's lighter on network traffic.

**Conclusion:** OSPF is the better choice for a real, growing university network — it scales better, converges faster, and makes smarter path decisions. RIP is fine for a tiny, simple network, but not for anything expected to expand.

**Where you could take it further:** splitting OSPF into multiple areas as the campus grows, using smaller subnet sizes to save IP addresses, adding backup gateways (HSRP/VRRP), adding IPv6, or testing how the network performs under real traffic load instead of just topology changes.
