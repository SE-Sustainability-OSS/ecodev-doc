# Backup 

!!! warning
    Backup is one of the reason why mature structures should switch to managed services, in order 
    to ease the manual work and benefit from the long experience of the hyperscalers. As often 
    the solution presented here should only be used by individuals/small SME/startups in their infancies.

The `backup.md` module of ecodev-core has only one `backup` method (an original name 😁).

## `backup`

`backup` eats 

- a pathlib <a href=https://docs.python.org/3/library/pathlib.html class="external-link" target="_blank">Path</a> `backed_folder` corresponding to a folder one wants to backup 
- (Optional) a `nb_saves` int (default value 5) corresponding to the maximum number of backups to keep. 

The `backup` method also uses some environment variables, meant to connect to a <a href=https://www.digitalocean.com/community/tutorials/what-is-ftp-and-how-is-it-used class="external-link" target="_blank">ftp</a> server

- `backup_username`: the username to connect to the backup server
- `backup_password`: the password associated to `backup_username`
- `backup_url`: the backup server url address

`backup` will backup both `backed_folder` and the db on which the application running the backup is plugged (thanks to  <a href=https://www.postgresql.org/docs/current/app-pgdump.html class="external-link" target="_blank">`pg_dump`</a> . 

It will connect to the backup server thanks to  <a href=https://lftp.yar.ru/ class="external-link" target="_blank">lftp</a>.


## `retrieve_most_recent_backup`

use this method when you want to restore the most recent backup. It will retrieve from the lftp the most recent `tar.gz` backup. 