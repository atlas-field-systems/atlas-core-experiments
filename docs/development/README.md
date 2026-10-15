# PStack development

Use the installed [PStack-T3 runtime](https://github.com/creedants/pstack-t3). The repository profile names its `poteto-mode`, `how`, `architect`, `interrogate`, `swarm` and `brigade` capabilities without vendoring the runtime. The profile is based on [upstream revision `4ee9e3c`](https://github.com/creedants/pstack-t3/tree/4ee9e3c73dcca16278ace6879164f0314e6b161d). Check the installed revision and available model catalog instead of assuming a capability exists.

## Initial workflow

Start with the specification-audit prompt in [PSTACK_START.md](../../PSTACK_START.md). Use appropriate investigation and adversarial review skills. Do not build code during that audit. Let findings drive a proposed plan instead of automatically adopting a historical implementation sequence.

## Later implementation

After the user authorizes a build increment, use the relevant PStack playbook to establish a real outcome, implement it and verify it. Choose internal design independently within accepted requirements. Keep concurrency, bookkeeping and review depth proportional to the task. A standing coordinator is optional and requires an authorized program, not merely these Markdown files.

One writer owns each isolated worktree. Review the exact commit and verify reviewer claims. Keep significant decisions in the runtime's local decision trail, promoting lasting authorized requirements into their proper documentation homes. Do not commit model settings, leases or transcripts as product documentation.

## Setup and verification

PStack is installed on the working machine, not by this repository. No runtime or coordinator has started merely because these instructions exist. The initial checkout has no application verifier. Follow [verification](verification.md) and create executable checks only as implementation is authorized.

PR publication and merge authority follow [Contributing](../../CONTRIBUTING.md#review-and-publication). Human landing is not permission to publish without a request.
