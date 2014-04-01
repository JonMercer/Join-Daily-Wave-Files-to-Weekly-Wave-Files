WavWeeklyJoin
=============

Concatenates multiple .wav files based on the week they were created. Used for merging daily audio journals into weekly audio journals.

Use Case: 
You record a daily journal and you want to merge them by week for easy listening in the future. Similar to a podcast.

Example input:
2013-12-22 Sun 15:11.wav
2013-12-23 Mon 14:30.wav
2013-12-23 Mon 17:45.wav
2013-12-26 Thu 06:30.wav
2013-12-29 Sun 07:22.wav // Ths is where the next week starts for 2014
2013-12-29 Sun 08:44.wav
2013-12-31 Tue 06:30.wav
2014-01-01 Wed 09:33.wav
2014-01-03 Thu 09:33.wav

Example output:
2013-52.wav
2014-01.wav
