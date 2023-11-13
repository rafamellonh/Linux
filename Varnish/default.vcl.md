Linux Configuration
Packages installed
cloog-ppl-0.15.7-1.2.el6.x86_64.rpm
cpp-4.4.7-16.el6.x86_64.rpm
gcc-4.4.7-16.el6.x86_64.rpm
glibc-2.12-1.166.el6_7.3.x86_64.rpm
glibc-common-2.12-1.166.el6_7.3.x86_64.rpm
glibc-devel-2.12-1.166.el6_7.3.x86_64.rpm
glibc-headers-2.12-1.166.el6_7.3.x86_64.rpm
jemalloc-3.6.0-1.el6.x86_64.rpm
kernel-headers-2.6.32-573.8.1.el6.x86_64.rpm
libgcc-4.4.7-16.el6.x86_64.rpm
libgomp-4.4.7-16.el6.x86_64.rpm
mpfr-2.4.1-6.el6.x86_64.rpm
ppl-0.10.2-11.el6.x86_64.rpm
varnish-4.0.3-1.el6.x86_64.rpm
varnish-libs-4.0.3-1.el6.x86_64.rpm
GeoIP-1.6.5-1.el6.x86_64.rpm
GeoIP-GeoLite-data-2015.04-2.el6.noarch.rpm
GeoIP-GeoLite-data-extra-2015.04-2.el6.noarch.rpm
geoipupdate-2.2.1-2.el6.x86_64.rpm
nginx-1.0.15-12.el6.x86_64.rpm
nginx-filesystem-1.0.15-12.el6.noarch.rpm
openssl-1.0.1e-42.el6_7.1.x86_64.rpm 
Additions to /etc/hosts
142.101.177.151 whml13046
142.101.177.150 whml13049
142.101.177.168 whml12788
142.101.177.171 whml12791
 

Varnish configuration
/etc/sysconfig/varnish
# Configuration file for varnish
#
# /etc/init.d/varnish expects the variable $DAEMON_OPTS to be set from this
# shell script fragment.
#

# Maximum number of open files (for ulimit -n)
NFILES=131072

# Locked shared memory (for ulimit -l)
# Default log size is 82MB + header
MEMLOCK=82000

# Maximum number of threads (for ulimit -u)
NPROCS="unlimited"

# Maximum size of corefile (for ulimit -c). Default in Fedora is 0
# DAEMON_COREFILE_LIMIT="unlimited"

# Set this to 1 to make init script reload try to switch vcl without restart.
# To make this work, you need to set the following variables
# explicit: VARNISH_VCL_CONF, VARNISH_ADMIN_LISTEN_ADDRESS,
# VARNISH_ADMIN_LISTEN_PORT, VARNISH_SECRET_FILE, or in short,
# use Alternative 3, Advanced configuration, below
RELOAD_VCL=1

# This file contains 4 alternatives, please use only one.

## Alternative 1, Minimal configuration, no VCL
#
# Listen on port 6081, administration on localhost:6082, and forward to
# content server on localhost:8080.  Use a fixed-size cache file.
#
#DAEMON_OPTS="-a :6081 \
#             -T localhost:6082 \
#             -b localhost:8080 \
#             -u varnish -g varnish \
#             -s file,/var/lib/varnish/varnish_storage.bin,1G"


## Alternative 2, Configuration with VCL
#
# Listen on port 6081, administration on localhost:6082, and forward to
# one content server selected by the vcl file, based on the request.  Use a
# fixed-size cache file.
#
#DAEMON_OPTS="-a :6081 \
#             -T localhost:6082 \
#             -f /etc/varnish/default.vcl \
#             -u varnish -g varnish \
#             -S /etc/varnish/secret \
#             -s file,/var/lib/varnish/varnish_storage.bin,1G"


## Alternative 3, Advanced configuration
#
# See varnishd(1) for more information.
#
# # Main configuration file. You probably want to change it :)
VARNISH_VCL_CONF=/etc/varnish/default.vcl
#
# # Default address and port to bind to
# # Blank address means all IPv4 and IPv6 interfaces, otherwise specify
# # a host name, an IPv4 dotted quad, or an IPv6 address in brackets.
VARNISH_LISTEN_ADDRESS=0.0.0.0
VARNISH_LISTEN_PORT=80
#
# # Telnet admin interface listen address and port
VARNISH_ADMIN_LISTEN_ADDRESS=0.0.0.0
VARNISH_ADMIN_LISTEN_PORT=6082
#
# # Shared secret file for admin interface
VARNISH_SECRET_FILE=/etc/varnish/secret
#
# # The minimum number of worker threads to start
VARNISH_MIN_THREADS=50
#
# # The Maximum number of worker threads to start
VARNISH_MAX_THREADS=1000
#
# # Idle timeout for worker threads
VARNISH_THREAD_TIMEOUT=120
#
# # Cache file size: in bytes, optionally using k / M / G / T suffix,
# # or in percentage of available disk space using the % suffix.
VARNISH_STORAGE_SIZE=256M
#
# # Backend storage specification
VARNISH_STORAGE="malloc,${VARNISH_STORAGE_SIZE}"
#
# # Default TTL used when the backend does not specify one
VARNISH_TTL=120
#
# # DAEMON_OPTS is used by the init script.  If you add or remove options, make
# # sure you update this section, too.
DAEMON_OPTS="-a ${VARNISH_LISTEN_ADDRESS}:${VARNISH_LISTEN_PORT} \
             -f ${VARNISH_VCL_CONF} \
             -T ${VARNISH_ADMIN_LISTEN_ADDRESS}:${VARNISH_ADMIN_LISTEN_PORT} \
             -t ${VARNISH_TTL} \
             -p thread_pool_min=${VARNISH_MIN_THREADS} \
             -p thread_pool_max=${VARNISH_MAX_THREADS} \
             -p thread_pool_timeout=${VARNISH_THREAD_TIMEOUT} \
             -u varnish -g varnish \
             -S ${VARNISH_SECRET_FILE} \
             -s ${VARNISH_STORAGE}"
#


## Alternative 4, Do It Yourself. See varnishd(1) for more information.
#
# DAEMON_OPTS=""

/etc/varnish/default.vcl
vcl 4.0;
# Based on: https://github.com/mattiasgeniar/varnish-4.0-configuration-templates/blob/master/default.vcl

import std;
import directors;


#######################
# BACKEND : whml13046 #
#######################
backend whml13046 {
  .host = "whml13046";    # IP or Hostname of backend
  .port = "80";           # Port Apache or whatever is listening

  .max_connections = 400;   # Maximum number of open connections towards this backend.
                            # If Varnish reaches the maximum Varnish it will start failing connections.

  .probe = {
    # Health check 1: Uses complete drupal's health check
    .url = "/robots.txt";

    # Health check 2: Lightweight way with "HEAD /"
    ##.request =
    ##  "HEAD / HTTP/1.1"
    ##  "Host: localhost"
    ##  "Connection: close"
    ##  "User-Agent: Varnish Health Probe";

    .interval  = 5s; # Check the health of each backend every 5 seconds (Default is 5s)
    .timeout   = 1s; # The timeout for the probe (Default is 2s)

    # Determine backend health state
    # In this example: if 3 (threshold) out of the last 5 (window) polls succeeded the backend is considered healthy,
    #                  otherwise it will be marked as sick. In another word, it takes 15sec minimum before a backend
    #                  will be marked as healthy
    .window    = 5;  # How many of the latest polls we examine to determine backend health (Defaults: 8)
    .threshold = 3;  # How many of the polls must have succeeded for us to consider the backend healthy (Default: 3)
  }

  .first_byte_timeout     = 60s;   # How long to wait before we receive a first byte from our backend?
  .connect_timeout        = 5s;     # How long to wait for a backend connection?
  .between_bytes_timeout  = 60s;     # How long to wait between bytes received from our backend?
}


#######################
# BACKEND : whml13049 #
#######################
backend whml13049 {
  .host = "whml13049";
  .port = "80";
  .max_connections = 400;
  .probe = {
    .url = "/robots.txt";
    .interval  = 5s;
    .timeout   = 1s;
    .window    = 5;
    .threshold = 3;
  }
  .first_byte_timeout     = 60s;   # How long to wait before we receive a first byte from our backend?
  .connect_timeout        = 5s;     # How long to wait for a backend connection?
  .between_bytes_timeout  = 60s;     # How long to wait between bytes received from our backend?
}


#######################
# BACKEND : whml12788 #
#######################
backend whml12788 {
  .host = "whml12788";
  .port = "80";
  .max_connections = 400;
  .probe = {
    .url = "/robots.txt";
    .interval  = 5s;
    .timeout   = 1s;
    .window    = 5;
    .threshold = 3;
  }
  .first_byte_timeout     = 60s;   # How long to wait before we receive a first byte from our backend?
  .connect_timeout        = 5s;     # How long to wait for a backend connection?
  .between_bytes_timeout  = 60s;     # How long to wait between bytes received from our backend?
}


#######################
# BACKEND : whml12791 #
#######################
backend whml12791 {
  .host = "whml12791";
  .port = "80";
  .max_connections = 400;
  .probe = {
    .url = "/robots.txt";
    .interval  = 5s;
    .timeout   = 1s;
    .window    = 5;
    .threshold = 3;
  }
  .first_byte_timeout     = 60s;   # How long to wait before we receive a first byte from our backend?
  .connect_timeout        = 5s;     # How long to wait for a backend connection?
  .between_bytes_timeout  = 60s;     # How long to wait between bytes received from our backend?
}

############
# VCL_INIT #
############
sub vcl_init {
  # Called when VCL is loaded, before any requests pass through it.
  # Typically used to initialize VMODs.
  new cluster_uat = directors.round_robin();
  cluster_uat.add_backend(whml13046);
  cluster_uat.add_backend(whml13049);
  cluster_uat.add_backend(whml12788);
  cluster_uat.add_backend(whml12791);
}


sub vcl_recv {
  # Called at the beginning of a request, after the complete request has been received and parsed.
  # Its purpose is to decide whether or not to serve the request, how to do it, and, if applicable,
  # which backend to use.
  # also used to modify the request
  # Respond to incoming requests (Browser -> Varnish)
  #
  # Typically you clean up the request here:
  #  - Define if we want to use X-Varnish-Debug
  #  - Define backend that will respond
  #  - Detect device (mobile or desktop)
  #  - Define X-Forwarded-For
  #  - Implement PURGE ruleset
  #  - Remove / Keep incoming cookies we don't need
  #
  #
  # Return:
  #      return (pass)   : Tell Varnish not to cache the request
  #      return (purge)  : Tell Varnish to purge item in the cache
  #      return (hash)   : Tell Varnish to deliver content from cache
  #      return (pipe)   : Tell Varnish to pipe the request to the backend
  #
  # Note:
  #      pass differs from pipe.
  #      With pipe, Pipe short circuits the client and the backend connections.
  #      Varnish will just sit there and shuffle bytes.
  #      Varnish will not look at the data being send back and forth,
  #      So your logs will be incomplete...
  set req.backend_hint = cluster_uat.backend(); # send all traffic to the vdir director

  # Forward real IP address to the backend
  # Akamai uses a different header (True-Client-IP) for X-Forwarded-For
  # https://community.akamai.com/videos/1232
  if (req.http.True-Client-IP) {
      if (req.http.X-Forwarded-For) {
          set req.http.X-Forwarded-For = req.http.X-Forwarded-For + ", " + req.http.True-Client-IP;
      } else {
          set req.http.X-Forwarded-For = req.http.True-Client-IP;
      }
  }

  # Do not allow outside access to cron.php or install.php or update.php
  if (req.url ~ "^/(cron|install|update)\.php$") {
  # Have Varnish throw the error directly.
      return (synth(404, "404 - Page not found"));
  }

  return (pipe);
}


############
# VCL_PIPE #
############
sub vcl_pipe {
  # Called upon entering pipe mode.
  # In this mode, the request is passed on to the backend, and any further data from both the client
  # and backend is passed on unaltered until either end closes the connection. Basically, Varnish will
  # degrade into a simple TCP proxy, shuffling bytes back and forth. For a connection in pipe mode,
  # no other VCL subroutine will ever get called after vcl_pipe.

  # Note that only the first request to the backend will have
  # X-Forwarded-For set.  If you use X-Forwarded-For and want to
  # have it set for all requests, make sure to have:
  # set bereq.http.connection = "close";
  # here.  It is not set by default as it might break some broken web
  # applications, like IIS with NTLM authentication.

  set bereq.http.Connection = "close";
}



#############
# VCL_SYNTH #
#############
sub vcl_synth {
  #
  # Used to generate custom HTTP response (page or redirects)
  #

  if (resp.status == 720) {
    # We use this special error status 720 to force redirects with 301 (permanent) redirects
    # To use this, call the following from anywhere in vcl_recv: return (synth(720, "http://host/new.html"));
    set resp.http.Location = resp.reason;
    set resp.status = 301;
    return (deliver);
  } elseif (resp.status == 721) {
    # And we use error status 721 to force redirects with a 302 (temporary) redirect
    # To use this, call the following from anywhere in vcl_recv: return (synth(720, "http://host/new.html"));
    set resp.http.Location = resp.reason;
    set resp.status = 302;
    return (deliver);
  } elseif (resp.status == 404) {
    set resp.status = 404;
    set resp.http.Content-Type = "text/plain; charset=utf-8";

    # Use basic message or uncomment to use static HTML template
    synthetic( {"ERROR: Page not found"} );

    # Example of how to include a static HTML template
    # synthetic(std.fileread("/usr/local/etc/varnish/vcl_404_error.html"));
    return (deliver);
  }

  return (deliver);
}




#####################
# VCL_BACKEND_ERROR #
#####################
sub vcl_backend_error {
    set beresp.http.Content-Type = "text/html; charset=utf-8";
    synthetic( {"ERROR: backend unavailable"} );
    return (deliver);
}

 

 

nginx configuration
/etc/nginx/conf.d/ssl.conf
#
# HTTPS server configuration
# Proxy everything over http to local Varnish process
#

server {
    listen       443;
    server_name  _;

    ssl                  on;
    ssl_certificate      /etc/pki/tls/certs/client.com-multi-bundle.crt;
    ssl_certificate_key  /etc/pki/tls/private/client.com-multi.key;

    ssl_session_timeout  5m;

    ssl_protocols  TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers "ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA:DES-CBC3-SHA:HIGH:!aNULL:!eNULL:!EXPORT:!DES:!MD5:!PSK:!RC4"; #Disables all weak ciphers
    ssl_prefer_server_ciphers   on;

    location / {
        proxy_pass            http://127.0.0.1:80;
        proxy_read_timeout    90;
        proxy_connect_timeout 90;
        proxy_redirect        off;

        proxy_set_header      X-Real-IP $remote_addr;
        proxy_set_header      X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header      X-Forwarded-Proto https;
        proxy_set_header      X-Forwarded-Port 443;
        proxy_set_header      Host $host;
    }
}

NOTE: remove the file /etc/nginx/conf.d/default.conf, as it would interfere with varnish on port 80.