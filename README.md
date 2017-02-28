## Patch for Vmware 12.1.1 Kernel 4.9

**Note: User at your own risk !**

Vmware 12.1 modules compilation is broke since kernel 4.9.
Here’s a quick and dirty patch I came up with after failing to find a solution.


### Usage

1. Go to vmware modules source directory 

        cd /usr/lib/vmware/modules/source/

2. Extract **vmmon** and **vmnet** sources

        tar xf vmnet.tar
        tar xf vmmon.tar
        
3. In the same directory, move the patch files from this repo

        cp <path_to_this_repo>/vmmon-hostif.c/hostif.patch .
        cp <path_to_this_repo>/vmnet-user.c/userif.patch .
        
4. Apply patches

        patch -s -p0 < hostif.patch
        patch -s -p0 < userif.patch
        
5. Recreate archives

        tar -cf vmmon.tar vmmon-only
        tar -cf vmnet.tar vmnet-only
        
5. Recompile `sudo vmware-modconfig --console --install-all`


### Resources
Commits responsible for this error:

https://github.com/torvalds/linux/commit/9beae1ea89305a9667ceaab6d0bf46a045ad71e7
https://github.com/torvalds/linux/commit/1e9877902dc7e11d2be038371c6fbf2dfcd469d7#diff-e37c5ffd9b4db050c3f7eae7d74e64c3R1230

Ths signature of get_user_pages_remote changed, instead of passing (write=1,force=0) they are passed in a flag ( 0 |= FOLL_WRITE) 

https://github.com/torvalds/linux/blob/6e5c8381d1db4c1cdd4b4e49d5f0d1255c2246fd/include/linux/mm.h#L227://github.com/torvalds/linux/blob/6e5c8381d1db4c1cdd4b4e49d5f0d1255c2246fd/include/linux/mm.h#L2278


