# Yellow Canary - The HA Failure Stimulator

Yellow Canary is a script that stimulates failures on your systems to help identify weaknesses in your high availability systems. The theory is simple: our HA systems don't get enough exercise and things break in production when you least want them to, so why not trigger more failures in environments on an ongoing basis so we can find all the little gotchas that are hiding in our otherwise brilliant architecture. :)

You can even consider running running this script in production, but to help you get more sleep and spend time with your family, perhaps just schedule that as a cron job that runs from 9am-11am Tuesday to Thursday.

This script simply sends STOP or KILL signals to a list of processes (or their children) at random intervals. 

## Usage

    Usage: yellow-canary [option]... PROCESSNAME...
       
           --min-interval duration       minimum sleep duration in seconds (default 10)
           --max-interval duration       maximum sleep duration in seconds (default 1800)
           --iterations iterationcount   # of iterations to run (default no limit)
           --stop-weight percent         % of signals that are STOP (default 50)                

    Example:

    yellow-canary --min-interval 10 --max-interval 1800 --iterations 10 --stop-weight 50 java nginx apache2 docker collectd haproxy 
    

## Project Status

2015-02: This project is still an idea and code has not yet been written. Please contribute!

### Current thinking:

I'm currently thinking a simple sh script is the best language to write this in, but my sh skills are kinda lacking. Help is welcome!

- v1 could simply call killall with a name randomly selected from the list of process names.
- *ps --ppid pidlist* seems like it would be good to get a list of child processes if we want to get deeper into each process
- `date -Iseconds` for time display


Suggested stdout:

    2015-02-03T15:17:26+0000 yellow-canary: Now running; press ctrl+C or killall yellow-canary to stop
    2015-02-03T15:17:26+0000 yellow-canary: next event: KILL haproxy (8723) at 2015-02-03T15:18:26+0000 (in 60s)
    2015-02-03T15:18:21+0000 yellow-canary: next event: KILL haproxy (8723) at 2015-02-03T15:18:26+0000 (in 5s)  
    2015-02-03T15:18:26+0000 yellow-canary: sending KILL to haproxy (8723)
    2015-02-03T15:18:26+0000 yellow-canary: next event: STOP nginx (12345) at 2015-02-03T15:48:26+0000 (in 30m)
    2015-02-03T15:48:21+0000 yellow-canary: next event: STOP nginx (12345) at 2015-02-03T15:48:26+0000 (in 5s)  
    2015-02-03T15:48:26+0000 yellow-canary: sending STOP nginx (12345)
	etc.
    

### Contributions

Please contribute to this project. Some ideas:

- Write or contribute to the script
- Update this help file
- Suggest what language to write this in so it works as many places as possible, but in particular on lean systems like CoreOS.
- Suggest improvements
- Comment... is there something like this out there already? Would this be useful to you?

Any help is appreciated! Thanks!
