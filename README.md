# lean-certificate-checks

Independent, reproducible checks of public Lean certificates, run on GitHub-hosted runners.

One workflow (`.github/workflows/lean-certificate-check.yml`, started by hand) takes a certificate repository and a
commit, and runs [leanprover/comparator](https://github.com/leanprover/comparator) with the certificate's own
configuration: the solution's theorems must state exactly the challenge's, use only the permitted axioms, and pass
the Lean kernel and [nanoda](https://github.com/ammkrn/nanoda_lib). The solution is built inside
[landrun](https://github.com/Zouuup/landrun), after the challenge. A full `leanchecker --fresh` replay is optional.

Every download is pinned: elan and landrun by SHA-256, nanoda by commit, the certificate by commit. The logs and a
`facts.txt` (kernel, commit, exit codes, seconds) are bundled as `evidence.tgz`, uploaded, and attested.

## Verify a run's evidence

```sh
gh run download <run-id> -R wadeaaron312-NEO/lean-certificate-checks -n evidence
gh attestation verify evidence.tgz -R wadeaaron312-NEO/lean-certificate-checks
```

## Limits

- A check confirms what the Lean statements say, not that they encode the intended mathematical problem; that
  reading is separate work.
- comparator's README also runs it under `systemd-run` against an AF_UNIX escape; GitHub's runners are single-use
  virtual machines, and this workflow does not add that layer.

Not affiliated with the authors of any certificate checked here.
