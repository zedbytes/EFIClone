# EFIClone

`EFIClone` is a macOS bash shell script for Hackintosh machines that is designed to integrate with either Carbon Copy Cloner or SuperDuper! - the two most popular macOS disk cloning utilities.

CCC and SD! will both automatically create bootable clones on real Macintoshes in a single step. Though modern Macs support EFI booting in order to maintain compatibility with running non-Apple operating system on their hardware, they do NOT need the EFI partition in order to boot macOS. Because of this these disk cloning utilities do not copy the contents of the secondary EFI partition from one drive to another when doing their job.

This is where `EFIClone` comes in.

Both CCC and SD! have the ability to configure a "post flight" script that will be launched when the main clone job has been completed. They pass details of the source and destination drives that were used in the clone job to these scripts, from which the script is able to figure out the associated EFI partitions to automatically copy the contents of the critical EFI folder from the source drive to the destination drive.

The script provides extensive logging, has a "test" mode that will log its actions during a dry run without modifying any data and sends notifications to the notification center with the results of the run.

When configured in your CCC or SD! clone job, `EFIClone` will allow you to do a single-step clone from your current hackintosh drive to a truly bootable backup drive with no other steps required.

## Disclaimer

We are not responsible for any data loss that might occur on your system as a result of this script. Please use common sense and always have a backup handy when attempting things such as this.

## Configuration

There are currently only two user configuration settings. Since this is a script file, they have to be manually edited with a text editor.

The most important setting is `TEST_SWITCH`.

```bash
TEST_SWITCH="Y"
```

A value of `Y` tells the script to do a dry run - no data will be modified.
Any other value (preferably `N` for consistency) allows the script to run in normal mode - it will delete the contents of the destination EFI partition and replace them with the contents of the source EFI partition.

**It is recommended to run the script in test mode at least once before doing a full run.**

The only other setting is the path where the log file will be written out.

```bash
LOG_FILE="/Users/Shared/EFIClone.log"
```

There is no need to change this setting except for convenience. It is recommended to leave it at its default  value.

## Usage

For a more detailed installation guide please reference [this excellent writeup](https://www.tonymacx86.com/threads/success-gigabyte-designare-z390-thunderbolt-3-i7-9700k-amd-rx-580.267551/#Bootable%20Backup) of CaseySJ on the TonyMacx86 forums.

To prepare the setup, download the file `EFIClone.sh` and place it anywhere on your system that is accessible.

The configuration of both utilities is similar, but not exact. See the following sections for each.

### Carbon Copy Cloner

1. Create a Clone task as you normally would, defining the Source and Destination partitions.
2. Click on the `Advanced Settings` button, just below the Source partition.
3. The advanced settings pane will open. If necessary, scroll down until you can see the section labeled `AFTER COPYING FILES` and click on the folder icon next to `Run a Shell Script:`
4. Use the file selector window to select `EFIClone.sh` from the folder you moved it to after downloading.
5. After you have selected the script your task should have the script name `EFIClone.sh` showing next to the `Run a Shell Script:` line.

If you need to remove the script you can click on the `X` icon to detach the script from your CCC Task.

### SuperDuper!

1. Choose your Source and Destination partitions in the `Copy` and `to` dropdown menus.
2. Click on the `Options...` button.
3. This will display the `General` options tab. Click on `Advanced` to show the advanced options pane.
4. Check the box that says `Run shell script after copy completes` and click on `Choose...`
5. Use the file selector window to select `EFIClone.sh` from the folder you moved it to after downloading.
67. After you have selected the script your task should have the script name `EFIClone.sh` showing next to the `Run shell script after copy completes` line.
