# PNPM packageExtension checksum error repro

If turbo rewrites the root `package.json` as part of the prune process
it doesn't respect the `packageExtensions` order, causing a checksum failure
when running `pnpm install --frozen-lockfile` on the generated output.

To make sure turborepo will rewrite the file I added a patch for a dependency 
of another workspace, which it'll filter out as part of `prune`.

Reproduction steps:

```
pnpm exec turbo prune --scope lib-a
cd out
pnpm install --frozen-lockfile
```

Output:
```
❯ pnpm exec turbo prune --scope lib-a
Generating pruned monorepo for lib-a in FILTERED/pnpm-package-extension-checksum-error-repro/out
 - Added lib-a
❯ cd out
❯ pnpm i --frozen-lockfile
Scope: all 2 workspace projects
 ERR_PNPM_LOCKFILE_CONFIG_MISMATCH  Cannot proceed with the frozen installation. The current "packageExtensionsChecksum" configuration doesn't match the value found in the lockfile

Update your lockfile using "pnpm install --no-frozen-lockfile"
```
