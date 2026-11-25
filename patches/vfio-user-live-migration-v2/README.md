# vfio-user live migration — v2

Eight patches adding live migration support for vfio-user PCI devices through the existing VFIO migration infrastructure.

## Source

- Mailing list: [PATCH v2 0/8, 2026-09-11](https://lists.nongnu.org/archive/html/qemu-s390x/2026-09/msg00202.html).
- Cover Message-ID: `20260911140710.2348139-1-hugo.komatsu@nutanix.com`.
- Authors: William Henderson (1/8), Hugo Komatsu (2/8–8/8).
- Original base: `55347990687e7bc5b6b0d624f290025726e8fbfa`.
- Retrieved: 2026-11-25. Patch files preserve author metadata, Message-IDs, commit messages, sign-offs, and original diffs.

## Changes

| Patches | Function |
| --- | --- |
| 1–2 | Document migration commands, data transport, and DMA logging behavior. |
| 3–5 | Route device features, reset, and migration I/O through `VFIODeviceIOOps`. |
| 6 | Enable DMA logging for dirty page tracking. |
| 7 | Transfer migration data over the vfio-user socket. |
| 8 | Enable migration for vfio-user PCI devices. |

Read the numbered `.patch` files for the implementation and commit explanations. `series` lists their application order.

## Apply

From the QEMU project root, with a clean working tree and Git committer identity configured:

```bash
git am --message-id patches/vfio-user-live-migration-v2/000[1-8]-*.patch
```

This creates eight commits in numeric order. To resolve a conflict, edit the affected files, run `git add <file>`, then `git am --continue`. To cancel an unfinished application, run `git am --abort`.

To try the patches on a separate branch and worktree:

```bash
patch_dir="$PWD/patches/vfio-user-live-migration-v2"
git worktree add -b vfio-user-migration-v2 ../qemu-vfio-user-migration HEAD
git -C ../qemu-vfio-user-migration am --message-id "$patch_dir"/000[1-8]-*.patch
```

## Compatibility

All eight patches passed sequential application checks against the original base and local commit `005c61c0ef6f6ae615283440b4bb7be51b2c6ddd`. Actual `git am` application also passed in an isolated clone of the local commit. Build and migration runtime testing have not been performed.

The series uses stop-and-copy; device state pre-copy and P2P migration states are unsupported. Migration to older QEMU versions is not handled. A compatible device-side implementation is required; the author references this [SPDK WIP series](https://review.spdk.io/q/topic:vfio-user-live-migration-v2).
