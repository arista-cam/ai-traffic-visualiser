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

### Interactive Features

- **GPU count selector** — Switch between 64, 128, 256, and 512 GPUs
- **Hover any GPU** — Path tracing lights up the full route through the fabric
- **Hover a leaf switch** — Tooltip shows aggregate GPU stats
- **Click a GPU** — Detail modal with utilization sparkline history
- **Click a spine** — Toggle link failure; traffic reroutes to surviving spines
- **Hover a spine-leaf link** — ECMP flow count label
- **Drag switches** — Reposition spine and leaf nodes; links follow
- **Pause/Play** — Spacebar or button; freeze the simulation while keeping interaction
- **Speed slider** — 0.1x to 5x simulation speed
- **Phase info panel** — Browse all phases with ◀ ▶ buttons; pin to prevent auto-switching

### Visual Features

- Continuous GPU utilization heatmap (blue → cyan → green → yellow → red)
- Multi-job coloring — 2-3 training jobs shown as colored rings per GPU group
- Live throughput waveform strip showing compute/all-reduce heartbeat
- Micro-burst congestion flashes on spine links
- Pulsing links during peak load and all-reduce
- Particles react to GPU state (idle = slow/dim, peak = fast/bright)

## Running Locally

No dependencies. Just open the file:

```bash
open index.html
```

## Tech

Single self-contained HTML file (~2400 lines). Canvas 2D rendering with offscreen sprite caching and batched draw calls. No frameworks, no build step, no external dependencies beyond Google Fonts.
