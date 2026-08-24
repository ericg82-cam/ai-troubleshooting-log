# Case Study 01 — Getting ComfyUI + Wan 2.2 running on a low-VRAM GPU

**Goal:** Run a local AI video generation pipeline (ComfyUI with the Wan 2.2 TI2V-5B workflow) on a Windows machine with an RTX 3050 — a GPU most people say can't handle it.

**Time to working state:** hours, not days.

---

## The problem

Fresh ComfyUI portable install on Windows. Three separate failures stacked on top of each other:

1. **Network/DNS** — the app couldn't reach what it needed to download during setup.
2. **Antivirus false positive** — security software flagged and quarantined part of the install.
3. **VRAM ceiling** — the RTX 3050 doesn't have the memory the workflow assumes by default, so generation would fail or crash.

Any one of these looks like "it's broken." Together they look hopeless. The skill is separating them.

## How I diagnosed it

I treated each symptom as its own hypothesis instead of assuming one root cause:

- **DNS:** traced the failure to the gateway's DNS resolution rather than the app itself. Confirmed by testing whether the machine could resolve the addresses at all.
- **Antivirus:** matched the timing of the "missing file" errors to the quarantine event — the file wasn't missing, it had been pulled after download. Classic false positive.
- **VRAM:** identified that the default workflow config exceeds the card's memory, which is a configuration problem, not a hardware dead end.

## The fix

1. **DNS** — pointed the machine at a reliable public DNS resolver so downloads completed.
2. **Antivirus** — restored the quarantined file and added an exclusion so it wouldn't get pulled again.
3. **VRAM** — launched ComfyUI with the `--lowvram` flag and confirmed the model files were placed in the correct folders for the Wan 2.2 TI2V-5B workflow.

Result: a working local text/image-to-video pipeline on hardware that "shouldn't" run it.

## Why it matters

Nobody handed me a guide for this exact combination. The value wasn't any single command — it was refusing to accept "it's broken," isolating three tangled failures, and verifying each fix independently. That's the same instinct whether the target is ComfyUI, a production tool, or a customer's environment.

## Transferable skill

Isolating stacked failures · GPU/VRAM constraints · Windows environment/driver issues · reading errors as evidence, not verdicts.
