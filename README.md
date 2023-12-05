# over-the-wire-bandit
A write up repo for overthewire level 1-15 !

Level 0 : Taught us how to use SSH , the command bandit0@bandit.labs.overthewire.org:2220 then prompted for a password (bandit0) which allowed us to log in !

Level 1: the ls command displayed a readme file which was cat to reveal the password : NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL

Level 2: the filename was also a special character '-' hence couldnt be directly used . the full file path ./- had to be specified for cat . The password was rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi

level 3: the filename had spaces which need to be escaped as '\ ' . Hence cat space\ in\ the\ filename yielded : aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG

level 4: cd-ing into the inhere directory and using ls -a revealed the hidden file :.hidden (. before the name) . Performing cat .hidden yielded : 2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe

Level 5: cd-ing into inhere and manually checking file ./-file0(0-8) revealed that ./-file07 was of type ASCII Text and contained lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR . (note a bash script will also work , especially for a larger number of files , maybe something like this :
for f in ./inhere;do
        if $(file f) == "ASCII text"
        then
                cat $f
done
)

level 6:
