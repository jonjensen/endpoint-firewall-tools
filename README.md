# End Point Firewall Tools

© End Point Corporation  
Licensed under the GNU GPL version 3 or any later version published by the Free Software Foundation

These are scripts that we have used for many years at [End Point](https://www.endpointdev.com/) to allow
web-based temporary firewall whitelisting for users at otherwise blocked IP addresses.

The programs are currently "single stack" and support only IPv4, since
supporting both IPv4 and IPv6 addresses for a single visitor is more
complicated to implement, support, and troubleshoot.

Network-based access control like this is not intended as a primary
security feature, but rather as an additional "moat" in front of already
well-protected servers and applications, to deny access to bots and
scanners and DoS attacks so that there is less load and noise in logs.

## firewall-pf-flushtables

Trivial script to purge OpenBSD pf address tables. We've used this in a cron
job to purge temporary whitelisting every day, or week, for example.

## firewall-whitelisting-failcount

This script manages the database of failed logins by user name, and lets you
clear counters.

## firewall-whitelisting-local-cgi

This is the original, simple firewall whitelisting script that runs under
suidperl. Set its permissions like this:

```sh
chown root:root $file
chmod u=srx,go= $file
```

It is invoked by Apache as a CGI program, and counts on you setting Apache up
with HTTP basic authentication to protect it.

## firewall-whitelisting-remote-cgi

This is a more feature-rich firewall whitelisting script that runs under
plain perl from within a sudo wrapper.

It also expects to be invoked by Apache as a CGI program, but has its own
user and group authentication. It also adds per-username login failure
counters, and can ssh to remote systems to adjust their firewall rules too.

## firewall-whitelisting-aws-cgi

This is based on firewall-whitelisting-remote-cgi but instead of using ssh
to a host-based firewall, it uses the Amazon API to add IP addresses to
a particular AWS security group.

## firewall-whitelisting-sudo-wrapper

OpenBSD does not provide suidperl, and newer versions of Perl don't provide
it any more at all, so this script is a simple sudo wrapper that will
run the CGI program with root privileges it needs (if you configure your
/etc/sudoers appropriately).
