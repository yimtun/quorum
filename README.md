## Design Philosophy

Most AI Agent projects follow the same pattern: *"Come use our new platform."* Quorum takes the opposite approach: *"We go adapt to your existing system."*

### Meet the infrastructure where it is

Operations teams already have Ansible playbooks, existing workflows, and established tooling. They shouldn't need to learn a new platform to benefit from LLM capabilities. By implementing the Ansible binary module spec, Quorum integrates as a native module — no new orchestration layer, no migration cost, no workflow disruption.

### Consensus before action

Infrastructure changes are high-stakes. A single model's judgment isn't reliable enough. Quorum requires multiple LLMs to reach consensus before proceeding — and surfaces disagreements explicitly for human review rather than hiding them. This is engineering responsibility, not a gimmick.

### The result

These two principles share the same underlying belief: **LLM capability should earn its place in production systems by adapting to them, not by demanding they adapt to it.**
