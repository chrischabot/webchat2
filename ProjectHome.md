A fast, highly interactive, fun chat application using a javascript, comet (real time push communication), ajax (async posting of information) modern web interface, and a custom PHP based backend daemon that interfaces between the (web) frontend and the IRC backend server.

From: http://www.chabotc.com/javascript/webchat-20-first-release

The project has been released under the GPL v2 licence. In laymen terms this means you can do with it what you want, but you have to contribute your changes back to the community / the project, and you can’t just pick it up and sell it integrated into your own projects as if it was your own, integration into other projects is only allowed if you either contact me for a commercial licence, or if your project is GPL compatible too; However using it in your website, intranet, extranet, etc is fully permitted without any problems

Do be ware, since the project isn’t completely finished yet, it does mean it currently comes without to much of documentation or instalation guides, and some small bugs remain. And to get it all running you need a number of things:

IRC server (hybrid prefered). I used the Fedora Extra’s ircd-hybrid package
PHP (developed and tested with 5.2.x) Need to have socket extentions enabled. The demo version is running in a chroot’d envirioment, there’s plenty of guides out there on how to set this up if you need more information about this. Should only be a extra layer of security, since there’s no real potential for code injection, however if you are paranoid about security, this could be a good idea
A modern web browser, like IE6, IE7 or preferable Firefox
When configuring your IRC server (which is used in the backend, and guarantees infinite scalability which is tried and tested) a few things are important:

Set the “throttle\_time” to 0 or else connections would be denied if multiple people connect at the same time.

Then to allow for more then the pre configured users from one IP (standard this is set to 3 or 4 connections)
Change:
max\_clients
number\_per\_ip
max\_global
max\_local setting
cidr\_bitlen\_ipv4
cidr\_bitlen\_ipv6
number\_per\_cidr

The demo server on chabotc.nl is set to a max of 4096 connections but any number will do as long as your server can handle it.

Also change the “auth” section in your ircd.conf and comment out the following:
/**flags = need\_ident;**/

This is because all connections come from your own server, so it only causes delayes when connecting thru the web chat frontend.

Then change your network name and description to something fitting, and you should be all set! There’s a lot of other options in the ircd.conf file, but their pretty well documented in the configuration file and there’s plenty of documentation about it on the web.

After this, check out the webchat2 source, change the port number you want it to listen on (currently defaults to 2001) in chat.php, change the server list in htdocs/js/chatConnectWindow.js(!!) you can hide the selection box and make it default to your local server easily there too and change the default channel in httpServer.php (line 78).

After this you should be able to run the daemon:
# chmod +x ./chat.php
# ./chat.php

if all worked well you should be able to connect to your chat setup thru (replace yourhost.com and port number with your local values): http://www.yourhost.com:2001/

A few wishlist items that are currently high on my list of things to fix:

Configuration file for IP’s, port numbers, etc
Implement IRC /ignore, /query, /whois, /ping and /help commands in back and frontend
Implement double click on nick (in left list) = open private chat window with that person
Implement right click menu on nick (in left list) for common options like private chat, whois, etc
Few cross browser bugs remain, and smiley’s selection still needs to be made! Much in the same style as color parsing in javascript, it needs to insert the image on click, then on sending the msg rewrite the image tag to the matching smiley sequence, aka “:D” etc, translating these smilies to images again is already implemented
There’s plenty of other items that i left out of the list but those are top priority in my perspective and i’ll be working on them as time permits, but i’m very open to patches too
