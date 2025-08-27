# ideas

Just general ideas, about things.

## Nix

Nix should:

- A way to guard against evaluation under OfBorg or nixpkgs-review, both of which allow broken, that does not involve setting badPlatforms

### Issues and PRs to follow

#### Issues

- https://github.com/NixOS/nix/issues/11712
- https://github.com/NixOS/nix/issues/11683
- https://github.com/NixOS/nix/issues/11642
- https://github.com/NixOS/nix/issues/11557
- https://github.com/NixOS/nix/issues/11555
- https://github.com/NixOS/nix/issues/11359
- https://github.com/NixOS/nix/issues/11268
- https://github.com/NixOS/nix/issues/11228
- https://github.com/NixOS/nix/issues/11161
- https://github.com/NixOS/nix/issues/11101
- https://github.com/NixOS/nix/issues/11001

#### PRs

- https://github.com/NixOS/nix/pull/11744
- https://github.com/NixOS/nix/pull/11746
- https://github.com/NixOS/nix/pull/11719
- https://github.com/NixOS/nix/pull/11506
- https://github.com/NixOS/nix/pull/11373
- https://github.com/NixOS/nix/pull/11294
- https://github.com/NixOS/nix/pull/11143
- https://github.com/NixOS/nix/pull/11130
- https://github.com/NixOS/nix/pull/10937
- https://github.com/NixOS/nix/pull/10590
- https://github.com/NixOS/nix/pull/10511
- https://github.com/NixOS/nix/pull/10505
- https://github.com/NixOS/nix/pull/10280
- https://github.com/NixOS/nix/pull/10218
- https://github.com/NixOS/nix/pull/10201
- https://github.com/NixOS/nix/pull/9967
- https://github.com/NixOS/nix/pull/9895
  - pennae mentions reducing the size of a Value to a single tagged pointer
- https://github.com/NixOS/nix/pull/9551
- https://github.com/NixOS/nix/pull/9429
- https://github.com/NixOS/nix/pull/9287
- https://github.com/NixOS/nix/pull/9145
- https://github.com/NixOS/nix/pull/8585
- https://github.com/NixOS/nix/pull/8105
- https://github.com/NixOS/nix/pull/7247
- https://github.com/NixOS/nix/pull/6855

### Single-threaded evaluation speed

Idea one: allocate memory in bulk — since lists are strict in their length, and attribute sets are strict in their keys. So for lists for example, the list builder would allocate a contiguous block of memory for pointers to values, and another contiguous block of memory for the values themselves. To implement that, I’d introduce a new `allocValues` function in `eval-inline.hh` which allocates multiple values and *then* increments the global counter for number of values. (`allocValue` increments it for each call.)

Idea two: Looking at the builtins handling lists or attribute sets, it looks like there’s a fair amount of pointer arithmetic, referencing, and dereferencing going on inside for loops. With something like `allocValues` allocating memory ahead of time (and handling incrementing the global variable for number of elements), I thought using OpenMP’s SIMD pragma on some of the for loops concerning pointers in the builtins might improve performance — provided the length of the list or attribute set is larger than some threshold.

Idea three: Regardless of bulk memory/ value allocation, I understand cache lines in modern processor architectures are very important. The current Value structure weighs in at 24 bytes on my x64 machine: 8 bytes for the enum with padding and 16 bytes for the actual payload. I’m curious if there would be benefits to getting it down to 16 bytes.
I had thought of either using something like tagged pointers, though that introduces the need for bit twiddling and much stronger encapsulation than what Value current has.
Another alternative was to keep the enum, but change the payload to 8 bytes: everything that can fit in the 8 bytes is inlined (Null, Boolean, Integer, Float, string *without* context (which is just a char pointer), empty/singleton list, etc.), while everything else is represented as a reference to that larger structure (path, string *with* context, lists of two or more elements, attribute sets, etc.). This avoids the need for bit twiddling, but introduces additional dereferences — I’m not sure how expensive those are on modern processors.

## nixpkgs-review

nixpkgs-review should:

- be able to skip packages which use `requireFile` as their `src` instead of reporing them as broken every time

## Attic

[Attic](https://github.com/zhaofengli/attic) should:

- be refactored into a client, frontend (HTTP binary cache protocol), and backend (chunk store and NAR assembly)
- be able to run on Cloudflare via Workers and D1
- naive object storage (except maybe MinIO) will not be performant due to the number of small files being fetched simultaneously <https://blog.min.io/challenge-big-data-small-files/>