# heroxcreed

Independent **AI-system reliability checkups** — fixed scope, evidence-first, by email.

I find the bugs other people's test suites miss: silent numerical corruption in AI
compute kernels, and retrieval / citation / injection failures in RAG systems.
Then I hand you a report with reproductions, root causes, and merge-ready fixes —
not a slide deck.

📧 **heroxcreed861@gmail.com** — email only, no calls, no video.

---

## Evidence (everything below is verifiable today)

### Merged upstream fixes — tenstorrent/tt-metal (4)

External contributor (`CONTRIBUTOR` on every PR) to Tenstorrent's open-source AI
compute stack. All merged 2026-09-16 / 2026-09-17. Each ships a torch-reference
before/after measurement, regression tests, and a coverage-gap analysis (the
project's nightly suite missed every one of these).

| PR | Fix | Maintainer triage |
|---|---|---|
| [#56200](https://github.com/tenstorrent/tt-metal/pull/56200) | `moreh_nll_loss_step2` 2d/3d reader defects — unguarded weight lookup (wild NOC read → NaN-poisoned loss) + face-local loop bound (CB page overrun) | **P1 · risk:high** |
| [#56202](https://github.com/tenstorrent/tt-metal/pull/56202) | `moreh_group_norm_backward` — gamma_grad read the wrong group's mean/rstd (silently wrong gradients) | **P1 · risk:high** |
| [#56203](https://github.com/tenstorrent/tt-metal/pull/56203) | `moreh_arange` row-major writer — last chunk overran the output buffer, corrupting the neighbouring DRAM tensor | **P1 · risk:high** |
| [#56334](https://github.com/tenstorrent/tt-metal/pull/56334) | `moreh_norm_backward` for `p = ±inf` — garbage gradients from undefined float→uint cast | P2 · risk:medium |

### Under review (4) — NOT claimed as merged

[#56074](https://github.com/tenstorrent/tt-metal/pull/56074) (P1/high) ·
[#56111](https://github.com/tenstorrent/tt-metal/pull/56111) (P1/medium) ·
[#56197](https://github.com/tenstorrent/tt-metal/pull/56197) (P1/high) ·
[#56386](https://github.com/tenstorrent/tt-metal/pull/56386) (P2/medium)

Bug reports with the same rigor (repro + measurements + suggested fix): e.g.
[#56938](https://github.com/tenstorrent/tt-metal/issues/56938) (28 unary ops
silently returning float32 bit patterns for int32 input) — triaged, labeled,
assigned to a Tenstorrent engineer, closed.

### What this proves — and what it does NOT

- **Proves:** low-level engineering — C++ device kernels, numerical correctness,
  finding *silent data corruption* that test suites miss, and writing it up with
  measurements.
- **Does NOT prove:** RAG/agent domain authority, client-service history, or
  employment. Those rest on separate evidence — kept separate on purpose.

### RAG reliability work

- 📄 **[Sample audit report](https://github.com/heroxcreed/heroxcreed/blob/main/SAMPLE-AI-Reliability-Checkup-Report-heroxcreed.pdf)** — full checkup report on a demo
  RAG system (clearly labeled SAMPLE; all client names and data fictional).
  Shows the method: scope → methodology → findings → remediation → fix verification.
- 🛠 **[rag-eval-kit](https://github.com/heroxcreed/rag-eval-kit)** — open-source,
  zero-dependency evaluation pipeline for RAG/agent systems: sampling,
  baseline metrics, quality gates, judge-consistency checks, client-readable
  report generation.

---

## Honesty block

- I am an **external open-source contributor**, not a Tenstorrent employee.
  Nothing here claims employment, client history, or authority beyond what the
  links show.
- All kernel verification was done on the **ttsim Blackhole simulator — no
  physical hardware**; limits are stated in the PR bodies, not hidden.
- The tt-metal record proves low-level numerical-correctness ability. RAG
  capability is demonstrated separately by the sample report and the eval kit —
  one claim, one piece of evidence.
- **No client cases yet** — that's what the sample report and the open-source
  kit are for.
- Contact is **email only** (heroxcreed861@gmail.com). No calls, no video, no
  in-person meetings.
