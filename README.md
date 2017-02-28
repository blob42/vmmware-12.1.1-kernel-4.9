## Patch for Vmware 12.1.1 Kernel 4.9

**Note: User at your own risk !**

Vmware 12.1 modules compilation is broke since kernel 4.9.
Here’s a quick and dirty patch I came up with after failing to find a solution.


### Usage

1. Go to vmware modules source directory
`cd /usr/lib/vmware/modules/source/`

2. 

https://github.com/torvalds/linux/commit/9beae1ea89305a9667ceaab6d0bf46a045ad71e7

- two variables(write, force) replaced with gup_flags
- gup flags used like this
unsigned int flags = 0;

flags |= FOLL_WRITE
1, 0, pvec + pinned ---> flags, pvec + pinned
https://github.com/torvalds/linux/commit/1e9877902dc7e11d2be038371c6fbf2dfcd469d7#diff-e37c5ffd9b4db050c3f7eae7d74e64c3R1230


write, force, pages --> flags(write=1,force=0)

flags: the flags must be write only and not force
FOLL_WRITE
https://github.com/torvalds/linux/blob/6e5c8381d1db4c1cdd4b4e49d5f0d1255c2246fd/include/linux/mm.h#L227://github.com/torvalds/linux/blob/6e5c8381d1db4c1cdd4b4e49d5f0d1255c2246fd/include/linux/mm.h#L2278
