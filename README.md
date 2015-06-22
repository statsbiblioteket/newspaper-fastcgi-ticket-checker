newspaper-fastcgi-ticket-checker
================================

Newspaper access ticker checker - Perl script running via fastcgi

In order to validate access to a given resource we must check that the
given request corresponds to the credentials stored in JSON format in
memcached.

Development is expected to happen on achernar.  When check.pl or
CheckTicket.pm is updated run "sudo /usr/local/sbin/restart_httpd.sh"
to reload the script, and then run "perf/test-page.sh" which expects
one or more lines containing a DOMS uuid on stdin, does the proper
voodoo, and then downloads the corresponding assets to the "perf/tmp"
folder.  As of 2015-06-19 this is hardcoded to the test environment
including achernar.


Under Ubuntu 14.10 adapt /etc/apache2/apache2.conf similarly to

  <Directory /var/www/>
	Options Indexes FollowSymLinks
	AllowOverride None
	FcgidAccessChecker /home/ravn/git/newspaper-fastcgi-ticket-checker/fcgid-access-checker/check.pl
	Require all granted
  </Directory>

and ensure that the appropriate module is installed and activated.

Notes:

1) Possible bottleneck in "extract uuid from memcached ressources strings".
2) Estimated initial workload around 600 requests pr second.
3) Currently supports one memcached server only.

/tra 2014-11-05

Code now works.

tests in test/
source in fcgid-access-checker/

Design note: Perl was chosen as jFastCGI did not support ACCESS
CHECKER, and Python was too much in flux for our target.  Perl 5 is
quite stable and has good modules for this.

/tra 2014-12-01

Preparing for production.

Use 
http://achernar:7880/fedora/risearch?type=triples&lang=spo&format=N-Triples&limit=&dt=on&stream=on&query=*+*+%3Cinfo%3Afedora%2Fdoms%3AContentModel_EditionPage%3E&template=
to ask DOMS for test uuids.

/tra 2014-12-09

Statistics data logging added by TRA and ABR.  Processed by
newspaper-usage-statistics.

/tra 2015-06-19