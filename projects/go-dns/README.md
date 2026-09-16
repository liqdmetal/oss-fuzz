# go-dns — fork-local provenance annotation

This directory is **upstream OSS-Fuzz's `projects/go-dns`**, byte-identical to
upstream master at the commit this fork is pinned to below — the fork has
modified nothing in it. This README is a fork-local annotation and is NOT part
of the upstream project definition; drop it when syncing or PRing upstream.

## Why it is here

`projects/go-dns` (fuzzing `github.com/miekg/dns`) is the native-Go OSS-Fuzz
project used as the working reference while building `projects/spore` — its
`build.sh` was the model for Go fuzz-target compilation in this repo, and it
contrasts usefully with the spore integration:

| | go-dns (upstream) | spore (fork-local) |
|---|---|---|
| Compiler macro | `compile_go_fuzzer` (v1 flow, hand-written `Fuzz` wrapper, `-tags fuzz`) | `compile_native_go_fuzzer` (native `go test -fuzz` targets via the go-118-fuzz-build shim) |
| Harness location | in-repo `_test.go` with a wrapper target | dedicated `internal/wirefuzz` package |
| Seed corpus | none committed (relies on `f.Add`/libFuzzer growth) | zipped corpus committed in spore (the shim makes `f.Add` a no-op) |

## Provenance pins

| What | Commit | Enforced where |
|---|---|---|
| This fork's upstream base (tree identical for `projects/go-dns`) | `1eb3684` | this repository's history |

The fuzzed project itself (`github.com/miekg/dns`) is cloned at build time and
is intentionally not pinned here — that is the upstream project definition's
own choice, and "fixing" it would diverge this directory from upstream.

## Local check

```sh
./.github/actions/doc-refs/verify_doc_refs.sh
```

Runs the vendored doc-refs action's verifier against the `docs/refs.yml`
manifest (which lists this README). Requires full history. To re-verify the
unmodified claim against upstream:

```sh
git diff origin/master master -- projects/go-dns   # empty = identical
```
