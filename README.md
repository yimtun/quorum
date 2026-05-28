## Design Philosophy

Most AI Agent projects follow the same pattern: *"Come use our new platform."* Quorum takes the opposite approach: *"We go adapt to your existing system."*

### Meet the infrastructure where it is

Operations teams already have Ansible playbooks, existing workflows, and established tooling. They shouldn't need to learn a new platform to benefit from LLM capabilities. By implementing the Ansible binary module spec, Quorum integrates as a native module — no new orchestration layer, no migration cost, no workflow disruption.

### Consensus before action

Infrastructure changes are high-stakes. A single model's judgment isn't reliable enough. Quorum requires multiple LLMs to reach consensus before proceeding — and surfaces disagreements explicitly for human review rather than hiding them. This is engineering responsibility, not a gimmick.

### The result

These two principles share the same underlying belief: **LLM capability should earn its place in production systems by adapting to them, not by demanding they adapt to it.**

## Training Data & Self-Improvement

Quorum is designed to get smarter over time through a fault injection and data flywheel pipeline.

### Fault Injection

Real failure scenarios are generated in test environments using tools like Chaos Mesh and LitmusChaos, covering:

- Pod OOMKill / CrashLoopBackOff
- Node pressure and eviction
- Network latency and DNS failure
- Misconfigured resources (wrong image, missing ConfigMap, invalid limits)

### Data Collection

After each fault injection, Quorum collects and sanitizes the cluster snapshot automatically. Each record is stored as a labeled input/output pair:

```json
{
  "input": "sanitized cluster snapshot at fault time",
  "root_cause": "OOMKill due to memory limit too low",
  "resolution": "increase memory limit or optimize application memory usage"
}
```

### Labeling Strategy

- **Human annotation** for high-quality ground truth cases
- **LLM-assisted labeling** (GPT-4 / DeepSeek generate candidates, humans review) for scale

### Usage Pipeline

| Stage | Approach | When |
|-------|----------|------|
| Early | Few-shot prompting | < 100 cases |
| Mid | RAG knowledge base | 100–500 cases |
| Advanced | Fine-tune small model (Qwen / DeepSeek) | 500+ cases |

The RAG knowledge base stage is the primary near-term target — fault cases are embedded and retrieved at diagnosis time, grounding LLM responses in real historical incidents rather than general knowledge.
