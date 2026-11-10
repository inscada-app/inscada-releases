# inSCADA Releases

Official releases and nightly builds of [inSCADA](https://inscada.com), the industrial SCADA platform, and how
to update an installation to them.

## What is published here

| | Official release | Nightly |
|---|---|---|
| Name | `<yyyyMMdd>-<n>`, e.g. `20261108-11` | marked **Pre-release** |
| Files | `app-<rev>.jar`, `app-<rev>.jar.sig`, `inscada-launcher-<rev>.zip` | `app-<rev>.jar` only |
| Signed | yes | no |
| Use | production | testing only - not for production; the update button refuses it |

Each official release page has two lists:

- **What's new** - what changed for you.
- **Before you upgrade** - changes that take something away or narrow what a configuration may do.
  **Read it before updating.** When you skip releases, read it on every release in between that is still listed.

## Updating with the update button

For an installation whose service starts inSCADA through the launcher (`inscada-launcher.jar`). An installation
set up before the launcher existed can be moved to it once - see [the next section](#moving-an-existing-installation-to-the-update-button-once).

1. Open **Settings → Server → Update**.
2. Get the release onto the server, either way:
   - press **Download &lt;version&gt;** when the card offers a newer official release (give a reason; downloading
     installs nothing). The card looks for a newer release once a day; from 20261110-7 on, **Check now** on the
     card asks at once, and on an earlier release restarting the inSCADA service makes it look a minute later. Or
   - on a server without internet access, copy `app-<rev>.jar` **and** `app-<rev>.jar.sig` from the release page
     into the `update\pending` folder of the installation (`C:\Program Files\inSCADA\update\pending`).
3. Check the card: the launcher's verdict on the release must be accepted, and every precondition must hold
   (disk space for the database backup, the licence's maintenance period, no Settings → Server change waiting for
   a restart, and the others the card lists).
4. Press **Update and restart**. inSCADA takes a database backup, then restarts; the launcher verifies the release's
   signature, puts the new `inscada.jar` in place and starts it. The update is complete when the new release reports
   ready.

If the new release does not come up, nothing is rolled back by itself. The rollback command for this server - the
previous jar and the database backup taken before the update - is shown on the card and written to
`update\state\result.properties`. Run it in an administrator command prompt.

**A cluster is not updated by the button.** Update its nodes by hand, in the order the cluster documentation gives.

## Moving an existing installation to the update button (once)

An installation made before the launcher starts `inscada.jar` directly, and the update button is not offered. Two
steps move it over; the setup does not need to be run again.

1. **Update to the latest official release by hand**, as [below](#installations-without-the-launcher-manual-update):
   read "Before you upgrade", stop the service, replace `inscada.jar` with `app-<rev>.jar`, start the service.
2. **Enable the update button.** Unzip `inscada-launcher-<rev>.zip` of the same release into one folder on the
   server (the desktop will do). It holds `inscada-launcher.jar`, `enable-updates.bat` and `enable-updates.ps1`.
   Right-click `enable-updates.bat` → **Run as administrator**.

   It checks the installation first and changes nothing if a check fails, saying why. It then shows what it will
   change, copies `inscada-launcher.jar` next to `inscada.jar` and points the service at it. It does not touch
   `inscada.jar`, the database or the configuration. Then it asks:

   > Restart the inSCADA service now, so that it starts through the launcher?

   Answer `y` to restart now (SCADA is unavailable during the restart); it waits for the product to come up through
   the launcher and says whether it did. Any other answer leaves the service running; it starts through the launcher
   at its next restart.

   To go back: `enable-updates.bat /undo` puts the previous service command back, so the service starts
   `inscada.jar` again.

The zip carries `enable-updates.bat` and `enable-updates.ps1` from the release that follows `20261108-11`. For
`20261108-11` itself, the zip holds only `inscada-launcher.jar`; take the two scripts from the inSCADA setup package.

## Installations without the launcher (manual update)

1. Read **Before you upgrade** on the release page, and on every release you skip.
2. Take a database backup. The new release migrates the database when it starts, so the backup is the way back.
3. Stop the inSCADA service.
4. Replace `inscada.jar` in the installation folder with the release's `app-<rev>.jar`, renamed to `inscada.jar`.
5. Start the service.

## Support

For support and documentation, visit [inscada.com](https://inscada.com).
