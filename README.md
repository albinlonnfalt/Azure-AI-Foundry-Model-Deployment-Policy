# Azure AI Foundry — Model Deployment Policy

TL;DR

This repository contains an Azure Policy definition that restricts which models (by provider) and SKUs can be deployed into an Azure AI Foundry (Cognitive Services) resource. Administrators supply a list of allowed model providers so new models from those providers are automatically allowed without per-model manual approvals.

Why this exists

- Reduce governance overhead by allowing entire trusted model providers instead of approving every model individually.
- Enable faster innovation for teams by removing manual approval bottlenecks.
- Improve cost effectiveness and consistency of deployments through SKU controls.

What’s in this repo

- `foundry-model-policy-definition.json` — the Azure Policy definition that checks deployments of type `Microsoft.CognitiveServices/accounts/deployments` and enforces allowed providers (`model.format`) and allowed SKUs (`sku.name`).

Policy details (quick)

- Scope: checks resources of type `Microsoft.CognitiveServices/accounts/deployments`.
- Parameters:
  - `effect`: Action to take when non-compliant. Valid values: `Audit`, `Deny`. Default: `Deny`.
  - `allowedModels`: Array of allowed model provider identifiers that map to the deployment property `model.format`. Default: `["OpenAI"]`.
  - `allowedSkus`: Array of allowed SKUs for deployments. Default includes `Standard`, `DataZoneStandard`, `DataZoneBatch`.

How to use

1. Inspect or edit `foundry-model-policy-definition.json` if you want different defaults.
2. Test in `Audit` mode first to see what would be denied, then switch to `Deny` when ready.
3. Create the policy definition and assign it to your desired scope (subscription or resource group).

Notes

- Use `Audit` first to monitor non-compliant deployments without blocking them.
- `allowedModels` values must match what the Foundry deployment sets in `model.format` (the policy default uses `OpenAI`). If you rely on different provider identifiers in your environment, add them to the list.
- You can assign the policy at subscription level to protect all Foundry resources, or to a resource group for narrower scope.

Benefits summary

- Less manual approvals; allow trusted providers at scale.
- Faster model rollout for teams while maintaining governance boundaries.
- Cost and capacity control by limiting SKUs.
