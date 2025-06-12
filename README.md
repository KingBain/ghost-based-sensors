# Ghost-Based Sensors (GBS): Observability for Ephemeral CI/CD Pipelines
Ghost-Based Sensors (GBS) is a lightweight observability layer designed for ephemeral CI/CD environments like GitHub Actions or Azure Pipelines. It provides compliance-grade visibility without requiring dedicated agents or full system access.

## Project Vision

Modern Continuous Integration and Delivery (CI/CD) pipelines are ephemeral in nature. Built Quick, Run Quick, Die Quick ... they vanishing once theyre completed their job. Yet these short lived environments remain largely unmonitored, creating a critical blind spot. Inspired by traditional "[Host-Based Sensors (HBS)](https://www.cyber.gc.ca/en/news-events/host-based-sensors)" and "[Cloud-Based Sensors (CBS)](https://www.cse-cst.gc.ca/en/accountability/transparency/reports/communications-security-establishment-annual-report-2022-2023)," I introduce **Ghost-Based Sensors (GBS)** which is my attempt to gain deeper visibility into these transient, short-lived build and deployment environments.

## Why Ghost-Based Sensors?

Ephemeral CI environments like GitHub Actions or Azure Pipelines spin up quickly on shared and public infrastructure, perform builds and tests, and disappear just as fast; _Do a double take - did I see a ghosts_. Traditional security monitoring tools designed for persistent infrastructure fail here. GBS workes to close or shrink this gap by capturing crucial telemetry, ensuring even short-lived pipelines activities are recorded.

## What Will Ghost-Based Sensors Do?

Leveraging lightweight instrumentation and eBPF-based telemetry, Ghost-Based Sensors will capture:

| Feature                 | Description                                                                                        | Method                  |
| ----------------------- | -------------------------------------------------------------------------------------------------- | ----------------------- |
| Process Telemetry       | Logs every process execution and command-line argument used during pipeline runs                   | Process Monitoring      |
| File System Activity    | Monitors file creations, deletions, and modifications to detect suspicious changes                 | File systyem monitoring |
| Network Connections/DNS | Records all outbound connections and DNS lookups to reveal data exfiltration or unauthorized calls | Network monitoring      |

The majority of the monitoring is done using Sysdig BPF probes into the github runner kernel, this is reffered to the ["Modern eBPF" probe](https://falco.org/blog/falco-modern-bpf-0-35-0/)

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

##  Getting Started

1. Add the GBS container or setup action to your CI job.
2. Launch the tracer via your container entrypoint or `pre` step.
3. Run your build/test/deploy steps as usual.
4. Stop sensors, collect data, and upload artifacts for analysis.


## Example Pipeline

An **example pipeline** demonstrates how to integrate Ghost-Based Sensors into your CI workflow. You can find it at:

```text
.github/workflows/001-Example-Pipeline.yaml
```

This reference pipeline showcases:

* Starting Sysdig in **analyze** mode to capture ephemeral system telemetry.
* Executing the normal job workload (e.g., checkout and a 10‑second count).
* Stopping Sysdig at the end of the job.
* Snapshotting installed **Node.js** and **Python** packages.
* Recording a **changed-files** log for Audit trail.
* Uploading package manifests, changed-files logs and scap style System Capture as artifacts.
* A subsequent **analyze** job(in the same action) that generates a detailed summary report.

Use this pipeline as a template for your own public CI/CD workflows to harness the full power of GBS within ephemeral runners.

##  GBS Architecture Diagram

### ASCII Fallback

```
           Github Job Start
                  |
                  v
   +--------------------------------+
   |    Start Sysdig Monitor        |
   +--------------------------------+
     |                      |
     v                      v
 +--------+           +----------------+
 | eBPF   |           | Normal CICD    |
 | Probe  |           | Jobs & Steps   |
 +--------+           +----------------+
     \                      |
      \                     v
       \------> Trace Events
                  |
                  v
   +--------------------------------+
   |   Stop Sysdig Entrypoint       |
   +--------------------------------+
                  |
                  v
        Upload Artifacts
   (capture, hashes, lists, etc.)
                  |
                  v
   +--------------------------------+
   |      Analyze Artifacts         |
   +--------------------------------+
                  |
                  v
         Generate Summary

```


## Goals and Benefits

* **Full Pipeline Observability:** Transforms ephemeral runners into transparent, auditable environments.
* **Easy Integration:** Plug-and-play support for popular CI providers with minimal configuration.
* **Forensic Capability:** Enables rapid post-event analysis to identify, diagnose, and respond to security incidents.
* **Pipeline Fingerprinting & Anomaly Detection:** Provides a framework for repository administrators to fingerprint CI/CD pipelines and author Falco rules to catch anomalies.


##  License

See [LICENSE](LICENSE) for details.

