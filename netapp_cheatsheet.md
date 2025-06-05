NetApp Commandline Cheatsheet

This is a quick and dirty NetApp commandline cheatsheet on most of the common commands used, this is not extensive so check out the man pages and NetApp documentation. I will be updating this document as I become more familar with the NetApp application.

Server

|   |   |
|---|---|
|Startup and Shutdown||
|Boot Menu|1) Normal Boot.<br><br>2) Boot without /etc/rc.<br><br>3) Change password.<br><br>4) Clean configuration and initialize all disks.<br><br>5) Maintenance mode boot.<br><br>6) Update flash from backup config.<br><br>7) Install new software first.<br><br>8) Reboot node.<br><br>Selection (1-8)?<br><br>- Normal Boot - continue with the normal boot operation<br>- Boot without /etc/rc - boot with only default options and disable some services<br>- Change Password - change the storage systems password<br>- Clean configuration and initialize all disks - cleans all disks and reset the filer to factory default settings<br>- Maintenance mode boot - file system operations are disabled, limited set of commands<br>- Update flash from backup config - restore the configuration information if corrupted on the boot device<br>- Install new software first - use this if the filer does not include support for the storage array<br>- Reboot node - restart the filer|
|startup modes|- boot_ontap - boots the current Data ONTAP software release stored on the boot device<br>- boot primary - boots the Data ONTAP release stored on the boot device as the primary kernel<br>- boot_backup - boots the backup Data ONTAP release from the boot device<br>- boot_diags - boots a Data ONTAP diagnostic kernel<br><br>Note: there are other options but NetApp will provide these as when necessary|
|shutdown|halt [-t <mins>] [-f]<br><br>-t = shutdown after minutes specified<br><br>-f = used with HA clustering, means that the partner filer does not take over|
|restart|reboot [-t <mins>] [-s] [-r] [-f]<br><br>-t = reboot in specified minutes<br><br>-s = clean reboot but also power cycle the filer (like pushing the off button)<br><br>-r = bypasses the shutdown (not clean) and power cycles the filer<br><br>-f = used with HA clustering, means that the partner filer does not take over|
|System Privilege and System shell||
|Privilege|priv set [-q] [admin \| advanced]<br><br>Note: by default you are in administrative mode<br><br>-q = quiet suppresses warning messages|
|Access the systemshell|## First obtain the advanced privileges<br><br>priv set advanced<br><br>## Then unlock and reset the diag users password<br><br>useradmin diaguser unlock<br><br>useradmin diaguser password<br><br>## Now you should be able to access the systemshell and use all the standard Unix<br><br>## commands<br><br>systemshell<br><br>login: diag<br><br>password: ********|
|Licensing and Version||
|licenses (commandline)|## display licenses<br><br>license<br><br>## Adding a license<br><br>license add <code1> <code2><br><br>## Disabling a license<br><br>license delete <service>|
|Data ONTAP version|version [-b]<br><br>-b = include name and version information for the primary, secondary and diagnostic kernels and the firmware|
|Useful Commands||
|read the messages file|rdfile /etc/messages|
|write to a file|wrfile -a <file> <text><br><br># Examples<br><br>wrfile -a /etc/test1 This is line 6 # comment here<br><br>wrfile -a /etc/test1 "This is line \"15\"."|
|System Configuration||
|General information|sysconfig<br><br>sysconfig -v<br><br>sysconfig -a (detailed)|
|Configuration errors|sysconfig -c|
|Display disk devices|sysconfig -d<br><br>sysconfig -A|
|Display Raid group information|sysconfig -V|
|Display arregates and plexes|sysconfig -r|
|Display tape devices|sysconfig -t|
|Display tape libraries|sysconfig -m|
|Environment Information||
|General information|environment status|
|Disk enclosures (shelves)|environment shelf [adapter]<br><br>environment shelf_power_status|
|Chassis|environment chassis all<br><br>environment chassis list-sensors<br><br>environment chassis Fans<br><br>environment chassis CPU_Fans<br><br>environment chassis Power<br><br>environment chassis Temperature<br><br>environment chassis [PS1\|PS2]|
|Fibre Channel Information||
|Fibre Channel stats|fcstat link_status<br><br>fcstat fcal_stat<br><br>fcstat device_map|
|SAS Adapter and Expander Information||
|Shelf information|sasstat shelf<br><br>sasstat shelf_short|
|Expander information|sasstat expander<br><br>sasstat expander_map<br><br>sasstat expander_phy_state|
|Disk information|sasstat dev_stats|
|Adapter information|sasstat adapter_state|
|Statistical Information||
|System|stats show system|
|Processor|stats show processor|
|Disk|stats show disk|
|Volume|stats show volume|
|LUN|stats show lun|
|Aggregate|stats show aggregate|
|FC|stats show fcp|
|iSCSI|stats show iscsi|
|CIFS|stats show cifs|
|Network|stats show ifnet|

Storage

|   |   |
|---|---|
|Storage Commands||
|Display|storage show adapter<br><br>storage show disk [-a\|-x\|-p\|-T]<br><br>storage show expander<br><br>storage show fabric<br><br>storage show fault<br><br>storage show hub<br><br>storage show initiators<br><br>storage show mc<br><br>storage show port<br><br>storage show shelf<br><br>storage show switch<br><br>storage show tape [supported]<br><br>storage show acp<br><br>storage array show<br><br>storage array show-ports<br><br>storage array show-luns<br><br>storage array show-config|
|Enable|storage enable adapter|
|Disable|storage disable adapter|
|Rename switch|storage rename <oldname> <newname>|
|Remove port|storage array remove-port <array_name> -p <WWPN>|
|Load Balance|storage load balance|
|Power Cycle|storage power_cycle shelf -h<br><br>storage power_cycle shelf start -c <channel name><br><br>storage power_cycle shelf completed|

Disks

|   |   |
|---|---|
|Disk Information||
|Disk name|This is the physical disk itself, normally the disk will reside in a disk enclosure, the disk will have a pathname like 2a.17 depending on the type of disk enclosure<br><br>- 2a = SCSI adapter<br>- 17 = disk SCSI ID<br><br>Any disks that are classed as spare will be used in any group to replace failed disks. They can also be assigned to any aggregate. Disks are assigned to a specific pool.|
|Disk Types||
|Data|holds data stored within the RAID group|
|Spare|Does not hold usable data but is available to be added to a RAID group in an aggregate, also known as a hot spare|
|Parity|Store data reconstruction information within the RAID group|
|dParity|Stores double-parity information within the RAID group, if RAID-DP is enabled|
|Disk Commands||
|Display|disk show<br><br>disk show <disk_name><br><br>disk_list<br><br>sysconfig -r<br><br>sysconfig -d<br><br>## list all unnassigned/assigned disks<br><br>disk show -n<br><br>disk show -a|
|Adding (assigning)|## Add a specific disk to pool1 the mirror pool<br><br>disk assign <disk_name> -p 1<br><br>## Assign all disk to pool 0, by default they are assigned to pool 0 if the "-p"<br><br>## option is not specififed<br><br>disk assign all -p 0|
|Remove (spin down disk)|disk remove <disk_name>|
|Reassign|disk reassign -d <new_sysid>|
|Replace|disk replace start <disk_name> <spare_disk_name><br><br>disk replace stop <disk_name><br><br>Note: uses Rapid RAID Recovery to copy data from the specified file system to the specified spare disk, you can stop this process using the stop command|
|Zero spare disks|disk zero spares|
|fail a disk|disk fail <disk_name>|
|Scrub a disk|disk scrub start<br><br>disk scrub stop|
|Sanitize|disk sanitize start <disk list><br><br>disk sanitize abort <disk_list><br><br>disk sanitize status<br><br>disk sanitize release <disk_list><br><br>Note: the release modifies the state of the disk from sanitize to spare. Sanitize requires a license.|
|Maintanence|disk maint start -d <disk_list><br><br>disk maint abort <disk_list><br><br>disk maint list<br><br>disk maint status<br><br>Note: you can test the disk using maintain mode|
|swap a disk|disk swap<br><br>disk unswap<br><br>Note: it stalls all SCSI I/O until you physically replace or add a disk, can used on SCSI disk only.|
|Statisics|disk_stat <disk_name>|
|Simulate a pulled disk|disk simpull <disk_name>|
|Simulate a pushed disk|disk simpush -l<br><br>disk simpush <complete path of disk obtained from above command><br><br>## Example<br><br>ontap1> disk simpush -l<br><br>The following pulled disks are available for pushing:<br><br>                         v0.16:NETAPP__:VD-1000MB-FZ-520:14161400:2104448<br><br>ontap1> disk simpush v0.16:NETAPP__:VD-1000MB-FZ-520:14161400:2104448|

Aggregates

|   |   |
|---|---|
|Aggregate States||
|Online|Read and write access to volumes is allowed|
|Restricted|Some operations, such as parity reconstruction are allowed, but data access is not allowed|
|Offline|No access to the aggregate is allowed|
|Aggregate Status Values||
|32-bit|This aggregate is a 32-bit aggregate|
|64-bit|This aggregate is a 64-bit aggregate|
|aggr|This aggregate is capable of contain FlexVol volumes|
|copying|This aggregate is currently the target aggregate of an active copy operation|
|degraded|This aggregate is contains at least one RAID group with single disk failure that is not being reconstructed|
|double degraded|This aggregate is contains at least one RAID group with double disk failure that is not being reconstructed (RAID-DP aggregate only)|
|foreign|Disks that the aggregate contains were moved to the current storage system from another storage system|
|growing|Disks are in the process of being added to the aggregate|
|initializing|The aggregate is in the process of being initialized|
|invalid|The aggregate contains no volumes and none can be added. Typically this happend only after an aborted "aggr copy" operation|
|ironing|A WAFL consistency check is being performewd on the aggregate|
|mirror degraded|The aggregate is mirrored and one of its plexes is offline or resynchronizing|
|mirrored|The aggregate is mirrored|
|needs check|WAFL consistency check needs to be performed on the aggregate|
|normal|The aggregate is unmirrored and all of its RAID groups are functional|
|out-of-date|The aggregate is mirrored and needs to be resynchronized|
|partial|At least one disk was found for the aggregate, but two or more disks are missing|
|raid0|The aggrgate consists of RAID 0 (no parity) RAID groups|
|raid4|The agrregate consists of RAID 4 RAID groups|
|raid_dp|The agrregate consists of RAID-DP RAID groups|
|reconstruct|At least one RAID group in the aggregate is being reconstructed|
|redirect|Aggregate reallocation or file reallocation with the "-p" option has been started on the aggregate, read performance will be degraded|
|resyncing|One of the mirror aggregates plexes is being resynchronized|
|snapmirror|The aggregate is a SnapMirror replica of another aggregate (traditional volumes only)|
|trad|The aggregate is a traditional volume and cannot contain FlexVol volumes.|
|verifying|A mirror operation is currently running on the aggregate|
|wafl inconsistent|The aggregate has been marked corrupted; contact techincal support|
|Aggregate Commands||
|Displaying|aggr status<br><br>aggr status -r<br><br>aggr status <aggregate> [-v]|
|Check you have spare disks|aggr status -s|
|Adding (creating)|## Syntax - if no option is specified then the defult is used<br><br>aggr create <aggr_name> [-f] [-m] [-n] [-t {raid0 \|raid4 \|raid_dp}] [-r raid_size] [-T disk_type] [-R rpm>] [-L] [-B {32\|64}] <disk_list><br><br>## create aggregate called newaggr that can have a maximum of 8 RAID groups<br><br>aggr create newaggr -r 8 -d 8a.16 8a.17 8a.18 8a.19<br><br>## create aggregated called newfastaggr using 20 x 15000rpm disks<br><br>aggr create newfastaggr -R 15000 20<br><br>## create aggrgate called newFCALaggr (note SAS and FC disks may bge used)<br><br>aggr create newFCALaggr -T FCAL 15<br><br>Note:<br><br>-f = overrides the default behavior that does not permit disks in a plex to belong to different disk pools<br><br>-m = specifies the optional creation of a SyncMirror<br><br>-n = displays the results of the command but does not execute it<br><br>-r = maximum size (number of disks) of the RAID groups for this aggregate<br><br>-T = disk type ATA, SATA, SAS, BSAS, FCAL or LUN<br><br>-R = rpm which include 5400, 7200, 10000 and 15000|
|Remove(destroying)|aggr offline <aggregate><br><br>aggr destroy <aggregate>|
|Unremoving(undestroying)|aggr undestroy <aggregate>|
|Rename|aggr rename <old name> <new name>|
|Increase size|## Syntax<br><br>aggr add <aggr_name> [-f] [-n] [-g {raid_group_name \| new \|all}] <disk_list><br><br>## add an additonal disk to aggregate pfvAggr, use "aggr status" to get group name<br><br>aggr status pfvAggr -r<br><br>aggr add pfvAggr -g rg0 -d v5.25<br><br>## Add 4 300GB disk to aggregate aggr1<br><br>aggr add aggr1 4@300|
|offline|aggr offline <aggregate>|
|online|aggr online <aggregate>|
|restricted state|aggr restrict <aggregate>|
|Change an aggregate options|## to display the aggregates options<br><br>aggr options <aggregate><br><br>## change a aggregates raid group<br><br>aggr options <aggregate> raidtype raid_dp<br><br>## change a aggregates raid size<br><br>aggr options <aggregate> raidsize 4|
|show space usage|aggr show_space <aggregate>|
|Mirror|aggr mirror <aggregate>|
|Split mirror|aggr split <aggregate/plex> <new_aggregate>|
|Copy from one agrregate to another|## Obtain the status<br><br>aggr copy status<br><br>## Start a copy<br><br>aggr copy start <aggregate source> <aggregate destination><br><br>## Abort a copy - obtain the operation number by using "aggr copy status"<br><br>aggr copy abort <operation number><br><br>## Throttle the copy 10=full speed, 1=one-tenth full speed<br><br>aggr copy throttle <operation number> <throttle speed>|
|Scrubbing (parity)|## Media scrub status<br><br>aggr media_scrub status<br><br>aggr scrub status<br><br>## start a scrub operation<br><br>aggr scrub start [ aggrname \| plexname \| groupname ]<br><br>## stop a scrub operation<br><br>aggr scrub stop [ aggrname \| plexname \| groupname ]<br><br>## suspend a scrub operation<br><br>aggr scrub suspend [ aggrname \| plexname \| groupname ]<br><br>## resume a scrub operation<br><br>aggr scrub resume [ aggrname \| plexname \| groupname ]<br><br>Note: Starts parity scrubbing on the named online aggregate. Parity scrubbing compares the data disks to the<br><br>parity disk(s) in their RAID group, correcting the parity disk’s contents as necessary. If no name is<br><br>given, parity scrubbing is started on all online aggregates. If an aggregate name is given, scrubbing is<br><br>started on all RAID groups contained in the aggregate. If a plex name is given, scrubbing is started on<br><br>all RAID groups contained in the plex.<br><br>Look at the following system options:<br><br>raid.scrub.duration 360<br><br>raid.scrub.enable on<br><br>raid.scrub.perf_impact low<br><br>raid.scrub.schedule|
|Verify (mirroring)|## verify status<br><br>aggr verify status<br><br>## start a verify operation<br><br>aggr verify start [ aggrname ]<br><br>## stop a verify operation<br><br>aggr verify stop [ aggrname ]<br><br>## suspend a verify operation<br><br>aggr verify suspend [ aggrname ]<br><br>## resume a verify operation<br><br>aggr verify resume [ aggrname ]<br><br>Note: Starts RAID mirror verification on the named online mirrored aggregate. If no name is given, then<br><br>RAID mirror verification is started on all online mirrored aggregates. Verification compares the data in<br><br>both plexes of a mirrored aggregate. In the default case, all blocks that differ are logged, but no changes<br><br>are made.|
|Media Scrub|aggr media_scrub status<br><br>Note: Prints the media scrubbing status of the named aggregate, plex, or group. If no name is given, then<br><br>status is printed for all RAID groups currently running a media scrub. The status includes a<br><br>percent-complete and whether it is suspended.<br><br>Look at the following system options:<br><br>raid.media_scrub.enable on<br><br>raid.media_scrub.rate 600<br><br>raid.media_scrub.spares.enable on|

Volumes

|   |   |
|---|---|
|Volume States||
|Online|Read and write access to this volume is allowed.|
|Restricted|Some operations, such as parity reconstruction, are allowed, but data access is not allowed.|
|Offline|No access to the volume is allowed.|
|Volume Status Values||
|access denied|The origin system is not allowing access. (FlexCache volumes<br><br>only.)|
|active redirect|The volume's containing aggregate is undergoing reallocation (with the -p option specified). Read performance may be reduced while the volume is in this state.|
|connecting|The caching system is trying to connect to the origin system. (FlexCache volumes only.)|
|copying|The volume is currently the target of an active vol copy or snapmirror operation.|
|degraded|The volume's containing aggregate contains at least one degraded RAID group that is not being reconstructed after single disk failure.|
|double degraded|The volume's containing aggregate contains at least one degraded RAID-DP group that is not being reconstructed after double disk failure.|
|flex|The volume is a FlexVol volume.|
|flexcache|The volume is a FlexCache volume.|
|foreign|Disks used by the volume's containing aggregate were moved to the current storage system from another storage system.|
|growing|Disks are being added to the volume's containing aggregate.|
|initializing|The volume's containing aggregate is being initialized.|
|invalid|The volume does not contain a valid file system.|
|ironing|A WAFL consistency check is being performed on the volume's containing aggregate.|
|lang mismatch|The language setting of the origin volume was changed since the caching volume was created. (FlexCache volumes only.)|
|mirror degraded|The volume's containing aggregate is mirrored and one of its plexes is offline or resynchronizing.|
|mirrored|The volume's containing aggregate is mirrored.|
|needs check|A WAFL consistency check needs to be performed on the volume's containing aggregate.|
|out-of-date|The volume's containing aggregate is mirrored and needs to be resynchronized.|
|partial|At least one disk was found for the volume's containing aggregate, but two or more disks are missing.|
|raid0|The volume's containing aggregate consists of RAID0 (no parity) groups (array LUNs only).|
|raid4|The volume's containing aggregate consists of RAID4 groups.|
|raid_dp|The volume's containing aggregate consists of RAID-DP groups.|
|reconstruct|At least one RAID group in the volume's containing aggregate is being reconstructed.|
|redirect|The volume's containing aggregate is undergoing aggregate reallocation or file reallocation with the -p option. Read performance to volumes in the aggregate might be degraded.|
|rem vol changed|The origin volume was deleted and re-created with the same name. Re-create the FlexCache volume to reenable the FlexCache relationship. (FlexCache volumes only.)|
|rem vol unavail|The origin volume is offline or has been deleted. (FlexCache volumes only.)|
|remote nvram err|The origin system is experiencing problems with its NVRAM. (FlexCache volumes only.)|
|resyncing|One of the plexes of the volume's containing mirrored aggregate is being resynchronized.|
|snapmirrored|The volume is in a SnapMirror relationship with another volume.|
|trad|The volume is a traditional volume.|
|unrecoverable|The volume is a FlexVol volume that has been marked unrecoverable; contact technical support.|
|unsup remote vol|The origin system is running a version of Data ONTAP the does not support FlexCache volumes or is not compatible with the version running on the caching system. (FlexCache volumes only.)|
|verifying|RAID mirror verification is running on the volume's containing aggregate.|
|wafl inconsistent|The volume or its containing aggregate has been marked corrupted; contact technical support .|
|General Volume Operations (Traditional and FlexVol)||
|Displaying|vol status<br><br>vol status -v (verbose)<br><br>vol status -l (display language)|
|Remove (destroying)|vol offline <vol_name><br><br>vol destroy <vol_name>|
|Rename|vol rename <old_name> <new_name>|
|online|vol online <vol_name>|
|offline|vol offline <vol_name>|
|restrict|vol restrict <vol_name>|
|decompress|vol decompress status<br><br>vol decompress start <vol_name><br><br>vol decompress stop <vol_name>|
|Mirroring|vol mirror volname [-n][-v victim_volname][-f][-d <disk_list>]<br><br>Note:<br><br>Mirrors the currently-unmirrored traditional volume volname, either with the specified set of disks or with the contents of another unmirrored traditional volume victim_volname, which will be destroyed in the process.<br><br>The vol mirror command fails if either the chosen volname or victim_volname are flexible volumes. Flexible volumes require that any operations having directly to do with their containing aggregates be handled via the new aggr command suite.|
|Change language|vol lang <vol_name> <language>|
|Change maximum number of files|## Display maximum number of files<br><br>maxfiles <vol_name><br><br>## Change maximum number of files<br><br>maxfiles <vol_name> <max_num_files>|
|Change root volume|vol options <vol_name> root|
|Media Scrub|vol media_scrub status [volname\|plexname\|groupname -s disk-name][-v]<br><br>Note: Prints the media scrubbing status of the named aggregate, volume, plex, or group. If no name is given, then<br><br>status is printed for all RAID groups currently running a media scrub. The status includes a<br><br>percent-complete and whether it is suspended.<br><br>Look at the following system options:<br><br>raid.media_scrub.enable on<br><br>raid.media_scrub.rate 600<br><br>raid.media_scrub.spares.enable on|
|FlexVol Volume Operations (only)||
|Adding (creating)|## Syntax<br><br>vol create vol_name [-l language_code] [-s {volume\|file\|none}] <aggr_name> size{k\|m\|g\|t}<br><br>## Create a 200MB volume using the english character set<br><br>vol create newvol -l en aggr1 200M<br><br>## Create 50GB flexvol volume<br><br>vol create vol1 aggr0 50g|
|additional disks|## add an additional disk to aggregate flexvol1, use "aggr status" to get group name<br><br>aggr status flexvol1 -r<br><br>aggr add flexvol1 -g rg0 -d v5.25|
|Resizing|vol size <vol_name> [+\|-] n{k\|m\|g\|t}<br><br>## Increase flexvol1 volume by 100MB<br><br>vol size flexvol1 + 100m|
|Automatically resizing|vol autosize vol_name [-m size {k\|m\|g\|t}] [-I size {k\|m\|g\|t}] on<br><br>## automatically grow by 10MB increaments to max of 500MB<br><br>vol autosize flexvol1 -m 500m -I 10m on|
|Determine free space and Inodes|df -Ah<br><br>df -I|
|Determine size|vol size <vol_name>|
|automatic free space preservation|vol options <vol_name> try_first [volume_grow\|snap_delete]<br><br>Note:<br><br>If you specify volume_grow, Data ONTAP attempts to increase the volume's size before deleting any Snapshot copies. Data ONTAP increases the volume size based on specifications you provided using the vol autosize command.<br><br>If you specify snap_delete, Data ONTAP attempts to create more free space by deleting Snapshot copies, before increasing the size of the volume. Data ONTAP deletes Snapshot copies based on the specifications you provided using the snap autodelete command.|
|display a FlexVol volume's containing aggregate|vol container <vol_name>|
|Cloning|vol clone create clone_vol [-s none\|file\|volume] -b parent_vol [parent_snap]<br><br>vol clone split start<br><br>vol clone split stop<br><br>vol clone split estimate<br><br>vol clone split status<br><br>Note: The vol clone create command creates a flexible volume named clone_vol on the local filer that is a clone of a "backing" flexible volume named par_ent_vol. A clone is a volume that is a writable snapshot of another volume. Initially, the clone and its parent share the same storage; more storage space is consumed only as one volume or the other changes.|
|Copying|vol copy start [-S\|-s snapshot] <vol_source> <vol_destination><br><br>vol copy status<br><br>vol copy abort <operation number><br><br>vol copy throttle <operation_number> <throttle value 10-1><br><br>## Example - Copies the nightly snapshot named nightly.1 on volume vol0 on the local filer to the volume vol0 on remote ## filer named toaster1.<br><br>vol copy start -s nightly.1 vol0 toaster1:vol0<br><br>Note: Copies all data, including snapshots, from one volume to another. If the -S flag is used, the command copies all snapshots in the source volume to the destination volume. To specify a particular snapshot to copy, use the -s flag followed by the name of the snapshot. If neither the -S nor -s flag is used in the command, the filer automatically creates a distinctively-named snapshot at the time the vol copy start command is executed and copies only that snapshot to the destination volume.<br><br>The source and destination volumes must either both be traditional volumes or both be flexible volumes. The vol copy command will abort if an attempt is made to copy between different volume types.<br><br>The source and destination volumes can be on the same filer or on different filers. If the source or destination volume is on a filer other than the one on which the vol copy start command was entered, specify the volume name in the filer_name:volume_name format.|
|Traditional Volume Operations (only)||
|adding (creating)|vol\|aggr create vol_name -v [-l language_code] [-f] [-m] [-n] [-v] [-t {raid4\|raid_dp}] [-r raidsize] [-T disk-type] -R rpm] [-L] disk-list<br><br>## create traditional volume using aggr command<br><br>aggr create tradvol1 -l en -t raid4 -d v5.26 v5.27<br><br>## create traditional volume using vol command<br><br>vol create tradvol1 -l en -t raid4 -d v5.26 v5.27<br><br>## Create traditional volume using 20 disks, each RAID group can have 10 disks<br><br>vol create vol1 -r 10 20|
|additional disks|vol add volname[-f][-n][-g <raidgroup>]{ ndisks[@size]\|-d <disk_list> }<br><br>## add another disk to the already existing traditional volume<br><br>vol add tradvol1 -d v5.28|
|splitting|aggr split <volname/plexname> <new_volname>|
|Scrubing (parity)|## The more new "aggr scrub " command is preferred<br><br>vol scrub status [volname\|plexname\|groupname][-v]<br><br>vol scrub start [volname\|plexname\|groupname][-v]<br><br>vol scrub stop [volname\|plexname\|groupname][-v]<br><br>vol scrub suspend [volname\|plexname\|groupname][-v]<br><br>vol scrub resume [volname\|plexname\|groupname][-v]<br><br>Note: Print the status of parity scrubbing on the named traditional volume, plex or RAID group. If no name is provided, the status is given on all RAID groups currently undergoing parity scrubbing. The status includes a percent-complete as well as the scrub’s suspended status (if any).|
|Verify (mirroring)|## The more new "aggr verify" command is preferred<br><br>## verify status<br><br>vol verify status<br><br>## start a verify operation<br><br>vol verify start [ aggrname ]<br><br>## stop a verify operation<br><br>vol verify stop [ aggrname ]<br><br>## suspend a verify operation<br><br>vol verify suspend [ aggrname ]<br><br>## resume a verify operation<br><br>vol verify resume [ aggrname ]<br><br>Note: Starts RAID mirror verification on the named online mirrored aggregate. If no name is given, then<br><br>RAID mirror verification is started on all online mirrored aggregates. Verification compares the data in<br><br>both plexes of a mirrored aggregate. In the default case, all blocks that differ are logged, but no changes<br><br>are made.|

FlexCache Volumes

|   |   |
|---|---|
|FlexCache Consistency||
|Delegations|You can think of a delegation as a contract between the origin system and the caching volume; as long as the caching volume has the delegation, the file has not changed. Delegations are used only in certain situations.<br><br>When data from a file is retrieved from the origin volume, the origin system can give a delegation for that file to the caching volume. Before that file is modified on the origin volume, whether due to a request from another caching volume or due to direct client access, the origin system revokes the delegation for that file from all caching volumes that have that delegation.|
|Attribute cache timeouts|When data is retrieved from the origin volume, the file that contains that data is considered valid in the FlexCache volume as long as a delegation exists for that file. If no delegation exists, the file is considered valid for a certain length of time, specified by the attribute cache timeout.<br><br>If a client requests data from a file for which there are no delegations, and the attribute cache timeout has been exceeded, the FlexCache volume compares the file attributes of the cached file with the attributes of the file on the origin system.|
|write operation proxy|If a client modifies a file that is cached, that operation is passed back, or proxied through, to the origin system, and the file is ejected from the cache.<br><br>When the write is proxied, the attributes of the file on the origin volume are changed. This means that when another client requests data from that file, any other FlexCache volume that has that data cached will re-request the data after the attribute cache timeout is reached.|
|FlexCache Status Values||
|access denied|The origin system is not allowing FlexCache access. Check the setting of the flexcache.access option on the origin system.|
|connecting|The caching system is trying to connect to the origin system.|
|lang mismatch|The language setting of the origin volume was changed since the FlexCache volume was created.|
|rem vol changed|The origin volume was deleted and re-created with the same name. Re-create the FlexCache volume to reenable the FlexCache relationship.|
|rem vol unavail|The origin volume is offline or has been deleted.|
|remote nvram err|The origin system is experiencing problems with its NVRAM.|
|unsup remote vol|The origin system is running a version of Data ONTAP that either does not support FlexCache volumes or is not compatible with the version running on the caching system.|
|FlexCache Commands||
|Display|vol status<br><br>vol status -v <flexcache_name><br><br>## How to display the options available and what they are set to<br><br>vol help options<br><br>vol options <flexcache_name>|
|Display free space|df -L|
|Adding (Create)|## Syntax<br><br>vol create <flexcache_name> <aggr> [size{k\|m\|g\|t}] -S origin:source_vol<br><br>## Create a FlexCache volume called flexcache1 with autogrow in aggr1 aggregate with the source volume vol1<br><br>## on storage netapp1 server<br><br>vol create flexcache1 aggr1 -S netapp1:vol1|
|Removing (destroy)|vol offline < flexcache_name><br><br>vol destroy <flexcache_name>|
|Automatically resizing|vol options <flexcache_name> flexcache_autogrow [on\|off]|
|Eject file from cache|flexcache eject <path> [-f]|
|Statistics|## Client stats<br><br>flexcache stats -C <flexcache_name><br><br>## Server stats<br><br>flexcache stats -S <volume_name> -c <client><br><br>## File stats<br><br>flexcache fstat <path>|

FlexClone Volumes

|   |   |
|---|---|
|FlexClone Commands||
|Display|vol status<br><br>vol status <flexclone_name> -v<br><br>df -Lh|
|adding (create)|## Syntax<br><br>vol clone create clone_name [-s {volume\|file\|none}] -b parent_name [parent_snap]<br><br>## create a flexclone called flexclone1 from the parent flexvol1<br><br>vol clone create flexclone1 -b flexvol1|
|Removing (destroy)|vol offline <flexclone_name><br><br>vol destroy <flexclone_name>|
|splitting|## Determine the free space required to perform the split<br><br>vol clone split estimate <flexclone_name><br><br>## Double check you have the space<br><br>df -Ah<br><br>## Perform the split<br><br>vol clone split start <flexclone_name><br><br>## Check up on its status<br><br>vol colne split status <flexclone_name><br><br>## Stop the split<br><br>vol clone split stop <flexclone_name>|
|log file|/etc/log/clone<br><br>The clone log file records the following information:<br><br>• Cloning operation ID<br><br>• The name of the volume in which the cloning operation was performed<br><br>• Start time of the cloning operation<br><br>• End time of the cloning operation<br><br>• Parent file/LUN and clone file/LUN names<br><br>• Parent file/LUN ID<br><br>• Status of the clone operation: successful, unsuccessful, or stopped and some other details|

Deduplication

|   |   |
|---|---|
|Deduplication Commands||
|start/restart deduplication operation|sis start -s <path><br><br>sis start -s /vol/flexvol1<br><br>## Use previous checkpoint<br><br>sis start -sp <path>|
|stop deduplication operation|sis stop <path>|
|schedule deduplication|sis config -s <schedule> <path><br><br>sis config -s mon-fri@23 /vol/flexvol1<br><br>Note: schedule lists the days and hours of the day when deduplication runs. The schedule can be of the following forms:<br><br>- day_list[@hour_list]  <br>    If hour_list is not specified, deduplication runs at midnight on each scheduled day.<br>- hour_list[@day_list]  <br>    If day_list is not specified, deduplication runs every day at the specified hours.<br>- • -  <br>    A hyphen (-) disables deduplication operations for the specified FlexVol volume.|
|enabling|sis on <path>|
|disabling|sis off <path>|
|status|sis status -l <path>|
|Display saved space|df -s <path>|

QTrees

|   |   |
|---|---|
|QTree Commands||
|Display|qtree status [-i] [-v]<br><br>Note:<br><br>The -i option includes the qtree ID number in the display.<br><br>The -v option includes the owning vFiler unit, if the MultiStore license is enabled.|
|adding (create)|## Syntax - by default wafl.default_qtree_mode option is used<br><br>qtree create path [-m mode]<br><br>## create a news qtree in the /vol/users volume using 770 as permissions<br><br>qtree create /vol/users/news -m 770|
|Remove|rm -Rf <directory>|
|Rename|mv <old_name> <new_name>|
|convert a directory into a qtree directory|## Move the directory to a different directory<br><br>mv /n/joel/vol1/dir1 /n/joel/vol1/olddir<br><br>## Create the qtree<br><br>qtree create /n/joel/vol1/dir1<br><br>## Move the contents of the old directory back into the new QTree<br><br>mv /n/joel/vol1/olddir/* /n/joel/vol1/dir1<br><br>## Remove the old directory name<br><br>rmdir /n/joel/vol1/olddir|
|stats|qtree stats [-z] [vol_name]<br><br>Note:<br><br>-z = zero stats|
|Change the security style|## Syntax<br><br>qtree security path {unix \| ntfs \| mixed}<br><br>## Change the security style of /vol/users/docs to mixed<br><br>qtree security /vol/users/docs mixed|

Quotas

|   |   |
|---|---|
|Quota Commands||
|Quotas configuration file|/mroot/etc/quotas|
|Example quota file|## hard limit \| thres \|soft limit<br><br>##Quota Target type disk files\| hold \|disk file<br><br>##------------- ----- ---- ----- ----- ----- ----<br><br>* tree@/vol/vol0 - - - - - # monitor usage on all qtrees in vol0<br><br>/vol/vol2/qtree tree 1024K 75k - - - # enforce qtree quota using kb<br><br>tinh user@/vol/vol2/qtree1 100M - - - - # enforce users quota in specified qtree<br><br>dba group@/vol/ora/qtree1 100M - - - - # enforce group quota in specified qtree<br><br># * = default user/group/qtree<br><br># - = placeholder, no limit enforced, just enable stats collection<br><br>Note: you have lots of permutations, so checkout the documentation|
|Displaying|quota report [<path>]|
|Activating|quota on [-w] <vol_name><br><br>Note:<br><br>-w = return only after the entire quotas file has been scanned|
|Deactivitating|quota off [-w] <vol_name>|
|Reinitializing|quota off [-w] <vol_name><br><br>quota on [-w] <vol_name>|
|Resizing|quota resize <vol_name><br><br>Note: this commands rereads the quota file|
|Deleting|edit the quota file<br><br>quota resize <vol_name>|
|log messaging|quota logmsg|

LUNs, igroups and LUN mapping

|   |   |
|---|---|
|LUN configuration||
|Display|lun show<br><br>lun show -m<br><br>lun show -v|
|Initialize/Configure LUNs, mapping|lun setup<br><br>Note: follow the prompts to create and configure LUN's|
|Create|lun create -s 100m -t windows /vol/tradvol1/lun1|
|Destroy|lun destroy [-f] /vol/tradvol1/lun1<br><br>Note: the "-f" will force the destroy|
|Resize|lun resize <lun path> <size><br><br>lun resize /vol/tradvol1/lun1 75m|
|Restart block protocol access|lun online /vol/tradvol1/lun1|
|Stop block protocol access|lun offline /vol/tradvol1/lun1|
|Map a LUN to an initiator group|lun map /vol/tradvol1/lun1 win_hosts_group1 0<br><br>lun map -f /vol/tradvol1/lun2 linux_host_group1 1<br><br>lun show -m<br><br>Note: use "-f" to force the mapping|
|Remove LUN mapping|lun show -m<br><br>lun offline /vol/tradvol1<br><br>lun unmap /vol/tradvol1/lun1 win_hosts_group1 0|
|Displays or zeros read/write statistics for LUN|lun stats /vol/tradvol1/lun1|
|Comments|lun comment /vol/tradvol1/lun1 "10GB for payroll records"|
|Check all lun/igroup/fcp settings for correctness|lun config_check -v|
|Manage LUN cloning|# Create a Snapshot copy of the volume containing the LUN to be cloned by entering the following command<br><br>snap create tradvol1 tradvol1_snapshot_08122010<br><br># Create the LUN clone by entering the following command<br><br>lun clone create /vol/tradvol1/clone_lun1 -b /vol/tradvol1/tradvol1_snapshot_08122010 lun1|
|Show the maximum possible size of a LUN on a given volume or qtree|lun maxsize /vol/tradvol1|
|Move (rename) LUN|lun move /vol/tradvol1/lun1 /vol/tradvol1/windows_lun1|
|Display/change LUN serial number|lun serial -x /vol/tradvol1/lun1|
|Manage LUN properties|lun set reservation /vol/tradvol1/hpux/lun0|
|Configure NAS file-sharing properties|lun share <lun_path> { none \| read \| write \| all }|
|Manage LUN and snapshot interactions|lun snap usage -s <volume> <snapshot>|
|igroup configuration||
|display|igroup show<br><br>igroup show -v<br><br>igroup show iqn.1991-05.com.microsoft:xblade|
|create (iSCSI)|igroup create -i -t windows win_hosts_group1 iqn.1991-05.com.microsoft:xblade|
|create (FC)|igroup create -i -f windows win_hosts_group1 iqn.1991-05.com.microsoft:xblade|
|destroy|igroup destroy win_hosts_group1|
|add initiators to an igroup|igroup add win_hosts_group1 iqn.1991-05.com.microsoft:laptop|
|remove initiators to an igroup|igroup remove win_hosts_group1 iqn.1991-05.com.microsoft:laptop|
|rename|igroup rename win_hosts_group1 win_hosts_group2|
|set O/S type|igroup set win_hosts_group1 ostype windows|
|Enabling ALUA|igroup set win_hosts_group1 alua yes<br><br>Note: ALUA defines a standard set of SCSI commands for discovering and managing multiple paths to LUNs on Fibre Channel and iSCSI SANs. ALUA enables the initiator to query the target about path attributes, such as primary path and secondary path. It also enables the target to communicate events back to the initiator. As long as the host supports the ALUA standard, multipathing software can be developed to support any array. Proprietary SCSI commands are no longer required.|
|iSCSI commands||
|display|iscsi initiator show<br><br>iscsi session show [-t]<br><br>iscsi connection show -v<br><br>iscsi security show|
|status|iscsi status|
|start|iscsi start|
|stop|iscsi stop|
|stats|iscsi stats|
|nodename|iscsi nodename<br><br># to change the name<br><br>iscsi nodename <new name>|
|interfaces|iscsi interface show<br><br>iscsi interface enable e0b<br><br>iscsi interface disable e0b|
|portals|iscsi portal show<br><br>Note: Use the iscsi portal show command to display the target IP addresses of the storage system. The storage system's target IP addresses are the addresses of the interfaces used for the iSCSI protocol|
|accesslists|iscsi interface accesslist show<br><br>Note: you can add or remove interfaces from the list|
|Port Sets||
|display|portset show<br><br>portset show portset1<br><br>igroup show linux-igroup1|
|create|portset create -f portset1 SystemA:4b|
|destroy|igroup unbind linux-igroup1 portset1<br><br>portset destroy portset1|
|add|portset add portset1 SystemB:4b|
|remove|portset remove portset1 SystemB:4b|
|binding|igroup bind linux-igroup1 portset1<br><br>igroup unbind linux-igroup1 portset1|
|FCP service||
|display|fcp show adapter -v|
|daemon status|fcp status|
|start|fcp start|
|stop|fcp stop|
|stats|fcp stats -i interval [-c count] [-a \| adapter]<br><br>fcp stats -i 1|
|target expansion adapters|fcp config <adapter> [down\|up]<br><br>fcp config 4a down|
|target adapter speed|fcp config <adapter> speed [auto\|1\|2\|4\|8]<br><br>fcp config 4a speed 8|
|set WWPN #|fcp portname set [-f] adapter wwpn<br><br>fcp portname set -f 1b 50:0a:09:85:87:09:68:ad|
|swap WWPN #|fcp portname swap [-f] adapter1 adapter2<br><br>fcp portname swap -f 1a 1b|
|change WWNN|# display nodename<br><br>fcp nodename<br><br>fcp nodename [-f]nodename<br><br>fcp nodename 50:0a:09:80:82:02:8d:ff<br><br>Note: The WWNN of a storage system is generated by a serial number in its NVRAM, but it is stored ondisk. If you ever replace a storage system chassis and reuse it in the same Fibre Channel SAN, it is possible, although extremely rare, that the WWNN of the replaced storage system is duplicated. In this unlikely event, you can change the WWNN of the storage system.|
|WWPN Aliases - display|fcp wwpn-alias show<br><br>fcp wwpn-alias show -a my_alias_1<br><br>fcp wwpn-alias show -w 10:00:00:00:c9:30:80:2|
|WWPN Aliases - create|fcp wwpn-alias set [-f] alias wwpn<br><br>fcp wwpn-alias set my_alias_1 10:00:00:00:c9:30:80:2f|
|WWPN Aliases - remove|fcp wwpn-alias remove [-a alias ... \| -w wwpn]<br><br>fcp wwpn-alias remove -a my_alias_1<br><br>fcp wwpn-alias remove -w 10:00:00:00:c9:30:80:2|

Snapshotting and Cloning

|   |   |
|---|---|
|Snapshot and Cloning commands||
|Display clones|snap list|
|create clone|# Create a LUN by entering the following command<br><br>lun create -s 10g -t solaris /vol/tradvol1/lun1<br><br># Create a Snapshot copy of the volume containing the LUN to be cloned by entering the following command<br><br>snap create tradvol1 tradvol1_snapshot_08122010<br><br># Create the LUN clone by entering the following command<br><br>lun clone create /vol/tradvol1/clone_lun1 -b /vol/tradvol1/lun1 tradvol1_snapshot_08122010|
|destroy clone|# display the snapshot copies<br><br>lun snap usage tradvol1 tradvol1_snapshot_08122010<br><br># Delete all the LUNs in the active file system that are displayed by the lun snap usage command by entering the following command<br><br>lun destroy /vol/tradvol1/clone_lun1<br><br># Delete all the Snapshot copies that are displayed by the lun snap usage command in the order they appear<br><br>snap delete tradvol1 tradvol1_snapshot_08122010|
|clone dependency|vol options <vol_name> <snapshot_clone_dependency> on<br><br>vol options <vol_name> <snapshot_clone_dependency> off<br><br>Note: Prior to Data ONTAP 7.3, the system automatically locked all backing Snapshot copies when Snapshot copies of LUN clones were taken. Starting with Data ONTAP 7.3, you can enable the system to only lock backing Snapshot copies for the active LUN clone. If you do this, when you delete the active LUN clone, you can delete the base Snapshot copy without having to first delete all of the more recent backing Snapshot copies.<br><br>This behavior in not enabled by default; use the snapshot_clone_dependency volume option to enable it. If this option is set to off, you will still be required to delete all subsequent Snapshot copies before deleting the base Snapshot copy. If you enable this option, you are not required to rediscover the LUNs. If you perform a subsequent volume snap restore operation, the system restores whichever value was present at the time the Snapshot copy was taken.|
|Restoring snapshot|snap restore -s payroll_lun_backup.2 -t vol /vol/payroll_lun|
|splitting the clone|lun clone split start lun_path<br><br>lun clone split status lun_path|
|stop clone splitting|lun clone split stop lun_path|
|delete snapshot copy|snap delete vol-name snapshot-name<br><br>snap delete -a -f <vol-name>|
|disk space usage|lun snap usage tradvol1 mysnap|
|Use Volume copy to copy LUN's|vol copy start -S source:source_volume dest:dest_volume<br><br>vol copy start -S /vol/vol0 filerB:/vol/vol1|
|The estimated rate of change of data between Snapshot copies in a<br><br>volume|snap delta /vol/tradvol1 tradvol1_snapshot_08122010|
|The estimated amount of space freed if you delete the specified<br><br>Snapshot copies|snap reclaimable /vol/tradvol1 tradvol1_snapshot_08122010|

File Access using NFS

|   |   |
|---|---|
|Export Options||
|actual=<path>|Specifies the actual file system path corresponding to the exported file system path.|
|anon=<uid>\|<name>|Specifies the effective user ID (or name) of all anonymous or root NFS client users that access the file system path.|
|nosuid|Disables setuid and setgid executables and mknod commands on the file system path.|
|ro \| ro=clientid|Specifies which NFS clients have read-only access to the file system path.|
|rw \| rw=clientid|Specifies which NFS clients have read-write access to the file system path.|
|root=clientid|Specifies which NFS clients have root access to the file system path. If you specify the root= option, you must specify at least one NFS client identifier. To exclude NFS clients from the list, prepend the NFS client identifiers with a minus sign (-).|
|sec=sectype|Specifies the security types that an NFS client must support to access the file system path. To apply the security types to all types of access, specify the sec= option once. To apply the security types to specific types of access (anonymous, non-super user, read-only, read-write, or root), specify the sec= option at least twice, once before each access type to which it applies (anon, nosuid, ro, rw, or root, respectively).<br><br>security types could be one of the following:<br><br>\|   \|   \|<br>\|---\|---\|<br>\|none\|No security. Data ONTAP treats all of the NFS client's users as anonymous users.\|<br>\|sys\|Standard UNIX (AUTH_SYS) authentication. Data ONTAP checks the NFS credentials of all of the<br><br>NFS client's users, applying the file access permissions specified for those users in the NFS server's /etc/passwd file. This is the default security type.\|<br>\|krb5\|Kerberos(tm) Version 5 authentication. Data ONTAP uses data encryption standard (DES) key<br><br>encryption to authenticate the NFS client's users.\|<br>\|krb5i\|Kerberos(tm) Version 5 integrity. In addition to authenticating the NFS client's users, Data<br><br>ONTAP uses message authentication codes (MACs) to verify the integrity of the NFS client's remote procedure requests and responses, thus preventing "man-in-the-middle" tampering.\|<br>\|krb5p\|Kerberos(tm) Version 5 privacy. In addition to authenticating the NFS client's users and verifying data integrity, Data ONTAP encrypts NFS arguments and results to provide privacy.\||
|Examples|rw=10.45.67.0/24<br><br>ro,root=@trusted,rw=@friendly<br><br>rw,root=192.168.0.80,nosuid|
|Export Commands||
|Displaying|exportfs<br><br>exportfs -q <path>|
|create|# create export in memory and write to /etc/exports (use default options)<br><br>exportfs -p /vol/nfs1<br><br># create export in memory and write to /etc/exports (use specific options)<br><br>exportsfs -io sec=none,rw,root=192.168.0.80,nosuid /vol/nfs1<br><br># create export in memory only using own specific options<br><br>exportsfs -io sec=none,rw,root=192.168.0.80,nosuid /vol/nfs1|
|remove|# Memory only<br><br>exportfs -u <path><br><br># Memory and /etc/exportfs<br><br>exportfs -z <path>|
|export all|exportfs -a|
|check access|exportfs -c 192.168.0.80 /vol/nfs1|
|flush|exportfs -f<br><br>exportfs -f <path>|
|reload|exportfs -r|
|storage path|exportfs -s <path>|
|Write export to a file|exportfs -w <path/export_file>|
|fencing|# Suppose /vol/vol0 is exported with the following export options:<br><br>   -rw=pig:horse:cat:dog,ro=duck,anon=0<br><br># The following command enables fencing of cat from /vol/vol0<br><br>exportfs -b enable save cat /vol/vol0<br><br># cat moves to the front of the ro= list for /vol/vol0:<br><br>   -rw=pig:horse:dog,ro=cat:duck,anon=0|
|stats|nfsstat|

File Access using CIFS

|   |   |
|---|---|
|Useful CIFS options||
|change the security style|options wafl.default_security_style {ntfs \| unix \| mixed}|
|timeout|options cifs.idle_timeout time|
|Performance|options cifs.oplocks.enable on<br><br>Note: Under some circumstances, if a process has an exclusive oplock on a file and a second process attempts to open the file, the first process must invalidate cached data and flush writes and locks. The client must then relinquish the oplock and access to the file. If there is a network failure during this flush, cached write data might be lost.|
|CIFS Commands||
|useful files|/etc/cifsconfig_setup.cfg<br><br>/etc/usermap.cfs<br><br>/etc/passwd<br><br>/etc/cifsconfig_share.cfg<br><br>Note: use "rdfile" to read the file|
|CIFS setup|cifs setup<br><br>Note: you will be prompted to answer a number of questions based on what requirements you need.|
|start|cifs restart|
|stop|cifs terminate<br><br># terminate a specific client<br><br>cifs terminate <client_name>\|<IP Address>|
|sessions|cifs sessions<br><br>cifs sessions <user><br><br>cifs sessions <IP Address><br><br># Authentication<br><br>cifs sessions -t<br><br># Changes<br><br>cifs sessions -c<br><br># Security Info<br><br>cifs session -s|
|Broadcast message|cifs broadcast * "message"<br><br>cifs broadcast <client_name> "message"|
|permissions|cifs access <share> <user\|group> <permission><br><br># Examples<br><br>cifs access sysadmins -g wheel Full Control<br><br>cifs access -delete releases ENGINEERING\mary<br><br>Note: rights can be Unix-style combinations of r w x - or NT-style "No Access", "Read", "Change", and "Full Control"|
|stats|cifs stat <interval><br><br>cifs stat <user><br><br>cifs stat <IP Address>|
|create a share|# create a volume in the normal way<br><br># then using qtrees set the style of the volume {ntfs \| unix \| mixed}<br><br># Now you can create your share<br><br>cifs shares -add TEST /vol/flexvol1/TEST -comment "Test Share " -forcegroup workgroup -maxusers 100|
|change share characteristics|cifs shares -change sharename {-browse \| -nobrowse} {-comment desc \| - nocomment} {-maxusers userlimit \| -nomaxusers} {-forcegroup groupname \| -noforcegroup} {-widelink \| -nowidelink} {-symlink_strict_security \| - nosymlink_strict_security} {-vscan \| -novscan} {-vscanread \| - novscanread} {-umask mask \| -noumask {-no_caching \| -manual_caching \| - auto_document_caching \| -auto_program_caching}<br><br># example<br><br>cifs shares -change <sharename> -novscan|
|home directories|# Display home directories<br><br>cifs homedir<br><br># Add a home directory<br><br>wrfile -a /etc/cifs_homedir.cfg /vol/TEST<br><br># check it<br><br>rdfile /etc/cifs_homedir.cfg<br><br># Display for a Windows Server<br><br>net view [\\<Filer](file:///%3cFiler) IP Address><br><br># Connect<br><br>net use * [\\192.168.0.75\TEST](file:///192.168.0.75/TEST)<br><br>Note: make sure the directory exists|
|domain controller|# add a domain controller<br><br>cifs prefdc add lab 10.10.10.10 10.10.10.11<br><br># delete a domain controller<br><br>cifs prefdc delete lab<br><br># List domain information<br><br>cifs domaininfo<br><br># List the preferred controllers<br><br>cifs prefdc print<br><br># Restablishing<br><br>cifs resetdc|
|change filers domain password|cifs changefilerpwd|
|Tracing permission problems|sectrace add [-ip ip_address] [-ntuser nt_username] [-unixuser unix_username] [-path path_prefix] [-a]<br><br>#Examples<br><br>sectrace add -ip 192.168.10.23<br><br>sectrace add -unixuser foo -path /vol/vol0/home4 -a<br><br># To remove<br><br>sectrace delete all<br><br>sectrace delete <index><br><br># Display tracing<br><br>sectrace show<br><br># Display error code status<br><br>sectrace print-status <status_code><br><br>sectrace print-status 1:51544850432:32:78|

File Access using FTP

|   |   |
|---|---|
|Useful Options||
|Enable|options ftpd.enable on|
|Disable|options ftpd.enable off|
|File Locking|options ftpd.locking delete<br><br>options ftpd.locking none<br><br>Note: To prevent users from modifying files while the FTP server is transferring them, you can enable FTP file locking. Otherwise, you can disable FTP file locking. By default, FTP file locking is disabled.|
|Authenication Style|options ftpd.auth_style {unix \| ntlm \| mixed}|
|bypassing of FTP traverse checking|options ftpd.bypass_traverse_checking on<br><br>options ftpd.bypass_traverse_checking off<br><br>Note: If the ftpd.bypass_traverse_checking option is set to off, when a user attempts to access a file using FTP, Data ONTAP checks the traverse (execute) permission for all directories in the path to the file. If any of the intermediate directories does not have the "X" (traverse permission), Data ONTAP denies access to the file. If the ftpd.bypass_traverse_checking option is set to on, when a user attempts to access a file, Data ONTAP does not check the traverse permission for the intermediate directories when determining whether to grant or deny access to the file.|
|Restricting FTP users to a specific directory|options ftpd.dir.restriction on<br><br>options ftpd.dir.restriction off|
|Restricting FTP users to their home directories or a default directory|options ftpd.dir.override ""|
|Maximum number of connections|options ftpd.max_connections n<br><br>options ftpd.max_connections_threshold n|
|idle timeout value|options ftpd.idle_timeout n s \| m \| h|
|anonymous logins|options ftpd.anonymous.enable on<br><br>options ftpd.anonymous.enable off<br><br># specify the name for the anonymous login<br><br>options ftpd.anonymous.name username<br><br># create the directory for the anonymous login<br><br>options ftpd.anonymous.home_dir homedir|
|FTP Commands||
|Log files|/etc/log/ftp.cmd<br><br>/etc/log/ftp.xfer<br><br># specify the max number of logfiles (default is 6) and size<br><br>options ftpd.log.nfiles 10<br><br>options ftpd.log.filesize 1G<br><br>Note: use rdfile to view|
|Restricting access|/etc/ftpusers<br><br>Note: using rdfile and wrfile to access /etc/ftpusers|
|stats|ftp stat<br><br># to reset<br><br>ftp stat -z|

File Access using HTTP

|   |   |
|---|---|
|HTTP Options||
|enable|options httpd.enable on|
|disable|options httpd.enable off|
|Enabling or disabling the bypassing of HTTP traverse checking|options httpd.bypass_traverse_checking on<br><br>options httpd.bypass_traverse_checking off<br><br>Note: this is similar to the FTP version|
|root directory|options httpd.rootdir /vol0/home/users/pages|
|Host access|options httpd.access host=Host1 AND if=e3<br><br>options httpd.admin.access host!=Host1|
|HTTP Commands||
|Log files|/etc/log/httpd.log<br><br># use the below to change the logfile format<br><br>options httpd.log.format alt1<br><br>Note: use rdfile to view|
|redirects|redirect /cgi-bin/* [http://cgi-host/*](http://cgi-host/*)|
|pass rule|pass /image-bin/*|
|fail rule|fail /usr/forbidden/*|
|mime types|/etc/httpd.mimetypes<br><br>Note: use rdfile and wrfile to edit|
|interface firewall|ifconfig f0 untrusted|
|stats|httpstat [-dersta]<br><br># reset the stats<br><br>httpstat -z[derta]|

Network Interfaces

|   |   |
|---|---|
|Display|ifconfig -a<br><br>ifconfig <interface>|
|IP address|ifconfig e0 <IP Address><br><br>ifconfig e0a <IP Address><br><br># Remove a IP Address<br><br>ifconfig e3 0|
|subnet mask|ifconfig e0a netmask <subnet mask address>|
|broadcast|ifconfig e0a broadcast <broadcast address>|
|media type|ifconfig e0a mediatype 100tx-fd|
|maximum transmission unit (MTU)|ifconfig e8 mtusize 9000|
|Flow control|ifconfig <interface_name> <flowcontrol> <value><br><br># example<br><br>ifconfig e8 flowcontrol none<br><br>Note: value is the flow control type. You can specify the following values for the flowcontrol option:<br><br>none    - No flow control<br><br>receive - Able to receive flow control frames<br><br>send    - Able to send flow control frames<br><br>full    - Able to send and receive flow control frames<br><br>The default flowcontrol type is full.|
|trusted|ifconfig e8 untrusted<br><br>Note: You can specify whether a network interface is trustworthy or untrustworthy. When you specify an interface as untrusted (untrustworthy), any packets received on the interface are likely to be dropped.|
|HA Pair|ifconfig e8 partner <IP Address><br><br>## You must enable takeover on interface failures by entering the following commands:<br><br>options cf.takeover.on_network_interface_failure enable<br><br>ifconfig interface_name {nfo\|-nfo}<br><br>nfo   — Enables negotiated failover<br><br>-nfo  — Disables negotiated failover<br><br>Note: In an HA pair, you can assign a partner IP address to a network interface. The network interface takes over this IP address when a failover occurs|
|Alias|# Create alias<br><br>ifconfig e0 alias 192.0.2.30<br><br># Remove alias<br><br>ifconfig e0 -alias 192.0.2.30|
|Block/Unblock protocols|# Block<br><br>options interface.blocked.cifs e9<br><br>options interface.blocked.cifs e0a,e0b<br><br># Unblock<br><br>options interface.blocked.cifs ""|
|Stats|ifstat<br><br>netstat<br><br>Note: there are many options to both these commands so I will leave to the man pages|
|bring up/down an interface|ifconfig <interface> up<br><br>ifconfig <interface> down|

Routing

|   |   |
|---|---|
|default route|# using wrfile and rdfile edit the /etc/rc file with the below<br><br>route add default 192.168.0.254 1<br><br># the full /etc/rc file will look like something below<br><br>hostname netapp1<br><br>ifconfig e0 192.168.0.10 netmask 255.255.255.0 mediatype 100tx-fd<br><br>route add default 192.168.0.254 1<br><br>routed on|
|enable/disable fast path|options ip.fastpath.enable {on\|off}<br><br>Note:<br><br>on   — Enables fast path<br><br>off  — Disables fast path|
|enable/disable routing daemon|routed {on\|off}<br><br>Note:<br><br>on   — Turns on the routed daemon<br><br>off  — Turns off the routed daemon|
|Display routing table|netstat -rn<br><br>route -s<br><br>routed status|
|Add to routing table|route add 192.168.0.15 gateway.com 1|

Hosts and DNS

|   |   |
|---|---|
|Hosts|# use wrfile and rdfile to read and edit /etc/hosts file , it basically use the sdame rules as a Unix<br><br># hosts file|
|nsswitch file|# use wrfile and rdfile to read and edit /etc/nsswitch.conf file , it basically uses the same rules as a<br><br># Unix nsswitch.conf file|
|DNS|# use wrfile and rdfile to read and edit /etc/resolv.conf file , it basically uses the same rules as a<br><br># Unix resolv.conf file<br><br>options dns.enable {on\|off}<br><br>Note:<br><br>on   — Enables DNS<br><br>off  — Disables DNS|
|Domain Name|options dns.domainname <domain>|
|DNS cache|options dns.cache.enable<br><br>options dns.cache.disable<br><br># To flush the DNS cache<br><br>dns flush<br><br># To see dns cache information<br><br>dns info|
|DNS updates|options dns.update.enable {on\|off\|secure}<br><br>Note:<br><br>on     — Enables dynamic DNS updates<br><br>off    — Disables dynamic DNS updates<br><br>secure — Enables secure dynamic DNS updates|
|time-to-live (TTL)|options dns.update.ttl <time><br><br># Example<br><br>options dns.update.ttl 2h<br><br>Note: time can be set in seconds (s), minutes (m), or hours (h), with a minimum value of 600 seconds<br><br>and a maximum value of 24 hour|

VLAN

|   |   |
|---|---|
|Create|vlan create [-g {on\|off}] ifname vlanid<br><br># Create VLANs with identifiers 10, 20, and 30 on the interface e4 of a storage system by using the following command:<br><br>vlan create e4 10 20 30<br><br># Configure the VLAN interface e4-10 by using the following command<br><br>ifconfig e4-10 192.168.0.11 netmask 255.255.255.0|
|Add|vlan add e4 40 50|
|Delete|# Delete specific VLAN<br><br>vlan delete e4 30<br><br># Delete All VLANs on a interface<br><br>vlan delete e4|
|Enable/Disable GRVP on VLAN|vlan modify -g {on\|off} ifname|
|Stat|vlan stat <interface_name> <vlan_id><br><br># Examples<br><br>vlan stat e4<br><br>vlan stat e4 10|

Interface Groups

|   |   |
|---|---|
|Create (single-mode)|# To create a single-mode interface group, enter the following command:<br><br>ifgrp create single SingleTrunk1 e0 e1 e2 e3<br><br># To configure an IP address of 192.168.0.10 and a netmask of 255.255.255.0 on the singlemode interface group SingleTrunk1<br><br>ifconfig SingleTrunk1 192.168.0.10 netmask 255.255.255.0<br><br># To specify the interface e1 as preferred<br><br>ifgrp favor e1|
|Create ( multi-mode)|# To create a static multimode interface group, comprising interfaces e0, e1, e2, and e3 and using MAC<br><br># address load balancing<br><br>ifgrp create multi MultiTrunk1 -b mac e0 e1 e2 e3<br><br># To create a dynamic multimode interface group, comprising interfaces e0, e1, e2, and e3 and using IP<br><br># address based load balancing<br><br>ifgrp create lacp MultiTrunk1 -b ip e0 e1 e2 e3|
|Create second level intreface group|# To create two interface groups and a second-level interface group. In this example, IP address load<br><br># balancing is used for the multimode interface groups.<br><br>ifgrp create multi Firstlev1 e0 e1<br><br>ifgrp create multi Firstlev2 e2 e3<br><br>ifgrp create single Secondlev Firstlev1 Firstlev2<br><br># To enable failover to a multimode interface group with higher aggregate bandwidth when one or more of<br><br># the links in the active multimode interface group fail<br><br>options ifgrp.failover.link_degraded on<br><br>Note: You can create a second-level interface group by using two multimode interface groups. Secondlevel interface groups enable you to provide a standby multimode interface group in case the primary multimode interface group fails.|
|Create second level intreface group in a HA pair|# Use the following commands to create a second-level interface group in an HA pair. In this example,<br><br># IP-based load balancing is used for the multimode interface groups.<br><br># On StorageSystem1:<br><br>ifgrp create multi Firstlev1 e1 e2<br><br>ifgrp create multi Firstlev2 e3 e4<br><br>ifgrp create single Secondlev1 Firstlev1 Firstlev2<br><br># On StorageSystem2 :<br><br>ifgrp create multi Firstlev3 e5 e6<br><br>ifgrp create multi Firstlev4 e7 e8<br><br>ifgrp create single Secondlev2 Firstlev3 Firstlev4<br><br># On StorageSystem1:<br><br>ifconfig Secondlev1 partner Secondlev2<br><br># On StorageSystem2 :<br><br>ifconfig Secondlev2 partner Secondlev1|
|Favoured/non-favoured interface|# select favoured interface<br><br>ifgrp nofavor e3<br><br># select a non-favoured interface<br><br>ifgrp nofavor e3|
|Add|ifgrp add MultiTrunk1 e4|
|Delete|ifconfig MultiTrunk1 down<br><br>ifgrp delete MultiTrunk1 e4<br><br>Note: You must configure the interface group to the down state before you can delete a network interface<br><br>from the interface group|
|Destroy|ifconfig ifgrp_name down<br><br>ifgrp destroy ifgrp_name<br><br>Note: You must configure the interface group to the down state before you can delete a network interface<br><br>from the interface group|
|Enable/disable a interface group|ifconfig ifgrp_name up<br><br>ifconfig ifgrp_name down|
|Status|ifgrp status [ifgrp_name]|
|Stat|ifgrp stat [ifgrp_name] [interval]|

Diagnostic Tools

|   |   |
|---|---|
|Useful options||
|Ping thottling|# Throttle ping<br><br>options ip.ping_throttle.drop_level <packets_per_second><br><br># Disable ping throttling<br><br>options ip.ping_throttle.drop_level 0|
|Forged IMCP attacks|options ip.icmp_ignore_redirect.enable on<br><br>Note: You can disable ICMP redirect messages to protect your storage system against forged ICMP redirect attacks.|
|Useful Commands||
|netdiag|The netdiag command continuously gathers and analyzes statistics, and performs diagnostic tests. These diagnostic tests identify and report problems with your physical network or transport layers and suggest remedial action.|
|ping|You can use the ping command to test whether your storage system can reach other hosts on your network.|
|pktt|You can use the pktt command to trace the packets sent and received in the storage system's network.|
