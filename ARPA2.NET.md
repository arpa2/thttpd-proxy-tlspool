ARPA2.NET extensions to thttpd
==============================

>   *This is an adapted version of thttpd.  It includes and demonstrates a few
>   principles behind the ARPA2.NET*[^1]* toolkit, which follows the Internet
>   Wide Architecture*[^2]*.*

Proxy forwarding
----------------

The thttpd webserver is efficient and secure; it is a highly usable frontend for
many services, especially those serving up static content.  But it is not the
best for running dynamic content; in that area it only supports cgi-bin, and not
even a modern implementation.

The HTTP protocol is *designed* to be stateless, meaning that no state is
carried from one HTTP request to the next.  This has resulted in infrastructure
of server-side scripts that need to rebuild their context from scratch for
every single page or query, and that store any intermediate data in a database,
from which this data must be fetched over and over again while serving
pages or parts of pages.

In reality, the way we *experience* our interaction with a web server, there
is a session that carries state from one request to the next.  In fact, there
are now HTTP extensions that permit listening in on server-side changes;
clearly, these can only exist when state is being kept on the server.

In light of this shift of focus, the Internet Wide Architecture prefers to
divert from the older web servers that treat every HTTP page as a fresh
request, and always rebuild state from scripts and databases.  We think it
is more realistic to treat a web service as an entity that tracks a session,
and maintains the state used in it.

This leads to an architecture with highly specialised daemons that happen
to serve their content over HTTP, but that are much more involved with the
data they carry around than a generic web server.  Call them Servlets or
Zope or Rails or anything else you like; the idea is that behind the facade
of generic HTTP interactions lies a dedicated application.

This leads to a web architecture where static / classic content is served
up from a plain web server, such as thttpd, and anything more dynamic is
provided by "plugins" which are really just independent web servers to
which service is forwarded.

To serve this style, we have introduced proxy forwarding into thttpd.  This
extension forwards anything dynamic to backend services, in a manner that
can be configured per path.  It follows a similar logic to that used for
cgi-bin, namely to match patterns against path names.



[^1]: <http://arpa2.net/>

[^2]: <http://internetwide.org/>
