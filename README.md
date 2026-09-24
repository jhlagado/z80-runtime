# Z80 Runtime

`@jhlagado/z80-runtime` is the independently versioned, UI-independent Z80
CPU and bounded-execution library used by Atom, Nucleus, Edit, Skate and other
Z80 development tools.

It does not depend on AZM, CP/M, TEC hardware, Glimmer, Visual Studio Code,
the Debug Adapter Protocol, a filesystem or a terminal. Machine and operating
system behaviour is supplied by callbacks owned by the host.

The package is ESM-only and requires Node.js 20 or newer.

Its repository is independent of the Debug80 editor and debugger. The current
TypeScript engine is a reference provider for a language-neutral runtime
contract; a Rust or WebAssembly provider must reproduce that contract rather
than silently replace debugger-visible semantics.

## Development

```sh
npm install
npm run check
```

The package was extracted with its path history from Debug80 commit
`6c8d0f19767166308bf6e3c9271c4d2aae0e309e`. Its first standalone commit starts
from the subtree split `6b3deccd6902cf4efe3997393730ba32dea0188a`.

## Runtime API

```ts
import { createZ80Runtime, parseIntelHex } from '@jhlagado/z80-runtime';

const runtime = createZ80Runtime(parseIntelHex(hex));

while (!runtime.isHalted()) runtime.step();
```

`parseIntelHex` returns a 64K image and write ranges. `createZ80Runtime` owns
CPU state and memory, calls the supplied port callbacks, and exposes bounded
single-step or breakpoint execution, register snapshots and reset. It does not
choose an operating system or device profile.

The package was extracted from the standalone Debug80 Runtime source at
`0024be1d868473568984fcde5a8323e575e595bf`. Platform-specific code remains in
that compatibility repository while consumers migrate to this CPU-first API.
