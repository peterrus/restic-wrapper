# Restic Backup Wrapper

This wrapper attempts to make running restic a bit easier. It reads configuration files (in the form of `.env` files) and provides shortcuts to be used in manual and automated (cronjob's) situations alike. I tried to keep the script as simple as possible so you can hack on it to suit your needs. Notifications about the result of a backup job are handled by calling a script called `restic_notify`. The one provided in this repository sends mail over SMTP.

This script should work without problems on most linux distributions. I can not vouch for OSX but feedback is welcome.

<!-- TOC GFM -->

- [Setup](#setup)
- [Notifications](#notifications)
- [Scheduled execution](#scheduled-execution)
- [Changelog](#changelog)

<!-- /TOC -->

## Setup

After checking out this repo (or just downloading the script) create config files (in the same directory as `restic_wrapper`). Replace 'cheesefactory' with whatever you want to call your backup job. You can have multiple!

**cheesefactory.env**:
```bash
export RESTIC_REPOSITORY=/media/cheese_backup_repo
export RESTIC_PASSWORD=sup3rs3cr3tp4ssphr4s3hunter2
export RESTIC_SOURCE=/home/user/secret_cheese_recipes
```

**cheesefactory.excludefile** (must exist, can be empty):
```bash
/home/user/secret_cheese_recipes/ignore_this_dir/
/home/user/secret_cheese_recipes/ignore_this_dir_also/
```
(See [Restic Docs](https://restic.readthedocs.io/en/latest/040_backup.html#excluding-files) for examples)

When you have created the config files it might be wise to back them up for safekeeping using `./restic_wrapper -a export-config`.

If you have not initialized a Restic repository at the location you set in `RESTIC_REPOSITORY` be sure to create one using `./restic_wrapper -a run -r init`.

You are now ready to use the wrapper. See `./restic_wrapper` (without parameters) for all options.

## Notifications

By default restic-wrapper outputs all status information to the terminal. You can route that output to the `restic_notify` script by providing the `-m` flag like so: `./restic_wrapper -b cheesefactory -a backup -m`.

The `restic_notify` script in this repository sends email over SMTP and sets the subject according to whether or not the backup job succeeded. Mail is sent using `s-nail` so be sure to install that using your system's package manager.

**smtp.env** (if you are using the example 'restic_notify' script:
```
export NOTIFICATION_SMTP_URL=smtp.eu.mailgun.org:587
export NOTIFICATION_SMTP_USER=admin@cheesefactory.biz
export NOTIFICATION_SMTP_PASSWORD=super-secret-smtp-password
export NOTIFICATION_MAIL_FROM=admin@cheesefactory.biz
export NOTIFICATION_MAIL_TO=admin@cheesefactory.biz
```

## Scheduled execution
You can set up a cronjob for (for example) nightly backups at 3:30.

```
 30 3 * * *       ~/restic_wrapper/restic_wrapper -b cheesefactory -a backup-full -m
```

If your system has an [MTA](https://cronitor.io/cron-reference/no-mta-installed-discarding-output) installed you will also get email if restic_wrapper itself fails for some reason but because most users do not have a properly configured MTA I chose to integrate basic notification functionality in restic_wrapper. Another advantage of letting restic_wrapper handle the notifications is that it can send different mails for failed and succeeded jobs without needing a lot of bash magic (which not everyone might be comfortable with).

## Changelog
**v0.2.1**: Miscellaneous documentation updates.
**v0.2.0**: Externalize notification functionality to make the script a lot simpler (and refactored a bunch).
**v0.1.0**: Added `export-config` option to create a tarball of all your config files.
