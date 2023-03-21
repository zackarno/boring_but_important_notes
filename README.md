# Compilation of boring but important notes

This repo contains notes for myself that I'd rather not spend time googling again. 

## Python 

- [pyenv and venv for windows instructions](https://k0nze.dev/posts/install-pyenv-venv-vscode/) and accompanying [video](https://www.youtube.com/watch?v=HTx18uyyHw8)

## R/R-studio Linux
- followed R-studio/CRAN directions for installing R
- as of 2022-09-25 Rstudio dependencies create issue Rstudio desktop installation for Ubuntu. Browsing revealed some work-arounds, but simplest option seemed to be to download directly from [daily builds for Ubuntu](https://dailies.rstudio.com/rstudio/spotted-wakerobin/desktop/jammy/) which should work out of box. This solution was [posted here](https://community.rstudio.com/t/dependency-error-when-installing-rstudio-on-ubuntu-22-04-with-libssl/135397/2). 
- I downloaded `rstudio-2022.07.3-578-amd64.deb` which worked fine
For getting packages up and running I followed this [link](https://blog.zenggyu.com/en/post/2018-01-29/installing-r-r-packages-e-g-tidyverse-and-rstudio-on-ubuntu-linux/)... but basically:
- To use `install.packages()` you need to install `r-base-dev` via terminal with  `sudo apt install r-base-dev`
- Some R packages (i.e **tidyverse**) depend on non R packages which should be installed via terminal first with: `sudo apt install libcurl4-openssl-dev libssl-dev libxml2-dev`

## QGIS 

### Linux
[Installing QGIS on Linux](https://courses.spatialthoughts.com/install-qgis-ltr.html#install-qgis-on-linux)

### Mac

Open multiple QGIS instances at once: in terminal - `open -n /Applications/QGIS.app`

## multiple GHs

### on windows
 - `ssh-keygen -t rsa -b 4096 -C "<personal file comment>" -f ~/.ssh/<personal file name>`
 - `eval $(ssh-agent -s)`
 - `ssh-add ~/.ssh/<personal file name>`
 - `clip < ~/.ssh/<personal public name (i.e .pub)`
 - go to github -> settings -> ssh key -> paste
 -  `ssh -T git@github.com` (should say successful)
 -  *Repeat above steps to make another ssh key and add it to the other GH acccount*

 - I tried editing config file in ~/.ssh/ folder as directed, but it does not yet seem to be picking the instructions correctly
 - at this stage I need to keep using `ssh-add` correct rsa file prior to pushing to the correct GH - it does not seem to automatically register the rsa/creds
 - unclear if user would auto chage -- it seems by running global config  --user.email=xxx@yyy.org this fixes ... however restarting computer also semeed to sort i out
 - first time pushing the `-u` flag seems critical (didn't work without): `git push -u origin master` 
 - `git remote set-url origin` can be used to change remote rather than removing and adding
 
 ### On Mac
 
- easier -- pretty much just follow instructions on github docs [here](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent), but basically set up the config file as in this [youtube video](https://www.youtube.com/watch?v=zBssUO_5H_A&t=324s)... *should look into keychain*
- **Important** When using `git remote set-url origin` or `git clone` you need to use ssh url and include your `Host` listed in config file within the ssh url. **Hint** leave personal/default just as github.com and you wont need to change the url. In this case will just modify when using other accounts 

### on linux

- ssh-keygen -t ed25519 -C `"youremail@domain.com"`
- Enter file in which to save the key (/home/{user}/.ssh/id_ed25519): `/home/{user}/.ssh/{custom_name}`
- eval "$(ssh-agent -s)"
- ssh-add ~/.ssh/{custom_name}  (should then say identity added)
- `cat ~/.ssh/{custom_name}.pub` copy contents to clipboard and make new key in GH setting
- test authentication with `ssh -T git@github.com`


## Git

- How to undo commit + push (from [here](https://happygitwithr.com/reset.html)) : `git reset --hard HEAD^` , then `git push -f`
- [Instructions on removing .DS_Store from git tracking](https://gist.github.com/lohenyumnam/2b127b9c3d1435dc12a33613c44e6308)

## `{targets}`
 
I've trouble shot this same problem more than once even though it's very simple. When `tar_load_everything()` gives error it's often because targets/names are no longer in pipeline. Quickest fix is `targets::tar_prune()`


## postgresl mac

- downloaded qgis
- homebrew install postgresql
- installed pgadmin 4 from dmg
- combine these
 + https://dev.to/letsbsocial1/installing-pgadmin-only-after-installing-postgresql-with-homebrew-part-2-4k44
 + https://stackoverflow.com/questions/61576670/databases-in-psql-dont-show-up-in-pgadmin4#:~:text=Try%20menu%20File%20%2D%3E%20Preferences%2C,option%20is%20at%20the%20bottom.&text=and%20then%20you%20will%20see%20your%20database%20without%20changing%20PgAdmin%20preferences.
 
 ## Mac terminal 
 - `caffeinate -i -s Rscript -e 'targets::tar_make() `

 

 

 


