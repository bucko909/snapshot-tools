snapshot-tools
==============

Some stuff I use to manage btrfs almost-streaming backups

SETUP
=====

Generate a key on each machine that'll have backups.

To anything that'll get backups, add this to ~root/.ssh/authorized_keys for each such generated key:

  command="/root/snapshot-tools/btrfs-recv-only" <public-key-here>

(Should probably do this more securely with sudo or something.)

Then schedule something to run

  btrfs-cleanup-all

every hour or so on all involved hosts. This will remove older backups, leaving a string of exponentially-spaced backups running into to the past. The script isn't /ideal/, but it keeps a few really old snapshots, and a number of new-ish ones. It'll also not remove anything that's a "current" snapshot on another host. I currently have:

20140627-071002
20140909-071003
20141007-101003
20141108-071003
20141202-071004
20141203-071004
20141204-071003
20141205-032002
20141205-052002
20141205-071003
20141205-072004
20141205-082003
20141205-090003
20141205-092002
20141205-094002
20141205-095003
20141205-100004

Snapshots on btrfs don't take up much space, but the little advice I found suggested trying to keep less than a few thousand of them.

Schedule something to run

  btrfs-send-host <target>

on hosts that /send/ them. I send every 10 minutes. You could do this more often, but may need to schedule the cleanup more often if so.


Now you just need to

  btrfs-add <absolute-path>

for each directory you want to backup (this will replace the directory with a subvolume rather impolitely, so don't do it on something that's in use right now).

You can't add the root directory this way.
