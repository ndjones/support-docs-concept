It is important to understand the different I/O performance
characteristics of nodes that connect to storage using *native Spectrum
Scale clients*, and those that employ *Cray's DVS* *solution*.

Applications that make heavy demands on metadata services and or have
high levels of small I/O activity should generally not be run on
[Māui](https://support.nesi.org.nz/hc/articles/360000163695) (the Cray
XC50).

Nodes which access storage via native Spectrum Scale Clients {#nodes-which-access-storage-via-native-spectrum-scale-clients .western}
------------------------------------------------------------

All [Mauhika](https://support.nesi.org.nz/hc/articles/360000163575) HPC
Cluster, [Mahuika
Ancillary](https://support.nesi.org.nz/hc/articles/360000163595), [Māui
Ancillary](https://support.nesi.org.nz/hc/articles/360000203776) and
Māui login (aka build) nodes have native Spectrum Scale clients
installed and provide high performance access to storage:

-   Metadata operations of the order of 190,000 file creates /second to
    a unique directory can be expected;
-   For 8MB transfer size, single stream I/O is \~3.3GB/s Write and
    \~5GB/s Read;
-   For 4KB transfer size, single stream I/O is \~1.3GB/s Write and
    \~2GB/s Read.

Nodes which access storage via DVS {#nodes-which-access-storage-via-dvs .western}
----------------------------------

Māui (XC50) utilizes a file system projection method via software, known
as DVS (Data Virtualisation Service), to expose the Spectrum Scale file
systems to XC compute nodes. DVS adds an additional layer of hardware
and software between the XC compute nodes and storage (see Figure).

 ![cray\_xc50.jpg](https://support.nesi.org.nz/hc/article_attachments/360000486995/cray_xc50.jpg)

Figure 1: Cray XC50 DVS architecture.

This *reduces the I/O performance of Māui for metadata and small (e.g.
4KB) I/O operations*. It does not impact the total bandwidth available
from the Māui (i.e. \~130 GB/s when writing to the filesystem).
Accordingly, the equivalent performance numbers for DVS connected
compute nodes are:

-   Metadata operations of the order of 36,000 file creates /second to a
    unique directory can be expected, i.e. approximately 23% of that
    achievable on a node that has a Spectrum Scale client.
-   For 8MB transfer size, single stream I/O, is \~3.2GB/s for Write and
    \~3.2 GB/s for Read;
-   For 4KB transfer size, single stream I/O, is \~2.3GB/s for Write and
    \~2.5GB/s for Read (when using IOBUF -- see Caution below). When
    IOBUF is not used Read and Write performance is \<1GB/s.

Unless Cray's [[IOBUF](#_IOBUF_-_Caution)]{.underline} capability is
suitable for an application, [users should avoid the use of
single-stream I/O with small buffers on]{.underline} Māui.

[IOBUF - Caution]{.wysiwyg-color-red} {#iobuf---caution .western}
-------------------------------------

Cray's IOBUF ( man iobuf ) is an I/O buffering library that can reduce
the I/O wait time for programs that read or write large (or small) files
sequentially. IOBUF intercepts standard I/O calls such as read and open
and replaces the stdio ( glibc, libio ) layer of buffering with an
additional layer of buffering, thus improving program performance by
enabling asynchronous prefetching and caching of file data.

**Caution**: IOBUF is not suitable for all I/O styles. IOBUF does not
maintain coherent buffering between processes which open the same file.
For this reason, do not use IOBUF with shared file I/O, such as MPI-IO
routines like MPI\_File\_write\_all . IOBUF is [not
thread-safe]{.underline}, so do not use it with multithreaded programs
in which the threads perform buffered I/O. IOBUF can be linked into
programs that use these I/O styles, but buffering should not be enabled
on those files.

 