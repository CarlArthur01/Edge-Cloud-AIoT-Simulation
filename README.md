# AIoT-MNIST-Tradeoff-Analysis

A PyTorch-based simulation of an adaptive AIoT data pipeline evaluating the trade-offs between CNN accuracy, latency, and data freshness under varying traffic loads (20, 50, and 150 FPS).

## Project Scope
- **Adaptive Inference:** Implements an AI Scheduler switching between Baseline, Pruned, and Quantized models on the Edge, or offloading to the Cloud based on real-time queue capacity.
- **Queue Policy Evaluation:** Compares `DropOld` vs `DropNew` overflow mechanics to analyze the Age of Information (AoI) and prevent the "staleness trap".
- **Hardware Profiling:** Tracks physical hardware complexity by benchmarking total model parameters and Multiply-Accumulate (MAC) operations using `torchinfo`.
- **Real-Time Trade-offs:** Demonstrates how arrival rate bottlenecks force the system to balance data freshness against classification accuracy on the MNIST dataset.

## Simulation Setup
The simulation environment is built with a 1 ms resolution over a total duration of 20,000 ms, modeling a continuous stream of MNIST digits under a strict decision deadline of 100 ms. 

### Execution Paths & Target Latencies:
- **Edge Device:** Baseline (40 ms), Pruned (25 ms), Quantized (20 ms)
- **Cloud Server:** Pruned model offloading with a fixed 20 ms network communication latency (3 ms processing time)

## Key Takeaways
1. **The Capacity Deficit:** Model compression techniques alone cannot stabilize a system if the incoming data rate permanently exceeds the hardware processing throughput.
2. **Age of Information (AoI):** Retaining older data in a filled queue (`DropNew`) leads to a "staleness trap" where execution cycles are wasted on samples bound to miss deadlines. Dropping old data (`DropOld`) successfully preserves data freshness.
