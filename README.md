# pynab_cli

A collection of small command line tools for [Pynab](https://github.com/nabaztag2018/pynab)-powered Nabaztag rabbits that have been revived using the [Tag:tag:tag](https://www.tagtagtag.fr/index_eng.html) card.

## nabaztag

A script to send packets to the rabbit's nabd daemon, according to the [Pynab protocol](https://github.com/nabaztag2018/pynab/blob/master/PROTOCOL.md).

		pi@Nabaztag:~ $ nabaztag -h
		Usage: nabaztag [-g | -e | -l | -s |-w | -r | -c commandfile]
			 no option :	get state
				-h :	this usage help
				-g :	get gestalt status
				-e :	execute ears test
				-l :	execute LEDs test
				-s :	go to sleep
				-w :	wake up
				-r :	rotate ears to random position
				-c :	execute given JSON command file
			
Examples of JSON command files are provided in the `json` directory.	

## tagtagtag-sound

A script to get/set the rabbit's Tag:tag:tag sound card low/high volume levels.

		pi@Nabaztag:~ $ tagtagtag-sound -h
		Usage: tagtagtag-sound [-m low | -M high]
			 no option :	get Nabaztag:tag sound volume levels
				-h :	this usage help
				-m :	set minimum volume level
				-M :	set maximum volume level

## backup/restore_pynab_db

Scripts to backup and restore the rabbit's Pynab PostgreSQL database.

		pi@Nabaztag:~ $ backup_pynab_db -h
		Usage: backup_pynab_db [-u DB_USER] [-d DB_NAME] [-b DB_BACKUP_FILE]
			Backup (as user DB_USER) PostgreSQL database DB_NAME to file DB_BACKUP_FILE
			defaults: DB_USER=postgres DB_NAME=pynab DB_BACKUP_FILE=pynab.db.sql
		
		pi@Nabaztag:~ $ restore_pynab_db -h
		Usage: restore_pynab_db [-u DB_USER] [-b DB_BACKUP_FILE]
			Restore (as user DB_USER) PostgreSQL database from file DB_BACKUP_FILE
			defaults: DB_USER=postgres DB_BACKUP_FILE=pynab.db.sql
			
**Note:**  
Backup can be done on the fly (while the rabbit's Pynab services are running). According to PostgreSQL documentation, the backup is consistent (based on a 'snapshot' of the database).  
On the other hand restore must be done without concurrent accesses to the database (so that it can be re-created). Pynab services must therefore be stopped beforehand and restarted after the backup (the script takes care of this).