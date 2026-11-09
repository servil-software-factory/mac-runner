# The Mac runner's exchange

Push a job file into `jobs/`; a Mac runs it and pushes its results into
`results/`. Nothing here is built or run by this repository itself: the
runner is BLAKE3-Servil's `tools/runner/runner.py`, on the team's Macs.

## To run a job

1. Push the commits the job names: a branch's, or a pull request's head
   (the runner fetches pull requests' heads too, from forks as well).
2. Take the next free number and write `jobs/NNNN-name.json`. The job
   kinds and their fields are the docstring at the top of
   [`runner.py`](https://github.com/servil-software-factory/BLAKE3/blob/next/tools/runner/runner.py):
   `benchmark`, `check`, `b3sum`, `crowd`, `mutants`, `example`, `test`.
   A `check` names the old and new BLAKE3-Servil commits, the bench-hashes
   commit and the points, for example:

       {"type": "check", "old_commit": "5d966a0c9f2f1f53bc53a266e7913addcb936873",
        "new_commit": "8625515f4a1bb16df6da600e5e24475158369281",
        "bench_commit": "decb4468c84391456fcd6d5174b2874232f623cd",
        "points": "continuous 64 B,continuous 4 KiB,continuous 1 MiB,continuous batch 1024"}

3. Commit and push. Within a minute a runner claims the job by pushing
   `results/NNNN-name.<time>-<machine>/claimed.json`; when it finishes,
   the folder holds `verdict.json` (the status), `runner.log` (every
   command and its output), and the job's samples and reports. Wait for it
   with `python3 tools/runner/wait_for.py NNNN-name PATH-OF-YOUR-CLONE`
   from a BLAKE3-Servil checkout, or pull and read `verdict.json`.

A job runs once per file name: a changed job takes a new number. The
machine's name is in the results folder's name, since results from
different machines never compare. Results are text (what a reader needs);
what a job built or fetched stays on the machine.

## Who may push

Members of the servil team. The runner pushes through a deploy key with
write access, one per machine.
