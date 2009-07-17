# NAME

__mythtvepisodes__ - Adds minor episode guide ability
to MythTV.

# DESCRIPTION

Run it after mythfilldatabase and it will trim out unwanted 
episodes from shows you are recording.  Episode information 
is stored in a new `episode` table which is populated with 
data from epguides.com.  Once you populate the episode table 
and mark the episodes you don't want, you just setup this script
in your crontab file and forget about it.

Because the data comes from epguides.com, episode names may not
always match.  The only affect of that is sometimes episodes 
get recorded when they should be skipped.

# OPTIONS

- __-help__

Display this message and exit.

- __-setup__

Adds the `episode` table to the mythconverg database. This only needs
to be done once.

- __-add__ <showname>

Screen scrapes <showname> from epguides.com and populates the
`episode` table with the episode and season data.  This is usually
done once per show.

- __-skip__ <showname> n1 n2...

Marks the specified seasons of the show as unwanted.  

- __-show__ <showname>

Lists the show's epidode information grouped by season with `x`
before any episodes that shoul be skipped.

- <no-params>

When run with no parameters, __mythtvepisodes__ will search the
`program` table for any episodes of seasons you have previously
flagged with the -skip command.  The unwanted episodes will be added
to the `oldrecorded` table and show up as duplicates.

# PREREQUISITES

The `DBI` modules are required for connecting to the MythTV database.

# EXAMPLES

Say you like `Buffy the Vampire Slayer` and want to capture the show,
but already have several seasons on DVD.  You can use
__mythtvepisodes__ to mark the unwanted seasons as having already
been recorded.  Then they will appear as duplicates and the MythTV
scheduler will not record them.  The following commands would set that
up for you.

  % mythtvepisodes -add "Buffy the Vampire Slayer"

This will download the page
`http://www.epguides.com/BuffytheVampireSlayer/` and the episode data
to the mythconverg database.

  % mythtvepisodes -skip "Buffy the Vampire Slayer" 1 2 3 4 5

Marks seasons 1, 2, 3, 4, and 5 as unwanted.

  % mythtvepisodes -show "Buffy the Vampire Slayer"

Lists the episode information in the database with `x` before the
unwanted episodes.  Not required, but nice to see everything is ok.

  % mythtvepisodes

Checks the database for shows that should be skipped and adds them to
oldrecorded.  This should be done from cron after mythfilldatabase is
run or whenever you add new shows to the episode table as above.

# AUTHOR

David Blevins <david.blevins@visi.com>

# COPYRIGHT

Copyright (c) 2004 David Blevins. All rights reserved. This program is
free software; you can redistribute it and/or modify it under the same
terms as Perl itself.