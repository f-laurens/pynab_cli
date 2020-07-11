# [pynab_cli](https://github.com/f-laurens/pynab_cli/archive/master.zip)

A collection of small command line tools for [Pynab](https://github.com/nabaztag2018/pynab)-running Nabaztag rabbits that have been revived using the [Raspberry Pi Zero W](https://www.raspberrypi.org/products/raspberry-pi-zero-w)-powered [Tag:tag:tag](https://www.tagtagtag.fr/index_eng.html) card.

## nabaztag

A script to send packets to the rabbit's nabd daemon, according to the [Pynab protocol](https://github.com/nabaztag2018/pynab/blob/master/PROTOCOL.md).

		pi@Nabaztag:~ $ nabaztag -h
		Usage: nabaztag [-g | -e | -l | -s | -w | -p LEFT.RIGHT | -r | -c COMMANDFILE] [HOST]
			Talk to rabbit HOST (default: localhost)
			 no option :	get state
				-h :	this usage help
				-g :	get gestalt status
				-e :	execute ears test
				-l :	execute LEDs test
				-s :	go to sleep
				-w :	wake up
				-p :	rotate ears to position LEFT.RIGHT
				-r :	rotate ears to random position
				-c :	execute given JSON COMMANDFILE
			
Examples of JSON command files are provided in the `json` directory.  
This script should be usable on a non-rabbit OS, if it provides the `nc` (`netcat`) command.

**Note:**  
Talking to a remote rabbit is possible only if the nabd socket on this rabbit has been enabled for public access.

## tagtagtag-sound

A script to get/set the rabbit's Tag:tag:tag sound card low/high volume levels.

		pi@Nabaztag:~ $ tagtagtag-sound -h
		Usage: tagtagtag-sound [-t | -T] [-m low | -M high]
			 no option :	get sound volume levels
				-h :	this usage help
				-t :	handle volume for Nabaztag
				-T :	handle volume for Nabaztag:tag (default)
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

## pynab

A script to manage Pynab services.
		
		pi@Nabaztag:~ $ pynab -h
		Usage: pynab [-status | -start | -stop | -restart | -local | -public | -log [NUM]]
			no option :	show status of Pynab services
			    -help :	this usage help
			   -start :	start Pynab services
			    -stop :	stop Pynab services
			 -restart :	restart Pynab services
			  -status :	show status of Pynab services
			   -local :	restrict nabd socket to local access
			  -public :	open nabd socket to public access
			     -log :	show log tails (last NUM lines) for Pynab daemons

**Note:**  
By default, rabbit's nabd socket is restricted to local access (from the rabbit itself).  
Opening it to public access makes it accessible from other hosts. This is a **potential security risk** if the rabbit is not on a local network protected by a firewall.