# How to Build Htaccess Files 


## Resources 

- Official Resource: https://httpd.apache.org/docs/2.4/howto/htaccess.html
- Quick Reference: https://support-acquia.force.com/s/article/360005257234-Introduction-to-htaccess-rewrite-rules


## RewriteEngine 

- To enable rewriting make sure you head your file with `RewriteEngine on` 


## RewriteCond

- A conditional used to evaluate whether a subsequent RewriteRule should be executed

- Syntax: `RewriteCond TestString CondPattern [flags]`
  - TestString - server/env variables, HTTP_HOST, REQUEST_URI, etc.
  - CondPattern - condition, usually a regex; can use a file attribute test
  - flags - optional, special actions (NC, OR)
  
- You can have several RewriteCond evaluate before a "Rule" is applied just have them sequentially next to one another 


## RewriteRule 

- A rule used to perform a rewrite 

- Syntax: `RewriteRule Pattern Substitution [flags]`
  - pattern - regex, matched against URL-path
  - substitution - string that replaces the matched URL-path
  - flags - optional, special actions (L, R)

- In most basic implementations you'll want to have all the `RewriteCond`'s evaluated and then just simply perform a redirect to effectively ommit the pattern just pass ^(.*)$

- If you're done with your hypothetical block of `RewriteCond`'s followed by a `RewriteRule` its a good idea to pass the [ L ] flag as to stop evaluating other rules

- Typically a good idea to pass in an [ R ] flag as well with corresponding 301 ( permenant ) or 302 ( temporary ) 


## Rewrite Variables 

Variable Description

%{HTTPS} Set to on if the request is served over SSL.

%{HTTP_HOST} Everything between the protocol (http:// or https://) and the request (/examplefile.txt)

%{REQUEST_URI} Everything after the server address — for example, a request to http://example.com/my/examplefile.txt sets this value as my/examplefile.txt


## Rewrite Flags 

[Flag] Description

[F]	The request is forbidden. This can be used with conditions, such as restricting access by IP address or environment to certain paths.

[L]	If the previous conditions pass this rule is executed and no more evaluation is done. Note that if the RewriteRule redirects a user to another host the L flag is forced.

[NC] Non case-sensitive matching, commonly used in a RewriteCond where the host is evaluated. For example, mysite.com is the same as MySite.com

[P] Proxy this request. This requires the mod_proxy extension to be enabled on the server. This acts like the R flag but instead of redirecting the visitor to a new URL, this URL is served as the content for the page they requested while keeping the URL in the address bar the same as requested. This is not enabled by default on Acquia Cloud. Customers who wish to use it should open a support ticket.

[PT] Passthrough causes any file path to be treated like a URI instead.

[R] Issue a redirect code with the request. By default an R flag will issue a 302 Found (Moved Temporarily) redirection, although this may be overridden by specifying the status code to accompany the redirection. Commonly this will be set as R=301 which sends a 301 "Moved Permanently" code. This is best for SEO purposes where old documents are redirected to new URLs as it will trigger the search engine to remove the previously indexed URL and replace it with the new one.
