libvmod-utils
=============

VMOD Varnish module utility

```
Function STRING hostname(PRIV_VCL)   # Return hostname
Function STRING timestamp()          # Return timestamp (%.9f)
Function REAL real(STRING, REAL)     # Convert a string into a REAL (double)
Function BOOL exists(STRING)         # Test presence of a file
```

Examples
=============
Add varnish hostname to HTTP headers sent to backend :
```
import utils;
sub vcl_pass {
	set bereq.http.X-Varnish-Server = utils.hostname();
}

sub vcl_miss {
	set bereq.http.X-Varnish-Server = utils.hostname();
}
```

Measure performance between VCL code or VMOD calls :
```
import utils;
sub vcl_recv {
        std.log("t_prerecv:" + utils.timestamp());
        (..) a lot of VCL or VMOD (..)
        std.log("t_postrecv:" + utils.timestamp());
}
```

Do an action based on the presence of a file :
```
import utils;
sub vcl_recv {
        if (std.tolower(req.url) == "/health_check") {
                if (utils.exists("/etc/varnish/healthcheck_" + server.identity)) {
                        error 443 "Service not available";
                }
                else {
                        error 440 "OK";
                }
        }
}

sub vcl_error {
        if (obj.status == 443) {
                set obj.status = 503;
                synthetic {"<body>0</body>"};
                return (deliver);
        }
        else if (obj.status == 440) {
                set obj.status = 200;
                synthetic {"<body>1</body>"};
                return (deliver);
        }
}

```


Copyright
=============
This document is licensed under BSD-2-Clause license. See LICENSE for details.

That code has been opened by (c) Thomson Reuters.
