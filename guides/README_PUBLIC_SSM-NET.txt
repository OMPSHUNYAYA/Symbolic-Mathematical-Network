README — Shunyaya Symbolic Mathematical Network (SSM-NET)
Status: Public Research Release (v2.1)
Date: November 10, 2025
Caution: Research/observation only. Not for critical decision-making.
License: Open standard. Free to implement with no fees or registration, provided strictly “as-is” with no warranty, no endorsement, and no claim of exclusive stewardship.
Citation: "Shunyaya Symbolic Mathematical Network (SSM-NET)" (concept origin).

What SSM-NET Is (one-liner)
A manifest-first overlay that rides beside existing traffic and turns ordinary messages into self-verifying declarations while keeping payload bytes unchanged.

Core Invariants (copy-ready)

Payload invariance. phi((m,a)) = m (original bytes are sacrosanct).

Deterministic bounded alignment.
a_c := clamp(a_raw, -1+eps_a, +1-eps_a) -> u := atanh(a_c) -> U += w*u ; W += w -> align := tanh( U / max(W, eps_w) ).

Policy -> duty. band comes from declared cutpoints in manifest_id; timing windows and escalation are explicit.

Canonical subset commitment. sha256=<hex> over a published byte recipe (canonical subset).

Continuity stamp. SSMCLOCK1|iso_utc|nonce|sha256=<canonical_subset>|prev=<prior_sha256> (ordering and deletion become obvious).

Disclosure defaults. Label-first by policy; align disclosed only when required.

Minimal Declaration (illustrative shape)

{
  value,          // unchanged application magnitude(s)
  align,          // optional per policy; in (-1,+1)
  band,           // required action stance (from manifest)
  manifest_id,    // pointer to the active rulebook
  sha256,         // commitment to declared canonical subset HEAD
  stamp           // SSMCLOCK1|iso_utc|nonce|sha256=<...>|prev=<...>
}


Canonical Subset (byte recipe — summary)

Fields. A compact JSON object containing value, band, and manifest_id in a fixed order with exact ASCII serialization (no trailing spaces; stable quoting).

Hashing. sha256(utf8_bytes(compact_json)) -> 64-hex HEAD.

Stamp. Insert the HEAD into stamp as sha256=<HEAD>.

Continuity. Next packet sets prev=<prior_HEAD>.

Manifest & Bands (policy — summary)

manifest_id names the active rulebook.

Cutpoints define labeled ranges over align, e.g., A++, A0, CRITICAL.

Windows/Actions (optional): escalation timers, retry windows, audit labels.

Determinism: identical inputs -> identical band.

One-Minute Acceptance (receiver / intermediary)

Check align in (-1,+1) if present.

Recompute SHA256(canonical_subset) -> equals sha256.

Re-map band via manifest_id cutpoints.

Verify chain: prev linkage and monotonic iso_utc.

Enforce disclosure: label-first vs full, as policy dictates.

Evidence & Parity (operational notes)

Envelope log. Append each canonical subset line to a newline-delimited ledger.

Hash list. Append each HEAD in order.

Checkpoint. Store HEAD=<64-hex> (optionally publish at a well-known endpoint).

Parity script. Recompute digests over the ledger; compare to the saved hash list; print ALL CHECKS PASSED on parity success.

Quickstart (micro flow)

Build canonical subset -> sha256=<HEAD>.

Emit packet with headers and stamp.

On next packet, set prev=<HEAD> to maintain continuity.

Receiver recomputes and checks (steps above).

Periodically publish/update manifest_id JSON.

Security & Interop Hints

Keep canonical subset minimal and stable across versions.

Treat manifests as auditable rulebooks (signed/published if needed).

Prefer label-first disclosure; reveal align only when policy requires.

Byte-for-byte payload stability is non-negotiable (phi((m,a)) = m).

For sensitive disclosures over HTTP-like transports, SHOULD use a secure channel.

Changes in v2.1 (summary)

Clarified Quick Help language and continuity verification.

Explicit guidance for data URLs / local file parity in validation flows.

Emphasis on exact ASCII serialization for the canonical subset.

One-Line Takeaway
Keep your bytes; add a tiny, stamped declaration. Make duty and proof travel with every message.

Attachment Index (this release package)

Brief: Brief-SSM-NET_ver2.1.pdf

Full Spec: SSM-NET_ver2.1.pdf

Demo (one-file HTML): SSMNET_demo_v5_1.html

Spec Supplements (TXT/JSON):

CANONICAL_SUBSET_RECIPE_SSM-NET.txt

EFFECTIVE_MANIFEST_SSM-NET_v2_1.json

GETTING_STARTED_SSM-NET.txt

MANIFEST_AND_BANDS_SSM-NET.txt

OUTPUT_EXAMPLES_SSM-NET.txt

STAMP_SPEC_SSM-NET.txt

README_PUBLIC_SSM-NET.txt

Evidence Bundle: SSM-NET_evidence_v2_1.zip (minimal parity artifacts)

manifests.json

envelopes.jsonl

hashes.txt

checkpoint.txt

verify.sh

How To Verify Locally (quick parity check)

Generate at least one packet in the demo, then download the evidence files.

Place all five evidence files in the same directory.

Run: bash verify.sh

Expect: ALL CHECKS PASSED (plus continuity notes).