# AI Network Fabric Visualizer

Interactive real-time visualization of an AI GPU cluster network fabric, built on an Arista 400GbE spine-leaf topology.

**[Live Demo →](https://arista-cam.github.io/ai-traffic-visualiser/)**

## What It Shows

A simulated data center fabric connecting up to 512 GPUs through Arista leaf and spine switches. The visualization demonstrates how network traffic patterns differ across AI workloads and how the fabric handles failures, congestion, and scale.

### Workload Modes

- **Training** — GPUs cycle through coordinated phases:
  - **Compute** — Forward/backward passes; traffic flows leaf → GPU
  - **All-Reduce** — Gradient synchronization; massive east-west traffic floods the spine fabric
  - **Straggler** — One slow GPU stalls the entire cluster (the #1 killer of training throughput at scale)
  - **Checkpoint** — Periodic model state save; all GPUs pause briefly for fault tolerance
- **Inference** — GPUs serve requests independently with steady north-south traffic; no coordination needed

### Topology Modes

- **Fat-Tree** — Classic spine-leaf with consecutive GPUs assigned to the same leaf switch
- **Rail-Optimized** — Same-index GPUs across servers share a leaf (rail), optimized for all-reduce patterns

### AI Networking Technology Scenarios

Browse 10 educational scenario cards explaining the protocols and technologies used in AI networking. Each scenario drives the simulation to demonstrate the concept visually, with a "More Info" button for deep-dive explanations.

| Scenario | What It Shows |
|----------|--------------|
| **Static ECMP** | Hash polarisation — one spine overloaded, others idle |
| **Dynamic Load Balancing (DLB)** | Hardware port-quality sampling redistributes flows |
| **Cluster Load Balancing (CLB)** | Per-flow round-robin prevents downstream contention |
| **Priority Flow Control (PFC)** | Hop-by-hop backpressure cascade from a congested link |
| **ECN** | Congestion marking at the switch, sender rate reduction |
| **Ultra Ethernet (UEC)** | Packet trimming — trim instead of drop for faster recovery |
| **NCCL Ring All-Reduce** | Unidirectional ring pattern through GPUs |
| **NCCL Tree All-Reduce** | Binary tree reduce/broadcast with phase animation |
| **MRC (Multipath Reliable Connection)** | OCP spec — single QP sprays across all spine paths |
| **Switch Radix** | Port density, 1:1 non-blocking topology scaling |

### Interactive Features

- **GPU count selector** — Switch between 64, 128, 256, and 512 GPUs
- **Click any element to toggle failure** — Click a spine, leaf, GPU, or spine-leaf link to shut it down. Click again to bring it back up.
  - Failed spines/leaves turn red with "DOWN" label
  - Failed GPUs turn red; orphaned GPUs (whose leaf is down) pulse with a red ring
  - Traffic reroutes to surviving paths; stats reflect reduced capacity
- **Hover any GPU** — Detail modal with status, utilization, temperature, memory, power, job assignment, leaf switch, and a sparkline showing utilization history
- **Hover a GPU** — Path tracing lights up the full route through the fabric (GPU → leaf → spines → all other leaves)
- **Hover a leaf switch** — Tooltip shows aggregate GPU stats (utilization, temperature, state counts)
- **Hover a spine-leaf link** — ECMP flow count label
- **Drag switches** — Reposition spine and leaf nodes; links follow
- **Pause/Play** — Spacebar or button; freeze the simulation while keeping interaction
- **Speed slider** — 0.1x to 5x simulation speed
- **Load slider** — 0-100% network load; controls GPU utilization, particle density, throughput, and temperatures
- **Phase info panel** — Browse all scenarios with ◀ ▶ buttons; pin to force the simulation into that phase; "More Info" for detailed explanations

### Visual Features

- Continuous GPU utilization heatmap (blue → cyan → green → yellow → red)
- Multi-job coloring — 2-3 training jobs shown as colored rings per GPU group
- Live throughput waveform strip showing compute/all-reduce heartbeat over time
- Micro-burst congestion flashes on spine links (more frequent during all-reduce)
- Pulsing links during peak load and all-reduce
- Particles react to GPU state and load level (idle = slow/dim, peak = fast/bright)
- Rack/rail bounding boxes grouping GPUs by leaf switch
- Failed elements shown in red with dashed links

## Running Locally

No dependencies. Just open the file:

```bash
open index.html
```

## Tech

Single self-contained HTML file. Canvas 2D rendering with offscreen sprite caching and batched draw calls for performance at 512 GPUs. No frameworks, no build step, no external dependencies beyond Google Fonts.
