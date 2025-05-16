# 👻Ghost-Based Sensors (GBS): Observability for Ephemeral CI/CD Pipelines

## Project Vision

Modern Continuous Integration and Delivery (CI/CD) pipelines are ephemeral in nature. Built Quick, Run Quick, Die Quick ... they vanishing once theyre done. Yet these short lived environments remain largely unmonitored, creating a critical blind spot. Inspired by traditional "Host-Based Sensors (HBS)" and "Cloud-Based Sensors (CBS)," I introduce **Ghost-Based Sensors (GBS)** which is my attempt to gain deeper visibility into these transient, short-lived build and deployment environments.

## Why Ghost-Based Sensors?

Ephemeral CI environments like GitHub Actions or Azure Pipelines spin up quickly on shared and public infrastructure, perform builds and tests, and disappear just as fast; _Do a double take - did I see a ghosts_. Traditional security monitoring tools designed for persistent infrastructure fail here. GBS workes to close or shrink this gap by capturing crucial telemetry, ensuring even short-lived pipelines activities are recorded.

## What Will Ghost-Based Sensors Do?

Leveraging lightweight instrumentation and eBPF-based telemetry, Ghost-Based Sensors will capture:

| Feature                 | Description                                                                                        | Method                  |
| ----------------------- | -------------------------------------------------------------------------------------------------- | ----------------------- |
| Process Telemetry       | Logs every process execution and command-line argument used during pipeline runs                   | eBPF tracing            |
| File System Activity    | Monitors file creations, deletions, and modifications to detect suspicious changes                 | Kernel event monitoring |
| Network Connections/DNS | Records all outbound connections and DNS lookups to reveal data exfiltration or unauthorized calls | Network monitoring      |
| System Call Tracing     | Captures granular syscall data to detect anomalies such as privilege escalations                   | eBPF syscall tracing    |

Because of how actions and pipelines are executed, just whats happening inside the pipeline/action will be monitored, but in the grand scheme of things thats where our code lives, so its the right spot to start listening

## Who Is This For?

Ghost‑Based Sensors (GBS) is tailored for teams and individuals who:

- **Rely on Shared/Public Runners:** You only have access to GitHub Actions, Azure Pipelines, or other public CI/CD runners—not self‑hosted agents.  
- **Work with Unclassified or Public Code:** Your projects don’t handle classified data, but you still need strong observability.  
- **Can’t Deploy Dedicated Monitoring Services:** You lack the resources or permissions to stand up long‑running agents, SIEMs, or logging infrastructures.  
- **Want to Build Their Own Reports:** You prefer open‑source, customizable tooling to generate your own forensic and audit reports.  
- **Need to Meet Observability Controls:** You must comply with standards like NIST or Canada’s GC ITSG‑33 for visibility, logging, and traceability in your software supply chain.  


## What Ghost-Based Sensors Will NOT Do

* **Prevention or Enforcement:** GBS is strictly focused on visibility and traceability. While prevention and active blocking of threats are important, they are outside the scope of this project.
* **Private Pipeline Support:** Initially, GBS targets publicly accessible code and public CI/CD runners. Although GBS could be adapted for private environments, our initial implementation will not cover private pipelines or environments explicitly.
* **Runner Hardening:** GBS does not harden or secure CI/CD runners themselves; it focuses solely on capturing telemetry and maintaining traceability.

## How Does it Work?

GBS will deploy as a containerized sensor, injected into CI pipelines via a custom GitHub Action or Azure Pipeline Task. The sensor will:

1. Automatically activate at the start of each pipeline job.
2. Collect detailed telemetry data throughout the build and test steps.
3. Export telemetry logs as artifacts immediately before the ephemeral environment disappears.

## Example Pipeline

An **example pipeline** demonstrates how to integrate Ghost-Based Sensors into your CI workflow. You can find it at:

```text
.github/workflows/001-Example-Pipeline.yaml
```

This reference pipeline showcases:

* Starting Falco in **analyze** mode to capture full telemetry.
* Executing the normal job workload (e.g., checkout and a 10‑second count).
* Snapshotting installed **Node.js** and **Python** packages.
* Recording a **changed-files** list for provenance.
* Uploading package manifests and changed-files as artifacts.
* Stopping Falco at the end of the job.
* A subsequent **analyze** job that generates a detailed forensic report.

Use this pipeline as a template for your own public CI/CD workflows to harness the full power of GBS within ephemeral runners.

## Goals and Benefits

* **Full Pipeline Observability:** Transforms ephemeral runners into transparent, auditable environments.
* **Easy Integration:** Designed to be plug-and-play for popular CI providers, with minimal configuration.
* **Forensic Capability:** Provides critical post-event analysis to rapidly identify, diagnose, and respond to potential security incidents.

## Getting Involved

I'm starting fresh, and I want your help! If you're passionate about security, DevOps, or observability, your contributions can shape this innovative approach to pipeline visibility. Join me and help capture the ghostly world of ephemeral CI/CD runners.
