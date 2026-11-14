# Sup, I'm Patrick Ryan

### I should not have access to production systems but here we are

**CTO at [Fireball Industries](https://fireballz.ai).** I'm a mechanical engineer who ended up writing the software for the machines instead of just fixing them. I build automation software, an operating system for real-time control, and the platform that ties plants together. Then I go home and run all of it on a rack in my house, because I don't trust anything I haven't broken myself.

I have root on real control systems in real plants. One of us is ending up in a deposition and I have made peace with which one.

## What I'm building right now

### HotLoop

[HotLoop](https://hotloop.io) is automation loops for the real world, written from scratch in Go. HotLoop is seven products for the plant floor: four that grow with a site (IoT, Edge, Gateway, Edge Relay), plus Flow for wiring, Cascade for workflows, and Transmitter for putting a PLC's values on the wire.

* It reads real equipment, keeps the history, raises ISA-18.2 alarms, and moves the machine. Every write, from a person or a rule, crosses one gate nothing routes around. Out of range gets refused, never clamped, and the refusal is written down.
* A failed read is bad quality, never zero, so a dead sensor doesn't set off every low alarm and send the night shift chasing a tank that's fine.
* Static binaries, no runtime to babysit, from a Pi on a shelf up to a whole plant.
* Public pieces: [HotLoop Flow](https://github.com/HotLoop-io/hotloop-flow), the [docs](https://docs.hotloop.io), and the [org](https://github.com/HotLoop-io).

### EmberRTOS and real-time Linux

[EmberRTOS](https://docs.embernet.ai/docs/emberrtos/overview) is the operating system we run on EmberNet edge nodes. I wanted a box you rack once and never have to babysit, so it's built to take care of itself.

* A read-only root, with atomic updates that land as one snapshot and roll back on their own if something misbehaves.
* Two variants on the same base. Standard is a general-purpose kernel for container hosts and gateways. [RT](https://docs.embernet.ai/docs/emberrtos/realtime) runs a real-time (PREEMPT_RT) kernel for anything with a hard deadline, like motion control, safety interlocks, and PLC runtimes.
* Real-time is about the worst case, not the average. RT isolates cores for the control work, steers interrupts and housekeeping off them, and turns off the idle states that add wake-up latency.
* We measured it on commodity 4-core Intel desktop hardware with `cyclictest` over a 5-minute run, with the housekeeping core saturated: 42 µs worst-case wake-up, about 1 µs average. The method and the table are on the docs page.
* If a node is set up as RT and a verified real-time kernel can't be found, provisioning refuses instead of quietly booting a normal kernel under an RT badge.

### EmberNet and EmberCORE

[EmberNet](https://embernet.ai) is the industrial IoT platform I built so plants could connect, manage, and watch their edge devices without a pile of vendor tools. EmberCORE is its dashboard. Flux carries the traffic over TLS on 443, because that's the egress an industrial site actually lets out, and ArcNet is the vNIC that rides on top of it. Everything is multi-tenant, one tenant can't see another, and the whole thing runs across AMD64 and ARM64.

I also keep [Emberburn](https://github.com/Embernet-ai/Emberburn) around, a small industrial tag application.

## The home lab

The house is where I find out whether something actually works before a plant has to.

* **A k3s cluster in the rack:** eleven nodes, six Raspberry Pi 5s and five x86 boxes, with three of the x86 servers holding the control plane.
* **The Beast:** a 32-core Threadripper workstation that is the compute spine of the cluster. Test suites, lab stacks, and release rehearsals run there so my laptop doesn't have to.
* **The network:** a UniFi build on a 100.64 address plan, split into VLANs for trusted, IoT, cameras, guests, management, and SCADA.
* **Podman and Quadlet** for the HotLoop lab, k3s with Helm for everything that wants a cluster. No Docker, and I'd like to keep it that way.
* **The house is HotLoop's proving ground.** My own smart home is where the software has to earn its keep before I ask anyone else to trust it.

## Metal, too

I'm mechanical first, and that half isn't a hobby. My father ran a machine shop, and I picked up the same distrust of anything nobody has ever held.

* CNC milling and turning, wire EDM, and additive (FDM and SLA), including printed parts that are doing real jobs on real machines.
* AMS2 robotics cells, plus SolidWorks and Fusion 360 for design.
* I hold 0.002 inch and treat tolerance as non-negotiable. I'll design a part at midnight, cut it in the morning, and have it in production by the afternoon.
* [Ryan Industries](https://ryanindustries.io) is the collaborative robots and machining side.

## Tools I reach for

![Go](https://img.shields.io/badge/-Go-00ADD8?style=flat-square&logo=go&logoColor=white)
![Kubernetes](https://img.shields.io/badge/-Kubernetes-326CE5?style=flat-square&logo=kubernetes&logoColor=white)
![K3s](https://img.shields.io/badge/-K3s-FFC61C?style=flat-square&logo=k3s&logoColor=black)
![Helm](https://img.shields.io/badge/-Helm-0F1689?style=flat-square&logo=helm&logoColor=white)
![Podman](https://img.shields.io/badge/-Podman-892CA0?style=flat-square&logo=podman&logoColor=white)
![Terraform](https://img.shields.io/badge/-Terraform-7B42BC?style=flat-square&logo=terraform&logoColor=white)
![Prometheus](https://img.shields.io/badge/-Prometheus-E6522C?style=flat-square&logo=prometheus&logoColor=white)
![Grafana](https://img.shields.io/badge/-Grafana-F46800?style=flat-square&logo=grafana&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/-PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![MQTT](https://img.shields.io/badge/-MQTT-660066?style=flat-square&logo=mqtt&logoColor=white)
![Python](https://img.shields.io/badge/-Python-3776AB?style=flat-square&logo=python&logoColor=white)
![C++](https://img.shields.io/badge/-C++-00599C?style=flat-square&logo=c%2B%2B&logoColor=white)
![C#](https://img.shields.io/badge/-C%23-239120?style=flat-square&logo=c-sharp&logoColor=white)
![Bash](https://img.shields.io/badge/-Bash-4EAA25?style=flat-square&logo=gnu-bash&logoColor=white)
![PowerShell](https://img.shields.io/badge/-PowerShell-5391FE?style=flat-square&logo=powershell&logoColor=white)
![Raspberry Pi](https://img.shields.io/badge/-Raspberry%20Pi-A22846?style=flat-square&logo=raspberrypi&logoColor=white)
![CODESYS](https://img.shields.io/badge/-CODESYS-009ACE?style=flat-square&logoColor=white)
![Ignition](https://img.shields.io/badge/-Ignition%20SCADA-FF6600?style=flat-square&logoColor=white)
![SolidWorks](https://img.shields.io/badge/-SolidWorks-FF0000?style=flat-square&logoColor=white)
![Fusion 360](https://img.shields.io/badge/-Fusion%20360-0696D7?style=flat-square&logo=autodesk&logoColor=white)

## Fun facts

* I run on White Monster, and the fridge at work has a label that says `env: production`.
* My commit history reads like the five stages of grief on a loop.
* Go is the house language because a static binary doesn't ask me how I'm feeling.
* I've gone from a CAD model to a machined part to software running on the machine that made it, all in one day.

## Reach me

* [patrick@fireballz.ai](mailto:patrick@fireballz.ai)
* [LinkedIn](https://www.linkedin.com/in/patrickr88)
* Discord: **pryan0508**
