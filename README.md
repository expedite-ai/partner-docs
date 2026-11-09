# Partner developer documentation

This is the public Mintlify site for approved partner integrations. Internal
repository documentation stays under `docs/`; only this directory is published.

The oRPC router and service schemas own the API contract. The backend's
`public-api.guide.ts` owns the integration guide. Regenerate `openapi.json` and
`guides/*.mdx` from a backend running this checkout:

```bash
node scripts/src/docs/sync-partner-api.mjs http://127.0.0.1:8481
node scripts/src/docs/sync-partner-api.mjs http://127.0.0.1:8481 --check
```

Use your dev stack's backend origin. Run these commands from the repository root
with Node 22.18 or newer. The export makes API and OAuth token URLs absolute for
hosting on a different domain. Webhook schemas use equivalent explicit string
enums and inclusive integer bounds because Mintlify misrenders untyped constants
and numeric exclusive bounds. The existing backend specification test compares
every exported operation, webhook, and generated guide against the runtime
owners; it requires no running backend or external credentials.

From this directory:

```bash
npx --yes mint@4.3.8 dev --no-open
npx --yes mint@4.3.8 validate
npx --yes mint@4.3.8 broken-links
```

The canonical source stays here. `partner-docs-publish.yaml` publishes this
committed directory from `main` to the public `expedite-ai/partner-docs` repository.
Connect Mintlify only to that publishing repository, branch `main`, directory `/`.
Do not edit the publishing copy or grant Mintlify private-monorepo access.

The monorepo Actions secret `PARTNER_DOCS_DEPLOY_KEY` holds an SSH deploy key with
write access only to `expedite-ai/partner-docs`. Publication runs on relevant
`main` changes or a manual dispatch on `main`, never on pull requests. The exporter
rejects symlinks and hidden files and copies only the committed docs subtree; no
application source, working-tree files or monorepo history is published. Verify
that boundary locally with `node --test .github/scripts/partner-docs-export.test.mjs`.
The initial reviewed documentation snapshot can be published during setup;
automatic updates start once the workflow is merged. Custom-domain DNS remains
a separate account-owner action.

The API playground uses simple mode: examples and schemas are visible, but the
site does not submit partner credentials or invite founders. The static guides
and API reference require no paid plan. Check Mintlify's current plan before
enabling optional hosted features.
