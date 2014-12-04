newspaper-fastcgi-ticket-checker
================================

Newspaper access ticker checker - Perl script running via fastcgi

In order to validate access to a given resource we must check that the given request corresponds
to the credentials stored in JSON format in memcached.

Ask KFC or TRA for instructions of how to manually create a ticket for testing purposes.

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

Offline usage:

	MEMCACHED_SERVERS=alhena:11211 perl .../stream-check.pl

/tra 2014-12-04