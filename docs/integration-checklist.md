
# Integration Checklist

**Sentinel Core v2.0 — Pilot Deployment Guide**

**Date:** July 2026  
**Contact:** [@tec_support_bot](https://t.me/tec_support_bot)

---

## Overview

This checklist guides the integration of Sentinel Core into a quantum computing control system from initial assessment through production deployment. Typical pilot timeline: **8–12 weeks**.

| Phase | Duration | Deliverable |
|-------|----------|-------------|
| Phase 0: Assessment | Week 1 | Platform threat model, hardware inventory |
| Phase 1: Hardware Integration | Weeks 2–4 | Sentinel modules installed, boot chain verified |
| Phase 2: Telemetry Integration | Weeks 5–7 | Sensors signed, panic thresholds calibrated |
| Phase 3: EHM Calibration | Weeks 8–9 | Entropy sources validated, panic response tested |
| Phase 4: Deterministic Execution | Weeks 10–11 | Control patterns verified, jitter measured |
| Phase 5: Validation & Handover | Week 12 | Security audit, documentation, team training |

---

## Phase 0: Assessment (Week 1)

### 0.1 Platform Identification

- [ ] Confirm quantum platform type: Neutral Atoms / Superconducting / Trapped Ions / Photonic
- [ ] Document qubit count (physical and logical targets)
- [ ] Identify control hardware: FPGA vendor/model, AWG/DDS vendor, ADC specifications
- [ ] Document existing security measures (if any)

### 0.2 Threat Model Review

- [ ] Identify primary attack vectors for this platform (see [Platform Adaptations](platform-adaptations.md))
- [ ] Document existing vulnerabilities in boot chain, telemetry, entropy sources
- [ ] Define acceptable risk threshold (what constitutes a "panic-worthy" event)
- [ ] Map Sentinel modules to identified threats

### 0.3 Hardware Inventory

| Component | Vendor | Model | Interface | Sentinel Compatibility |
|-----------|--------|-------|-----------|----------------------|
| | | | | |
| | | | | |
| | | | | |

### 0.4 Deliverables

- [ ] Completed threat model document
- [ ] Hardware compatibility matrix
- [ ] Integration proposal with timeline and milestones

---

## Phase 1: Hardware Integration (Weeks 2–4)

### 1.1 Secure Element Installation

- [ ] Install secure element (Microchip ATECC608 / Infineon OPTIGA / Xilinx HSM)
- [ ] Verify electrical interface (I²C / SPI / dedicated bus)
- [ ] Confirm galvanic isolation from quantum system (if required)
- [ ] Generate device keypair; store public key in Sentinel key registry

### 1.2 FPGA Boot Chain Modification

- [ ] Integrate Sentinel bootloader into FPGA boot ROM
- [ ] Configure signature verification algorithm (ECDSA P-256 / RSA-2048)
- [ ] Program fallback flash with verified "golden" bitstream
- [ ] Test boot sequence: valid signature → normal boot; invalid signature → fallback boot

### 1.3 Glitch Detection Setup

- [ ] Install voltage glitch detectors on FPGA power rails
- [ ] Install clock glitch detectors on system clock
- [ ] Configure glitch threshold (typical: ±5% voltage, ±10% clock)
- [ ] Test glitch response: verify fallback flash activation

### 1.4 Deliverables

- [ ] Successful boot with verified bitstream
- [ ] Successful fallback boot after simulated glitch attack
- [ ] Boot latency measurement (< 50 ms target)

---

## Phase 2: Telemetry Integration (Weeks 5–7)

### 2.1 Sensor Mapping

| Sensor ID | Physical Parameter | Location | ADC Channel | Sentinel Action |
|-----------|-------------------|----------|-------------|----------------|
| | | | | |
| | | | | |
| | | | | |

### 2.2 ADC Integration

- [ ] Connect secure element to ADC data line (inline or parallel)
- [ ] Configure signing protocol: per-sample or per-batch
- [ ] Verify signature latency (< 100 µs target)
- [ ] Test signature verification on control processor

### 2.3 Panic Threshold Calibration

- [ ] Define normal operating range for each sensor
- [ ] Set panic thresholds (see [Platform Adaptations](platform-adaptations.md) for defaults)
- [ ] Test panic trigger: simulate out-of-range reading, verify hardware panic
- [ ] Measure panic latency (< 1.5 µs for Tier 1, 10–50 µs for Tier 2)

### 2.4 Telemetry Logging

- [ ] Configure signed telemetry log storage
- [ ] Verify log integrity (tamper-evident chain)
- [ ] Test log retrieval for forensic analysis

### 2.5 Deliverables

- [ ] All sensors producing signed telemetry
- [ ] Panic response verified for each sensor type
- [ ] Telemetry latency within specification

---

## Phase 3: EHM Calibration (Weeks 8–9)

### 3.1 Entropy Source Characterization

- [ ] Identify primary entropy source (optical / electronic / thermal)
- [ ] Measure raw entropy rate (bits/second)
- [ ] Verify physical mechanism (shot noise / tunneling / thermal)

### 3.2 Tier 1: Physical Health Monitor

- [ ] Install analog health monitor circuit
- [ ] Calibrate baseline parameters (variance, mean, spectral density)
- [ ] Set anomaly thresholds (typically ±3σ from baseline)
- [ ] Test: simulate entropy degradation, verify < 1.5 µs panic response

### 3.3 Tier 2: Statistical Validation

- [ ] Configure sliding window size (1024–65536 samples)
- [ ] Select statistical tests (AIS-31 PTG.3 / NIST SP 800-90B)
- [ ] Calibrate test thresholds (p-value > 0.001 for pass)
- [ ] Test: inject biased sequence, verify 10–50 µs panic response

### 3.4 Fail-Safe Verification

- [ ] Verify hardware panic isolates compromised entropy stream
- [ ] Confirm fallback entropy source activation (if redundant)
- [ ] Test recovery procedure: entropy source restart, re-validation

### 3.5 Deliverables

- [ ] EHM baseline report (entropy rate, health parameters)
- [ ] Panic response verified for both tiers
- [ ] Fail-safe and recovery procedures documented

---

## Phase 4: Deterministic Execution (Weeks 10–11)

### 4.1 Control Pattern Verification

- [ ] Identify all control patterns (gates, measurements, calibration)
- [ ] Generate pattern signatures (hash + ECDSA sign)
- [ ] Integrate signature verification into pattern load sequence
- [ ] Test: invalid signature → pattern rejected; valid signature → pattern executed

### 4.2 Interrupt Isolation

- [ ] Audit all interrupt sources on control FPGA
- [ ] Disable non-critical interrupts during quantum operations
- [ ] Configure critical interrupt whitelist (if any)
- [ ] Verify: no unexpected IRQs during 1-hour continuous operation

### 4.3 Jitter Measurement

- [ ] Define measurement points (AWG output, DDS clock, FPGA control line)
- [ ] Measure baseline jitter without Sentinel
- [ ] Measure jitter with Sentinel active
- [ ] Verify: added jitter < 1 ps RMS (target)

| Measurement Point | Baseline Jitter | With Sentinel | Added Jitter | Pass/Fail |
|-------------------|-----------------|---------------|--------------|-----------|
| | | | | |
| | | | | |

### 4.4 Side-Channel Resistance

- [ ] Verify constant-time pattern dispatch (no data-dependent branches)
- [ ] Measure power consumption during pattern execution
- [ ] Verify no power correlation with secret parameters

### 4.5 Deliverables

- [ ] All control patterns signed and verified
- [ ] Jitter measurement report within specification
- [ ] Side-channel resistance verified

---

## Phase 5: Validation & Handover (Week 12)

### 5.1 Security Audit

- [ ] Penetration test: simulated bitstream injection
- [ ] Penetration test: simulated telemetry spoofing
- [ ] Penetration test: simulated entropy degradation
- [ ] Penetration test: simulated timing attack
- [ ] Document all findings and mitigations

### 5.2 Performance Validation

| Metric | Target | Measured | Pass/Fail |
|--------|--------|----------|-----------|
| Boot verification latency | < 50 ms | | |
| Telemetry signing latency | < 100 µs | | |
| EHM Tier 1 panic latency | < 1.5 µs | | |
| EHM Tier 2 panic latency | 10–50 µs | | |
| Added control jitter | < 1 ps RMS | | |
| Fallback flash swap | < 200 ms | | |

### 5.3 Documentation

- [ ] System architecture diagram (customer-specific)
- [ ] Operational procedures (startup, shutdown, panic response)
- [ ] Troubleshooting guide
- [ ] Integration with existing monitoring infrastructure

### 5.4 Team Training

- [ ] Security awareness training for operators
- [ ] Technical training for system administrators
- [ ] Emergency response drill (simulated attack)
- [ ] Handover sign-off

### 5.5 Deliverables

- [ ] Security audit report
- [ ] Performance validation report
- [ ] Complete documentation package
- [ ] Training completion certificates

---

## Post-Pilot: Production Licensing

Upon successful pilot completion:

1. **License Agreement** — Annual production license with SLA
2. **Source Code Escrow** (optional) — Third-party deposit for business continuity
3. **Ongoing Support** — Security updates, threat intelligence, platform upgrades
4. **Expansion** — Fleet licensing for multiple quantum systems

**Contact for next steps:** [@tec_support_bot](https://t.me/tec_support_bot)

---

## Quick Reference: Go/No-Go Gates

| Gate | Criteria | Checkpoint |
|------|----------|------------|
| G0 | Hardware compatibility confirmed | End of Week 1 |
| G1 | Secure boot operational | End of Week 4 |
| G2 | Telemetry signed and verified | End of Week 7 |
| G3 | EHM panic response validated | End of Week 9 |
| G4 | Jitter within specification | End of Week 11 |
| G5 | Security audit passed | End of Week 12 |

---

*Sentinel Core — Protecting quantum coherence at the classical boundary.*
```
