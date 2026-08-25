# eval-mcptox

**MCPTox: Tool Poisoning Attacks on Real-World MCP Servers**

> ⚠️ **Third-party eval.** This is a `register/` pointer in inspect_evals — the task code lives in an external repository of unaudited provenance and will execute on OpenEvalz infrastructure. Onboarding it is a security review, not a packaging task.

**Paper:** https://arxiv.org/abs/2508.14925

MCPTox measures whether tool-using LLM agents are manipulated by Tool Poisoning
Attacks, where a malicious instruction is hidden inside a tool's description (the
metadata an agent reads when planning) rather than in any executed code. Built on
45 live MCP servers and 353 authentic tools, it presents an agent with a benign
user query and a server whose tool set contains one poisoned tool, and measures
the Attack Success Rate via a model judge.

## At a glance

| | |
|---|---|
| Upstream | [`register/mcptox`](https://github.com/UKGovernmentBEIS/inspect_evals/tree/main/register/mcptox) |
| Group | — |
| Total samples | 0 |
| Execution class | `network` |
| Cost class | `low` |
| Flags | needs internet |
| Tags | Agent, Safeguards |

### Tasks

| Task | Samples |
|---|---|
| `mcptox` | 0 |

### External assets

_None declared upstream._

## Running one problem

OpenEvalz is problem-level: the atomic unit is a single sample, not the whole eval.

```bash
inspect eval inspect_evals/mcptox \
  --sample-id "<sample-id>" \
  --model openai-api/trustedrouter/<model> \
  --token-limit 200000
```

> **Two things that bite here, both verified in Inspect's source.**
>
> 1. **`--cost-limit` does not work on this routing path.** Inspect only records cost when its
>    pricing table resolves the model, and `_model_info.py` strips only `azure|bedrock|vertex`
>    prefixes — so `trustedrouter/<model>` never resolves and the cap silently never binds. The
>    real spend cap is enforced **server-side by TrustedRouter** via the delegated key's
>    `limit_microdollars` and spend window. Use `--token-limit` as the in-process bound.
> 2. **`--sample-id` matches with `fnmatch`.** A glob silently selects many samples and only warns.
>    Always pass a literal id.

## Reproducibility

`bundle.template.json` is the contract. A run that cannot emit a complete bundle does not publish.
Every image is pinned by `sha256` digest and every dataset by revision.

## Licensing

OpenEvalz wrapper code in this repository is **Business Source License 1.1** (see `LICENSE`) —
Licensor Lore Hex Corp, Change Date four years from publication, Change License Apache 2.0, no
Additional Use Grant. Same terms as TrustedRouter. Source-available, not open source: you may read,
modify and make non-production use of it, but production use needs a commercial licence
(licensing@openevalz.com).

**The packaged evaluation is NOT relicensed.** The task code, dataset and container images come from
upstream under their own terms — inspect_evals is MIT (UK AI Security Institute), and individual
datasets and images carry their own, sometimes unstated, licences. BSL covers only the OpenEvalz
packaging around them. See `NOTICE.md`, which must be completed before this repo publishes anything.
