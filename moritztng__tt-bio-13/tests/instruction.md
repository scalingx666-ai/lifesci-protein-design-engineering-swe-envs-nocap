# Issue

**Title:** Surface a worker's native stderr on failure instead of /dev/null

## Description

.

### Problem

`_silence_subprocess_output()` redirects **fd 2 to `/dev/null`**, so a fatal that never reaches Python — a C-level abort such as an **`MPI_Init` failure**, which writes to fd 2 and exits without unwinding — is lost. The launcher then raises:

```
every local worker exited before the run finished (SpawnProcess-1 exit 14); no job can be served. The worker's own traceback above says why.
```

…but nothing is above it. `_report_fatal` only ever fires for Python exceptions; a native abort exits before it runs. The cause is visible only with `--debug` (which skips silencing). The existing docstring already flags this class of bug ("writing that fatal to /dev/null is what left a device-open failure invisible for hours") — this extends that intent to native aborts.

### Change

- `_silence_subprocess_output()` now sends **stdout** to `/dev/null` (genuine per-op noise) but **fd 2 to a per-worker capture file** (`$TMPDIR/tt-bio-worker-<pid>.stderr`), so native fatals survive. The real-stderr dup is still kept, so `_report_fatal` surfaces Python fatals to the terminal immediately, unchanged.
- On the all-workers-dead path, `_stream_run` reads each dead worker's captured stderr tail (`read_worker_capture`, keyed by `proc.pid`) and appends it to the error, so the cause is visible **without `--debug`**.
- The capture file is removed on a clean/Python-level exit and consumed by the launcher otherwise, so `/tmp` is not littered.
- Adds a host-only test (`test_silenced_worker_native_stderr_is_captured`) — a silenced worker's native fd-2 write survives `os._exit`.

### After

```
RuntimeError: every local worker exited before the run finished (SpawnProcess-1 exit 14); no job can be served.
--- SpawnProcess-1 (exit 14) stderr tail ---
*** An error occurred in MPI_Init_thread
*** MPI_ERRORS_ARE_FATAL ... Local abort before MPI_INIT completed
```

### Verification

- Repo-main patch compiles; the added test passes; the fd-2 capture mechanism verified standalone (a native `os.write(2, …)` after silencing survives `os._exit(14)` and is read back).
- **Reproduced and verified on-device** (Blackhole p150a, tt-bio 0.7.2): with a host OpenMPI (`OMPI_MCA_pml=ucx`) breaking the bundled MPI, stock 0.7.2 gives the opaque `exit 14`; the same logic ported onto 0.7.2 surfaces the `MPI_Init` abort inline with no `--debug`, a normal fold still succeeds (`Done: 1 ok`), and no capture files are left behind.

Scoped to the crash-surfacing fix only. The preflight-warning and docs items from #12 are left for follow-ups.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d55d08fb98e6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.