# AGENTS.md

## Project purpose

This repository contains one Home Assistant custom integration: **Calibrated
Appliance Monitor**.

The integration detects appliance cycles from smart-plug power and cumulative
energy data using appliance-specific algorithms calibrated from recorded traces.
Runtime code lives under:

    custom_components/calibrated_appliance_monitor/

Keep the repository focused on this integration.

## Repository boundary

This repository is intentionally separate from household Home Assistant
configuration.

Do not add installation-specific entity IDs, notification targets, tariff
entities, secrets, dashboards, automations, packages, or other private household
configuration. Notifications, pricing policy, and other site-specific behaviour
belong outside this repository unless a genuinely reusable integration feature
is explicitly requested.

Do not copy unrelated configuration or experiments into this repository.

## Working with the integration

Read the relevant files in full before modifying them. Preserve existing
behaviour unless the requested change requires altering it, and make the
smallest coherent change needed.

Preserve existing config-entry identity and entity `unique_id` values unless an
identity change is explicitly intended. Avoid migrations or compatibility
machinery unless they are actually needed.

Keep shared integration plumbing small. Appliance-specific thresholds and state
machines belong in `algorithms/`; do not generalise them merely because two
appliances have superficially similar behaviour.

## Calibration algorithms

Treat recorded appliance traces as the evidence for calibration changes.

When changing thresholds, debounce windows, timing, or classification rules:

- preserve the distinction between measured cycle boundaries and informative
  phase labels;
- prefer robust electrical signatures over programme names or displayed
  appliance timers;
- keep candidate/debounce bookkeeping out of the public entity model unless it
  is genuinely useful to Home Assistant users;
- explain non-obvious calibration choices in comments, especially where a
  threshold exists to reject a pattern seen in recorded traces.

Do not retune an appliance algorithm without evidence or an explicit request.

## Home Assistant conventions

Use normal Home Assistant config-entry and entity APIs. Keep the integration
usable by copying `custom_components/calibrated_appliance_monitor` into a Home
Assistant configuration directory.

Use readable, human-friendly entity names and sentence case where appropriate.
Keep public entities sparse and useful; diagnostic entities are appropriate for
calibration and troubleshooting.

Do not add dependencies, frameworks, generators, build systems, release tooling,
or CI merely for completeness. Add project machinery only when it has a clear
need.

## Comments and documentation

Comments should explain why calibration or lifecycle logic exists rather than
narrating obvious code.

Keep `README.md` focused on the repository and user-facing integration behaviour.
If supported appliances, setup, public entities, or diagnostics change, update
the documentation that describes them.

## Validation

When a Home Assistant environment with the `ha` CLI is available, validate with:

    ha core check

For Python-only changes, basic syntax checks are useful but do not substitute for
Home Assistant validation.

If full Home Assistant validation has not been performed, say so. Do not deploy
to a Home Assistant instance or restart Home Assistant unless explicitly asked.

## Git behaviour

Agents may interact with GitHub directly for requested repository work.

Never commit directly to the default branch. Before the first commit for a piece
of work, create a branch prefixed with `agents/`. If work is already on a
non-default branch, continue using it.

Commit and push requested changes, then open a pull request to the default branch
when ready for review unless the user explicitly asks not to.

Do not merge pull requests, force-push, rewrite branch history, create tags, or
create releases unless explicitly asked.

After pushing changes, report the branch, commit SHA, and pull request.
