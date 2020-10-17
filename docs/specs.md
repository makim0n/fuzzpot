# Fuzzpot specs

## Overview

### Summary

* 0 - nmap host (tcp / udp), just opened port first and version scan after, no nse scripts store in db;
* 1 - gowitness to screen web server and store it as a file on disk ;
* 1 - sslscan and store it in the database ;
* 1 - get info from gospider ;
* 1 - get url from gau, httpx on status code 200 js file, subjs and then gf in JS files ;
* 2 - unfurl for directory / file / parameter and append them into a wordlist (anew) ;
* 3 - ffuf for fuzzing directory and find a way to remove false positive results and store them into database ;
* 4 - ffuf for parameter discovery on endpoint / page, find a way to be effective ;
* 5 - maybe Host Header fuzz on 403 / 401 endpoint ? (__NOT SURE__);
* 5 - qsreplace found parameters with retard vulns (ssti, lfi, rce...), xss might be more difficult to auto detect ;
* 5 - check for http smuggling / crlf / graphql ;
* 5 - nuclei scan with generic templates ;
* 5 - check for jaeles (__NOT SURE__) ;
* 6 - use information stored in database to generate the pandoc report and extract in with the desired format.

#### Class

* scope management : input scope, list, burp req...
* report generation : grab info in db, generate report...
* wordlist generation : gospider, gau / httpx / subjs / gf, unfurl...
* database management : db init, schema build, update fields...
* scan : class parent de
    * scan : nmap, nuclei, jaeles
    * misc : gowitness / sslscan
    * dir_param fuzz : ffuf on dir / param
    * web vuln : test common vuln...

#### Database schema

* Scope
    * table 1 - domains
        * column 1 - id
        * column 2 - __domain__
        * column 3 - __webUrl__
        * column 3 - isup
        * column 4 - timestamp
    * table 1 - nmap
        * column 1 - id
        * column 2 - json output
        * column 3 - cmd exec
        * column 4 - _domain_
        * column 5 - timestamp
    * table 2 - screen
        * column 1 - id
        * column 2 - screen path on disk
        * column 3 - _webUrl_
        * column 5 - timestamp 
    * table 3 - sslscan
        * column 1 - id
        * column 2 - tool output
        * column 3 - _webUrl_
        * column 4 - timestamp
    * table 4 - ffuf_dir
        * column 1 - id
        * column 2 - _webUrl_
        * column 3 - __path__
        * column 4 - isEndpointFile
        * column 5 - statusCode
        * column 6 - pageSize
        * column 7 - filterSize
        * column 8 - filterStatus
    * table 5 - ffuf_param
        * column 1 - id
        * column 2 - _webUrl_
        * column 3 - _path_
        * column 4 - __params__
        * column 5 - statusCode
        * column 6 - pageSize
        * column 7 - filterSize
        * column 8 - filterStatus
    * table 6 - nuclei
        * column 1 - id
        * column 2 - _domain_
        * column 3 - cmdUsed
        * column 4 - jsonOutput
        * column 5 - timestamp
    * table 7 - webVulns
        * column 1 - id
        * column 2 - _webUrl_
        * column 3 - _path_
        * column 4 - _params_
        * column 5 - payload
        * column 6 - vulnFamily
        * column 7 - output
    * table 8 - whatweb
        * column 1 - id
        * column 2 - _webUrl_
        * column 3 - _path_
        * column 4 - cmdUsed
        * column 5 - output
        * column 6 - timestamp

### Misc

* website list
* burp request parser -> marker where to inject (as sqlmap)
* connection to custom burp collab

### Useful link

> https://github.com/KingOfBugbounty/KingOfBugBountyTips
> https://github.com/defparam/smuggler
> https://github.com/s0md3v/XSStrike