## fs/cifs: send SMB_COM_FLUSH in cifs_fsync

In contrast to the now-obsolete smbfs, cifs does not send SMB_COM_FLUSH
in response to an explicit fsync(2) to guarantee that all volatile data
is written to stable storage on the server side, provided the server
honors the request (which, to my knowledge, is true for Windows and
Samba with 'strict sync' enabled).
This patch modifies the cifs_fsync implementation to restore the
fsync-behavior of smbfs by triggering SMB_COM_FLUSH after sending
outstanding data on the client side to the server.

I inquired about this issue in the linux-cifs-client@lists.samba.org
mailing list:

On Tue, Feb 10, 2009 at 12:35 AM, Jeff Layton <jlayton@redhat.com> wrote:
> Horst Reiterer wrote:
> > Why was the explicit SMB_COM_FLUSH dropped in the new implementation?
>
> It's not that it was "removed" per-se, just never implemented. Patches
> to add that capability would certainly be welcome.

I tested the patch with 2.6.28.6 and 2.6.18 (backported) on x86_64.

As of 2.6.30, the patch is part of mainline:

https://kernelnewbies.org/Linux_2_6_30

https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b298f223559e0205244f553ceef8c7df3674da74
