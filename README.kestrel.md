# Kestrel foot fork

Upstream: https://codeberg.org/dnkl/foot (MIT), pinned at 1.28.0.

The only source difference is the Kestrel per-task scheduling hint:

- `main.c` and `client.c`: hint the process main thread as term (2)
- `render.c`: hint each rendering worker thread as term
- `spawn.c`: hint the shell child before exec

`kestrel_hint.h` is the same ABI as `kestrel-distro/kestrel`'s
`libkestrel-hint`. On a stock kernel `sched_setattr` rejects the flag and the
calls are no-ops.

The package is `packaging/arch/PKGBUILD`, built as `foot-kestrel`, which
provides/conflicts `foot`. `kestrel-term.slice` and the hint both classify foot
as term; the cgroup is the primary path, the hint is the app-level fallback.
