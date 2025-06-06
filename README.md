# [pynab_cli](https://github.com/f-laurens/pynab_cli/archive/release.zip)

A collection of small command line tools for [Pynab](https://github.com/nabaztag2018/pynab)-running Nabaztag rabbits that have been revived using the [Raspberry Pi Zero W](https://www.raspberrypi.org/products/raspberry-pi-zero-w)-powered [Tag:tag:tag](https://www.tagtagtag.fr/index_eng.html) card.

## nabaztag

A script to send packets to the rabbit's nabd daemon, according to the [Pynab protocol](https://github.com/nabaztag2018/pynab/blob/master/PROTOCOL.md).

		pi@Nabaztag:~ $ nabaztag -h
		Usage: nabaztag [-g | -e | -l | -m | -s | -w | -p LEFT.RIGHT | -r | -v | -c COMMANDFILE] [HOST]
			Talk to rabbit HOST (default: localhost)
			 no option :	get state
				-h :	this usage help
				-g :	get gestalt status
				-e :	execute ears test
				-l :	execute LEDs test
				-m :	monitor asr/button/ears/rfid events
				-s :	go to sleep
				-w :	wake up
				-p :	rotate ears to position LEFT.RIGHT
				-r :	rotate ears to random position
				-v :	display virtual rabbit (running in Docker on HOST)
				-c :	execute given JSON COMMANDFILE
			
Examples of JSON command files are provided in the `json` directory.  
This script should be usable on a non-rabbit OS, if it provides the `nc` (`netcat`) command.

**Note:**  
Talking to a remote rabbit is possible only if the nabd socket on this rabbit has been enabled for public access.

## tagtagtag-sound

A script to get/set the rabbit's Tag:tag:tag sound card low/high volume levels.

		pi@Nabaztag:~ $ tagtagtag-sound -h
		Usage: tagtagtag-sound [-t | -T] [-m low | -M high] [-p]
			 no option :	get sound volume levels
				-h :	this usage help
				-t :	handle volume for Nabaztag
				-T :	handle volume for Nabaztag:tag (default)
				-m :	set minimum volume level
				-M :	set maximum volume level
				-p :	patch/unpatch ADC mixer (to eliminate NFC card interference)

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
		Usage: pynab [-status | -start | -stop | -restart | -enable | -disable | -local | -public | -log [NUM]]
			no option :	show status of Pynab services
			    -help :	this usage help
			   -start :	start Pynab services
			    -stop :	stop Pynab services
			 -restart :	restart Pynab services
			  -enable :	enable Pynab services
			 -disable :	disable Pynab services
			  -status :	show status of Pynab services
			   -local :	restrict nabd socket to local access
			  -public :	open nabd socket to public access
			     -log :	show log tails (last NUM lines) for Pynab daemons

**Note:**  
By default, rabbit's nabd socket is restricted to local access (from the rabbit itself).  
Opening it to public access makes it accessible from other hosts. This is a **potential security risk** if the rabbit is not on a local network protected by a firewall.

## Miscellaneous administration

A collection of wrapper scripts for Pynab development & administration:
- update_pynab_messages

		pi@Bunny:~ $ update_pynab_messages -h
		Usage: update_pynab_messages [-m] [-c] [MODULE...]
			Make (if -m) and Compile (if -c) localisation messages for Pynab modules MODULE...
			defaults: MODULES='nab?*d nabweb'
- migrate_pynab_models

		pi@Bunny:~ $ migrate_pynab_models -h
		Usage: migrate_pynab_models [-c] [-m]
			Create new (if -c) and Execute (if -m) Pynab data models migrations
- django_admin_pynab

		pi@Bunny:~ $ django_admin_pynab  -h
		Usage: django_admin_pynab [COMMAND ARGS...]
			Execute django-admin script for Pynab
- rebuild_pynab_drivers

		pi@Bunny:~ $ rebuild_pynab_drivers -h
		Usage: rebuild_pynab_drivers [-f]
			Rebuild (if -f) TagTagTag drivers for Pynab
- upgrade_pynab

		pi@Bunny:~ $ upgrade_pynab -h
		Usage: upgrade_pynab [-u]
			Run (if -u) Pynab upgrade script
- install_pynab

		pi@Bunny:~ $ install_pynab -h
		Usage: install_pynab [-i] [-u]
			Run (if -i) Pynab install script (in upgrade mode if -u)
- check_pynab_syntax

		pi@Bunny:~ $ check_pynab_syntax -h
		Usage: check_pynab_syntax [-b] [-p] [-f] [-i] [MODULE...]
			Run Python syntax checkers on Pynab modules MODULE...
			-b : run black   -p : run pycodestyle   -f : run flake8   -i : run isort
			defaults: MODULES='nabcommon nabboot nabd nab?*d nabweb'
- pytest_pynab

		pi@Bunny:~ $ pytest_pynab -h
		Usage: pytest_pynab
			Run pytest unit tests for Pynab

**Note:**  
These should be used with caution (**only knowingly**).
