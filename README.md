# over-the-wire-bandit
A write up repo for overthewire level 1-15 !

Level 0 : Taught us how to use SSH , the command bandit0@bandit.labs.overthewire.org:2220 then prompted for a password (bandit0) which allowed us to log in !

Level 0-1: the ls command displayed a readme file which was cat to reveal the password : NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL

Level 1-2: the filename was also a special character '-' hence couldnt be directly used . the full file path ./- had to be specified for cat . The password was rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi

level 2-3: the filename had spaces which need to be escaped as '\ ' . Hence cat space\ in\ the\ filename yielded : aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG

level 3-4: cd-ing into the inhere directory and using ls -a revealed the hidden file :.hidden (. before the name) . Performing cat .hidden yielded : 2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe

Level 4-5: cd-ing into inhere and manually checking file ./-file0(0-8) revealed that ./-file07 was of type ASCII Text and contained lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR . (note a bash script will also work , especially for a larger number of files , maybe something like this :
for f in ./inhere;do
        if $(file f) == "ASCII text"
        then
                cat $f
done
)

level 5-6: this command will do the trick ! : for directory in ./; do echo "$(find $directory -size 1033c ! -executable -exec file {} + | grep "ASCII text")" ; done
./ denotes pwd , ! -executable checks if NOT executable , 1033c indicated 1033 bytes , -exec file runs the file command on {} i.e. the placeholder for the file found by find command . grep then filters out lines which do not contain ASCII text , non human readable ones ! 
This yields : P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU

level 6-7: running find ./ -type f -user bandit7 -group bandit6 -size 33c after cd .. cd .. to go to the top level directory yielded z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S as the password 

level 7-8 : running cat data.txt | grep 'millionth' yielded TESKZC0XvTetK0S9xNwm25STk5iWrBvP as the password 

level 8-9 : running sort data.txt | uniq -u yielded EN632PlfYiZbn3PhVK3XOGSlNInNE00t

level 9-10 : running strings data.txt | grep '^==' yielded G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s
strings returns human readable strings and using regex and grep we can check if the line starts with (at least) two equal to signs 

level 10-11 : running base64 -d data.txt yeilds "The password is 6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM"

level 11-12 : i confess , i found it easier to just cat the file  and paste the output in cyberchef , after applying rot13 . I ended up getting The password is JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv

level 12-13 : The process was quite annoying , and i know i should have written a script for it , to check the file extensions and chenge the names and so on but it finally yielded this "wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw"
there were lot of tar compressions (tar -xf filename) and gzips (mv file file.gz followed by gunzip file.gz) and bzip2 files too (mv file file.bz2 bunzip2 file.bz2)

level 13-14 : "fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq" . We could ssh into bandit 14 using the private key , the command being ssh -i sshkey.private bandit14@localhost -p 2220

level 14-15 : performing echo "fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq" | nc localhost 30000 gave us "jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt" as the output !

level 15-16 : we needed to enter an SSL connection with our system localhost 30001 and submit our current password . This can be done like so : openssl s_client -ign_eof -connect localhost@30001
after submitting our password we were presented with "JQttfApK4SeyHwDlI9SXGR50qclOAil1" as our password 

level 16-17 : running nmap -sV localhost showed me which ports had ssl connection (although this was a messy way to do it ) . This was hard to script due to the verbose output of ssl and snce there were only two ports , i did them manually (similar to orev level) and i got the private RSA key 

level 17-18 : diff passwords.old passwords.new gave us the line which was changed in between them both 
hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg

level 18-19 : i confess i didnt get this one entirely on my own , but it turns out we needed to look at it from the POV that we aren't allowed to ssh into BASH but there are OTHER shells which we can ssh into (using -t flag) ! password was awhqfNnAbc1naukrpqDYcF95h7HoMTrC

level 19-20 : ./bandit20-do cat /etc/bandit_pass/bandit20 allowed us to read this file which only bandit20 has read permissions to 
password was VxCazJaVykI6W36BkBU0mJTCM8rR95XT

level 20-21 : Here we had to set up our own nc listener which would echo the password back to any connection , then connect through ./suconnect and get the password : NvEJF7oVjkddltPSrdKEFOllh9V1IBcq

level 21-22 : poking around in etc/cron.d i saw cronjob_bandit22.sh being run from usr/bin , catting the same showed that it was reading and storing the contents of bandit22_pass (ehich i dont have read permissions for) to /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv which i do have reading permissions for (chmod 644)
WdDozAdTM2z9DiFEQ2mGlwngMfj4EZff is the password 

level 22-23 : a similar process showed that the filename was the MD5 version of bandit23 (it will be executed as bandit23) . password is QYw0Y2aiA672PsMmh9puTQuhoz8SyR2G


