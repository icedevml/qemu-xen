# qemu-xen 4.22.0 for DRAKVUF (temporary fork)

**Use branch [`qemu-xen-4.22.0-drakvuf1`](https://github.com/icedevml/qemu-xen/tree/qemu-xen-4.22.0-drakvuf1).**

Stock qemu-xen `qemu-xen-4.22.0` (QEMU 9.1, the version Xen 4.22.0 uses) plus a backport of the
upstream QEMU fix for a deadlock between IDE TRIM and block drain. It is hit when Xen asks QEMU to
invalidate its mapcache (`xen_invalidate_map_cache()` → `bdrv_drain_all()`) while a TRIM is in
flight, and froze DRAKVUF analyses at random.

Backported upstream commits (Kevin Wolf):

- [34a6763776](https://github.com/qemu/qemu/commit/34a6763776) block: Add blk_co_start/end_request() and BDRV_REQ_NO_QUEUE
- [53074ba033](https://github.com/qemu/qemu/commit/53074ba033) block: Add flags parameter to blk_*_pdiscard()
- [095c08a7ba](https://github.com/qemu/qemu/commit/095c08a7ba) ide: Minimal fix for deadlock between TRIM and drain
- [c1c71a7e16](https://github.com/qemu/qemu/commit/c1c71a7e16) ide: Clean up ide_trim_co_entry() to be idiomatic coroutine code

Used by the Xen fork [icedevml/xen](https://github.com/icedevml/xen) (branch
`RELEASE-4.22.0-drakvuf1`), which builds this QEMU automatically.
Background: [tklengyel/drakvuf#1858](https://github.com/tklengyel/drakvuf/issues/1858).

This fork is temporary and will be removed once the fix is in a Xen release.
