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
- https://github.com/NixOS/nix/issues/11228
- https://github.com/NixOS/nix/issues/11161
- https://github.com/NixOS/nix/issues/11101
- https://github.com/NixOS/nix/issues/11001

#### PRs

- https://github.com/NixOS/nix/pull/11746
- https://github.com/NixOS/nix/pull/11719
- https://github.com/NixOS/nix/pull/11506
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
- https://github.com/DeterminateSystems/nix-src/pull/125

## nixpkgs-review

nixpkgs-review should:

- be able to skip packages which use `requireFile` as their `src` instead of reporing them as broken every time

## Attic

[Attic](https://github.com/zhaofengli/attic) should:

- be refactored into a client, frontend (HTTP binary cache protocol), and backend (chunk store and NAR assembly)
- be able to run on Cloudflare via Workers and D1
- naive object storage (except maybe MinIO) will not be performant due to the number of small files being fetched simultaneously <https://blog.min.io/challenge-big-data-small-files/>