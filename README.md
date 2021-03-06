# Restic Backup

## Help 
```
./restic_wrapper help
```

## Crontab
```
 30 3   */2 *   *       ~/bin/restic_wrapper cheesefactory full-auto-backup
```

## Config files (must be sibling files of this script)
cheesefactory.env:
```
export MAILTO=admin@cheesefactory.com
export RESTIC_REPOSITORY=/media/cheese_backup_repo
export RESTIC_PASSWORD=sup3rs3cr3tp4ssphr4s3hunter2
export RESTIC_SOURCE=/home/user/secret_cheese_recipes
```

cheesefactory.excludefile (must exist, can be empty):
```
/home/user/secret_cheese_recipes/ignore_this_dir/
/home/user/secret_cheese_recipes/ignore_this_dir_also/
```

smtp.env  (must exist, can be empty):
```
export NOTIFICATION_SMTP_URL=smtp.eu.mailgun.org:587
export NOTIFICATION_SMTP_USER=user
export NOTIFICATION_SMTP_PASSWORD=p4ssw0rd
export NOTIFICATION_MAIL_FROM=backups@cheesefactory.com
```

## Keep in mind
- pause backups while restoring, restore dies when lockfile gets created (I think)

## Changelog
**v0.1.0**: Added `export-config` option to create a tarball of all your config files.
