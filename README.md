# Pre-registration commitments (November 2026)

This repository holds fingerprints published before data collection for a pre-registered single-case randomized study (November 2026 to February 2027). The full registration is held on OSF under embargo and will be made public with the results.

Each value below is a SHA-256 hash or a public seed. Once the materials are revealed, anyone can check that they match.

| Item | Value |
|---|---|
| Randomization seed commitment: the SHA-256 of the secret seed's 64 lowercase hex characters, as ASCII | `bd56f2f5ebd410c84c2f75064704ad2d9c933ae80f9465af63147a1f2af4384a` |
| Scheduler code (`study-core.js`) SHA-256 | `2cd92b03215c21b069201d2afa6305918770bf440d729361a3d2e45dd76cb4dd` |
| Server code SHA-256 (version 0.8.2; the files and their hashes, hashed together in sorted order) | `5e111a91ec707d4054d67c385d6405b7c80ee6a62a6fc4a8cbec7266d068a951` |
| Probe-schedule seed text SHA-256 | `af6fcee6b333960ffbb02619817dc535f641601fa4b3ae682e4224e1c7f1f8a5` |
| Monte Carlo seed (public) | `7843484276932513151` |
| Registration document SHA-256 (the file attached to the OSF registration; final after an independent review, replacing an earlier draft's `5decc5db…76db64`) | `99648bc21030af0f77f857e0a7175aa3d516e5916848318fa73ebe1e9f7f59fc` |
| Sealed list of the software models used (revealed with the results) SHA-256 | `87b4c82d3b32edafd389cf15e3f740ea366210875d874028553545267a30cb66` |

## How the schedule is derived from the seed

- **The seed:** 32 random bytes, kept as 64 lowercase hex characters. The commitment is the SHA-256 of those 64 characters as ASCII (`printf %s <seed> | sha256sum`).
- **Each triplet:** for each participant and each triplet t = 0, 1, 2, compute d = HMAC-SHA256(key = the seed's 32 bytes, message = `"<participant_id>|<t>"` in UTF-8).
- **The order:** the order index is (the first byte of d below 252) mod 6. If all 32 bytes are 252 or more, do the same over `"<participant_id>|<t>|<i>"` for i = 1, 2, …
- **The six orders, by index:**
  - 0 = sham, facts, live
  - 1 = sham, live, facts
  - 2 = facts, sham, live
  - 3 = facts, live, sham
  - 4 = live, sham, facts
  - 5 = live, facts, sham
- **The blocks:** block k (0 to 8) is the order of triplet floor(k / 3), at position k mod 3. Each block is one local calendar week, Monday to Sunday.

After the study, the seed is revealed. Its SHA-256 must equal the commitment above, and recomputing the derivation must reproduce the schedule that was run.

First committed 5 November 2026.
