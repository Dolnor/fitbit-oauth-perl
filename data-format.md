# The Weightbot CSV Format

A sample line from the top of my Weightbot data:

	2008-10-28,83.1,183.2

The format appears to be:
	<ISO timestamp>,<weight in kg>,<weight in lbs>
	
# MiScale CSV Format

	Owner,69.8,2015-07-21,07:06:00,69.5,183,20.84

The format used by mihealth sql query:
	<User>,<Record Weight>,<Date>,<Time>,<Goal Weight>,<Height>,<BMI>
