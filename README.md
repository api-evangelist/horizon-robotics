# Horizon Robotics

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Horizon Robotics (Hong Kong Stock Exchange: 9660.HK, founded 2015) designs the Journey (征程)
family of automotive AI processors around its own BPU (Brain Processing Unit) architecture, and
ships the smart-driving software stack that runs on them — Horizon Mono ADAS, Horizon SuperDrive,
TogetheROS, Matrix, QoHo and AIDI.

**Horizon Robotics does not operate a hosted web API.** There is no OpenAPI document, no GraphQL
endpoint, no event surface, no pricing page and no self-serve signup. That was established by
probing, not assumed: `/openapi.json`, `/openapi.yaml`, `/swagger.json`, `/v1/openapi.json`,
`/api-docs`, `/redoc`, `/graphql` and the full `/.well-known/` set were fetched against six hosts
(`www.horizon.auto`, `developer.horizon.auto`, `doc.oe.horizon.auto`, `oe.horizon.auto`,
`chat.oe.horizon.auto`, `mcp.oe.horizon.auto`) on 2026-08-22 and every one either 404'd or returned
a single-page-app catch-all shell.

What Horizon *does* publish is two real agent surfaces on top of its OpenExplorer toolchain:

- **A live remote MCP server** at `https://mcp.oe.horizon.auto/mcp` — Model Context Protocol
  `2025-06-18`, serverInfo *Open Explorer MCP Server* 3.9.0, **no authentication required**. Four
  read-only tools (`list_codebases`, `search_code`, `search_doc`, `get_doc`) give an agent semantic
  retrieval over Horizon's indexed codebases and documentation library, plus three MCP resources.
  The verbatim `tools/list` and `resources/list` responses are saved in `mcp/`.
- **OE-Skills** — Horizon's own Apache-2.0 Agent Skill pack
  ([github.com/HorizonRobotics/OE-Skills](https://github.com/HorizonRobotics/OE-Skills), v0.2.0),
  37 `SKILL.md` files indexed as 29 skills across six modules, driving HBDK compilation, HMCT/Plugin
  quantization, UCP on-board inference and LLM compression from Claude Code, Codex or Cursor.
  Indexed in `skills/_index.yml`; nothing in this repository was authored on Horizon's behalf.

The developer contract for everything else is a command line, not an endpoint: `hb_compile`,
`hb_model_info`, `hb_verifier`, `hb_analyzer`, `hb_config_generator` and `hb_eval_preprocess`,
documented in `cli/`. The Python and C++ libraries behind them (`horizon_plugin_pytorch`,
`hbdk4-compiler`, `hmct`, `hbDNN`, …) are real first-party SDKs but ship only inside the
OpenExplorer package — none is on npm, PyPI, Maven Central, NuGet, pkg.go.dev, RubyGems, Packagist
or crates.io, all of which were probed. See `packages/`.

### Findings worth the company's attention

- `doc.oe.horizon.auto`, the canonical OpenExplorer manual linked from the developer portal, was
  serving an **expired TLS certificate** on 2026-08-22. Any client that verifies certificates —
  including most automated toolchains and agents — cannot read it.
- The product-security page tells reporters to encrypt with a PGP key and renders the unfilled
  template placeholders `[pgp-public-key.asc]` and `(insert key link)`. No key file is served.
- The product-security policy's in-scope list names `github.com/HorizonRDK` (now the D-Robotics
  spin-off) and `github.com/HorizonRobotics-Platform` (0 public repos), but not the active
  `github.com/HorizonRobotics` org, and not the anonymously reachable MCP endpoint.
- No `/.well-known/security.txt` on any host, despite a full disclosure policy existing.

### Not Horizon Robotics

**D-Robotics** (地瓜机器人 — `developer.d-robotics.cc`, `github.com/D-Robotics`, and the renamed
`github.com/HorizonRDK` org) is a **separate company**, spun off from Horizon Robotics in 2024 and
independently funded since. Its RDK developer kits, documentation and repositories are deliberately
**not** catalogued here and must not be attributed to Horizon Robotics.

### Links

- Website — https://www.horizon.auto/en
- Developer portal — https://developer.horizon.auto/
- Documentation — https://developer.horizon.auto/docs
- Forum — https://developer.horizon.auto/forum
- GitHub — https://github.com/HorizonRobotics
- Product security — https://www.horizon.auto/en/legal/security
