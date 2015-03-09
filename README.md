tar-multibackup
===============


Bash script to backup multiple folders and clean up old backups based on a configurable retention time.

### Installation

    cd /usr/local/src
    git clone https://github.com/frdmn/tar-multibackup.git
    ln -sf /usr/local/src/tar-multibackup/multibackup /usr/local/bin/multibackup
    cp /usr/local/src/tar-multibackup/multibackup.conf ~/.multibackup.conf

### Configuration

* `timestamp` = Format of the timestamp, used in the backup target filename
* `backup_destination` = Directory which is used to store the archives/backups
* `folders_to_backup` = Array of folders to backup
* `backup_retention` = Retention time how long we should keep the backups
* `pre_commands` = Array of commands that are executed before the backup starts (stop specific service)
* `post_commands` = Array of commands that are executed after the backup finished (start specific service)

### Example configuration 

In the example below you can find a `multibackup` configuration file to backup a productional [LiveConfig](http://www.liveconfig.com/) instance.

    # Timestamp format, used in the backup target filename
    timestamp=$(date +%Y%m%d)

    # Destination where you want to store your backups
    backup_destination="/var/backups"

    # Folders to backup
    folders_to_backup=(
      '/etc'
      '/var/mail'
      '/var/www'
      '/var/lib/mysql'
      '/var/spool/cron'
      '/var/lib/liveconfig'
    )

    # How long to you want to keep your backups (in days)
    backup_retention="+7"

    # Commands that are executed before the backup started
    # (We have to make sure the liveconfig process is not running)
    # (otherwise the databases changes while we try to save it)
    pre_commands=(
      "service liveconfig stop"
    )

    # Commands that are executed after the backup is completed
    # (To restart the liveconfig process again, once the backup is completed)
    post_commands=(
      "service liveconfig start"
    )

### Version
1.0.0

### Lincense
[WTFPL](LICENSE)
