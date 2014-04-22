dyndns-changeip
===============

Dynamic DNS updater for ChangeIP.com.  UNIX bash shell script.

This bash(1) shell script is used to check and update if necessary, 
the user's public dynamic IP address.  As given, it works with the 
service of ChangeIP (https://www.changeip.com), which offers free
dynamic addresses under a wide variety of domains.  It can be easily
modified to work with any other dynamic DNS provider who accepts update
requests in the form of a single web page request.

Several variables must be set in the script to identify your account
at ChangeIP.  The default values in the script here are examples only,
and will not work.  


HOW IT WORKS
============

The script minimizes the number of update requests at the dyndns
provider.  It queries the local WAN router's status page to get the 
public WAN IP address.  This is normally provided by DHCP from the
user's ISP.  The script knows how to parse the status homepage of a
DD-WRT router.  Other firmwares will likely require some customization.

If the script cannot extract a public WAN address from the local router,
it queries the IP address mirror service from ip.ChangeIP.Com.  This is
a very simple web server which responds with the user's public IP
address as seen by the web server.  This service can be used by anyone,
whether they are a customer of ChangeIP.Com or not.  We try not to use
this if possible in order to decrease the load on this free public
service.

The script then queries a public DNS server for the IP address
associated with this user's domain name.  The script currently uses a
Google public DNS server.

These two addresses are compared.  If they are the same, the dyndns
service is already reporting the proper address and no update is
necessary.  A command-line option to the script ("-f") will force an
update anyway.  The script also performs an update if the two addresses
don't match, or if there was an error determining the addresses.

The script reports what it did through the syslog service, and also by a
single line of output to stdout.  If run as a cron job, this line will
be picked up by cron and emailed back to the user.


WHEN TO RUN IT
==============

The script goes to some trouble to minimize its impact on the free
public resources it uses.  It makes two DNS queries for the user's
dynamic domain as a minimum.  One of those will likely be refered
upwards to the dyndns provider.  It makes no direct queries to the
dyndns provider at all, if this can be avoided.  All other references
are local to the user's own Linux host, or to the user's local WiFi
router/access point.  

This economical use of network facilities makes it safe to run this
script frequently.  The DNS minimum TTL is at least five minutes, so
it's pointless to run it more frequently than that.  I personally run
this script every fifteen minutes.  I also run it twice a day in 
"forced update" mode, just in case all the fancy detection and
comparison routines somehow fail.


LICENSING
=========

I got the forerunner of this script without encumbrance.  I pass it
along under the Apache License because of the dangers of software
patents.  I assert no patents on this software or what it does.  I offer
no warranty on this, and accept no liability for what it does to your
systems.  The full source is here; examine it for yourself and determine
whether it's something you want to run.  

