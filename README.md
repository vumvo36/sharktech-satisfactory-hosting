# satisfactory server hosting: Real Requirements, Honest Costs, and How to Keep Your Factory Running 24/7

Most people searching for satisfactory server hosting aren't shopping for enterprise infrastructure. They've got a save file with 40 hours of conveyor spaghetti in it, three friends in different time zones, and a problem: somebody's PC has to stay on for anyone to play. That's the actual job a dedicated server solves — the factory keeps running whether anyone is logged in or not.

This guide covers what the server software actually demands, what the realistic hosting options cost, and where a provider like Sharktech fits in (spoiler: it's the sledgehammer option, which is either perfect or ridiculous depending on how many players you have). Everything here is based on the current official Satisfactory wiki specs and the provider's live pricing pages.

## What a Satisfactory dedicated server actually needs

The server binaries are free. You download them through SteamCMD, the Steam client (as a "tool"), or as a free add-on from the Epic Games Store. App ID is 1690800, and it runs on 64-bit Windows or Linux. No 32-bit, no ARM — that rules out cheap ARM VPS plans entirely.

Here's what the official wiki lists as requirements:

- **CPU**: A recent x86-64 Intel (i5-3570 or better) or AMD (Ryzen 5 3600 or better) chip. The server spawns up to 26 threads, but the game loop is fundamentally single-threaded — one core always works harder than the rest. Anything with a single-thread rating of 2000+ works.
- **RAM**: 8GB minimum. 16GB is recommended for larger saves or more than 4 players.
- **Storage**: 12.4GB of server files on Windows, about 8GB total on Linux.
- **Network**: Ports 7777 (TCP + UDP) and 8888 (TCP) forwarded and reachable.

Two details in there matter more than people expect.

First, the single-core thing. A Coffee Stain developer put it plainly back in 2021: the server spawns plenty of threads, but almost all the work still happens on the primary thread. This is why a 36-core Xeon at 2.1GHz can feel worse than a 6-core chip at 3.3GHz. When you're comparing hosting configs, clock speed beats core count for this game.

Second, there's a landmine if you go the VPS route: the wiki explicitly warns that a `kvm64` CPU type in a virtual machine **won't work** — the server may boot, then crash the moment a new game is created. On Proxmox-based platforms, a `host` CPU type is the fix. If you rent a VPS anywhere and the provider exposes a generic virtual CPU profile, ask before you commit. It's the most common "why does my server keep dying" cause nobody expects.

Also worth knowing: vanilla sessions cap at 4 players by default, and the `MaxPlayers` setting in the ini can raise that (the wiki's own example bumps it to 8). Some game-panel hosts sell plans marketed for 16+ players. Your factory's framerate will have opinions about that long before the server does.

## Your three hosting options, and what they really cost

There are three realistic paths, and the price spread between them is genuinely absurd — current Satisfactory hosting plans range from about $6 to $72 a month depending on the provider.

**Game panel hosting ($5.99–$21/month typical).** Providers like Indifferent Broccoli (from $5.99/mo up to $20.99/mo for a 16-player plan) or GPORTAL (around $17.30 per 30 days for 10 slots) give you a web panel: click, deploy, connect. Someone else handles updates, reboots, and DDoS filtering. The catch is that you're paying for player slots on shared hardware, and RAM is often the squeeze point — community complaints about hitting RAM ceilings on big saves are common enough that "unlimited RAM" has become a marketing bullet for some hosts. It's the right choice for 2–4 friends who want zero fuss.

**A general-purpose VPS ($8–$50/month).** You rent a Linux or Windows VM, install the server yourself via SteamCMD, and manage it. This is where the wiki's kvm64 warning applies. One Reddit user priced out running the server continuously on Linode at roughly $50/month for the resources needed — hyperscaler pricing adds up fast. Smaller regional VPS providers get that down a lot. You trade convenience for control and price.

**Dedicated bare metal ($99–$700/month).** A whole physical machine, no neighbors, no virtualization quirks, full hardware access. For Satisfactory specifically this is usually overkill — the game needs one strong core and 8–16GB of RAM, not 64 cores. But it makes sense if you're hosting multiple game servers, a community, or you just want the factory to never, ever stutter. Which brings us to Sharktech.

## Where Sharktech fits in

Sharktech is a bare-metal and VPS provider that's been around for over two decades, running its own network (AS46844) with data centers in Las Vegas, Los Angeles, Denver, Chicago, and Amsterdam. They're known in the game-server world mainly for one thing: DDoS protection is built into everything, not a paid add-on. One of their hosting customers publicly noted their game servers absorbed 3–8Gbit attacks without skipping a beat. Public game servers get attacked constantly, so this is not a theoretical benefit.

They don't have a one-click "Satisfactory" installer like the panel hosts do — you'd install the server software yourself. What they offer instead is raw hardware with a 99.99% uptime guarantee, 10Gbps ports with 300TB/month of transfer, free setup, and hardware you can upgrade at any time.

Their product range relevant to game hosting:

- **Smart VPS** — Xeon Gold CPUs, NVMe storage, 60Gbps DDoS protection, Proxmox-based resource pool. Prices start around $7.95/month for a small configuration, with the slider showing the low end dipping to about $3.98/mo on annual billing. Billing discounts are real here: 25% off quarterly, 35% off semi-annually, 50% off annually. The platform itself is triple-redundant with a 99.999% uptime claim.
- **Bare-metal dedicated servers** — from $259/month for dual Xeon E5-2695v4 (64GB RAM) up to $699/month for dual AMD EPYC 7702. This is the "nobody touches my hardware" tier.
- **Public cloud** — resource pools from $39/month (Small tier, 4–16 vCPU, 8–32GB) through $499/month (Enterprise). More of a business/infrastructure product, but the Small and Medium tiers are viable game-server hosts too.

If you just want to see the whole catalog with live prices: 👉 browse Sharktech's current server lineup here.

## The full bare-metal lineup, with current prices

This is everything currently listed on their dedicated servers page, all with free setup, 10Gbps networking (300TB/month), DDoS protection included, and upgradeable RAM/storage/network. Monthly billing shown; quarterly and semi-annual billing shave about 5–10% off, annual billing around 15% off, based on the listed term prices.

| Server | CPU | RAM | Storage | Network | Price (monthly) | Order |
| --- | --- | --- | --- | --- | --- | --- |
| Dual Xeon E5-2695v4 (2.5" bays) | 36 × 2.1 GHz | 64GB DDR4 | 2TB M.2 NVMe + 6 SATA bays | 10Gbps, 300TB/mo | $259/mo | [Order this server](https://portal.sharktech.net/aff.php?aff=1611&pid=741) |
| Dual Xeon E5-2695v4 (3.5" bays) | 36 × 2.1 GHz | 64GB DDR4 | 2TB M.2 NVMe + 6 SATA bays | 10Gbps, 300TB/mo | $269/mo | [Contact sales for this config](https://bit.ly/SharKTech) |
| Dual Xeon Gold 6248 (3.5" bays) | 40 × 2.5 GHz | 128GB DDR4 | 2TB M.2 NVMe + 3 SATA bays | 10Gbps, 300TB/mo | $299/mo | [Order this server](https://portal.sharktech.net/aff.php?aff=1611&pid=660) |
| Dual Xeon Gold 6248 (2.5" bays) | 40 × 2.5 GHz | 128GB DDR4 | 2TB M.2 NVMe + 6 SATA bays | 10Gbps, 300TB/mo | $309/mo | [Order this server](https://portal.sharktech.net/aff.php?aff=1611&pid=636) |
| Dual Xeon Gold 6246 | 24 × 3.3 GHz | 128GB DDR4 | 2TB M.2 NVMe + 3 SATA bays | 10Gbps, 300TB/mo | $309/mo | [Order this server](https://portal.sharktech.net/aff.php?aff=1611&pid=814) |
| Dual Xeon Gold 6248 (U.2 bays) | 40 × 2.5 GHz | 128GB DDR4 | 2TB M.2 NVMe + 6 U.2 bays | 10Gbps, 300TB/mo | $329/mo | [Order this server](https://portal.sharktech.net/aff.php?aff=1611&pid=766) |
| AMD EPYC 7702P | 64 × 2 GHz | 128GB DDR4 | 2TB M.2 NVMe + 10 U.2 bays | 10Gbps, 300TB/mo | $499/mo | [Order this server](https://portal.sharktech.net/aff.php?aff=1611&pid=729) |
| Dual AMD EPYC 7702 | 128 × 2 GHz | 128GB DDR4 | 2TB M.2 NVMe + 10 U.2 bays | 10Gbps, 300TB/mo | $699/mo | [Contact sales for this config](https://bit.ly/SharKTech) |

One practical note: Sharktech says bare-metal delivery can take longer than 24 hours in some cases due to hardware demand, and custom configurations go through their sales team — which responds within hours, and their support is 24/7/365 either way.

## Which config to pick if you're doing this for Satisfactory

Here's the honest math. The game wants one fast core and 16GB of RAM for a comfortable multi-player world. All of the servers above clear the RAM bar by a mile (64GB minimum). So the differentiator is that single-threaded game loop — and among these options, the **Dual Xeon Gold 6246 at $309/mo** is the interesting one: 3.3GHz base clock versus 2.1–2.5GHz on the others. Per the wiki's own guidance (favor single-core performance, aim for a single-thread rating of 2000+), higher clocks are what your factory actually feels.

The dual EPYC boxes are magnificent machines that Satisfactory will use about 4% of. Buy those if you're running other things too, not for one factory.

And the honest verdict for most readers: if it's just you and three friends, a $309/month bare-metal server is paying $290/month more than you need. The Smart VPS route at Sharktech handles this job for pocket change — Xeon Gold cores, NVMe, 60Gbps DDoS protection, and you size the RAM yourself (16GB comfortably covers the wiki's recommendation). The platform runs on Proxmox clusters, and the kvm64-vs-host CPU caveat applies to Proxmox VMs generally, so confirm the CPU profile with their support when you deploy — it's a two-minute conversation with their 24/7 team and saves you a very confusing crash later. If that sounds like your speed: 👉 check out Sharktech's Smart VPS plans here.

Where bare metal earns its price: public community servers that get DDoSed, hosting multiple games on one box, or megabase saves where you want zero contention for RAM and disk I/O, ever.

## Getting the server online: the short version

Once you have any Windows or Linux box (VPS, dedicated, or the PC under your desk), the process is the same:

1. **Install SteamCMD** and pull the server files. On Linux, one command does it:
   bash
   steamcmd +force_install_dir ~/SatisfactoryDedicatedServer +login anonymous +app_update 1690800 validate +quit
   
   Add `-beta experimental` at the end if you want the Experimental branch. On Windows the same command works from a PowerShell prompt, or install the "Satisfactory Dedicated Server" tool directly in Steam.

2. **Open the ports**: 7777 TCP + UDP and 8888 TCP. On a hosted server there's no router to configure — just the OS firewall. With UFW on Ubuntu that's `7777/udp`, `7777/tcp`, and `8888/tcp`.

3. **Run it as a service**, not in a terminal window. The wiki strongly recommends this; a service auto-restarts after crashes and updates, while a manual launch dies the moment you close the window. Docker is also a well-trodden path if that's your thing.

4. **Claim the server from in-game**: open Server Manager in the main menu, add the server by IP, set an admin password, and create or import your game. Local saves upload straight to the server through the Manage Saves tab.

5. **Sanity-check it's reachable**: `curl -k https://ip:port` should return a JSON error about a route handler not found — that's success, weirdly enough. If friends get stuck on an endless loading screen, the 8888 reliable-messaging port is the usual suspect.

## What people usually ask

**Is a dedicated server worth it over hosting on my own PC?** If your group plays at overlapping times and someone's machine can handle it, in-game hosting costs nothing. The dedicated server wins when schedules don't line up, the save has outgrown someone's RAM, or you want the factory running while everyone's offline. Also, hosting and playing on the same machine has a known unresolved bug where the server becomes unreachable after the host disconnects — a dedicated box sidesteps that entirely.

**Can I run it on a cheap ARM VPS?** No. No ARM, no 32-bit. Check the CPU architecture and the virtualization profile before buying anything.

**Why do prices vary from $6 to $72 a month for the same game?** Slot-based panel hosts price by player count on shared hardware. VPS and dedicated providers price by actual resources. A $6 panel plan and a $259 bare-metal server are not selling the same thing, even though both will run Satisfactory.

**Is Sharktech any good?** They're an established provider (20+ years, own ASN, five data centers), and independent reviews are solid on the technical side — HostAdvice's testing highlighted strong IOPS and low latency, and their Trustpilot score sits around 3.4–3.5 out of 5, though that's from a small sample of 13 reviews, so treat it as a weak signal either way. Their reputation in gaming specifically centers on absorbing real DDoS attacks, which is the failure mode that actually kills public game servers.

**How much RAM do I really need?** 8GB to boot, 16GB for bigger saves or more than 4 players, and honestly 16GB is where you should start for anything you care about. Giant late-game factories can push past that — which is the one scenario where a panel host's RAM ceiling becomes a real problem and having a 64GB+ machine stops being funny.

## The bottom line

For a private friend group: a game panel host or a small VPS solves this for under $21/month. If you go the VPS route at Sharktech, the Smart VPS line starts around $7.95/month with Xeon Gold hardware and DDoS protection baked in — just confirm the CPU profile isn't kvm64 before you build. For public servers, communities, or anyone running more than one game: bare metal with real DDoS filtering is the tier that stops worrying you, and the Gold 6246 config is the one built the way Satisfactory actually runs — fast clocks, more RAM than the game knows what to do with, and nobody else's workload in sight.

👉 See all of Sharktech's current dedicated server and VPS pricing here.
