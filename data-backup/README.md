# Backups

Timestamped exports from the app land here when you use **Export to
backup folder** in Settings on a desktop browser that supports it
(Chrome/Edge). The first time, your browser will ask you to pick a
folder — choose this one (`data-backup`). After that, every export
writes straight into it with no dialog.

Filenames look like `sem-validation-2026-09-15_1645.json`.

On a phone, or in Safari, that browser API isn't available, so export
falls back to a normal download instead — move that file in here
manually (AirDrop, cloud drive, email to yourself, cable, whatever's
easiest) if you want it kept alongside the rest.

To restore one: open the app → Settings → Backup → **Import from
file** → pick the `.json` file. Works from any device.

These files are plain JSON and safe to commit to git if you want a
history of them.
