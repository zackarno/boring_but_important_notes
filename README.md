# Compilation of boring but important notes

This repo contains notes for myself that I'd rather not spend time googling again. 

## Python 

- [pyenv and venv for windows instructions](https://k0nze.dev/posts/install-pyenv-venv-vscode/) and accompanying [video](https://www.youtube.com/watch?v=HTx18uyyHw8)
- [new pyenv virtual env method ](https://realpython.com/intro-to-pyenv/)
- gotcha... need to `pip install -e.` in order to source local modules. They also need a `__init__.py` file in them and sub-directories
### pyenv for env & python
- clone or create new repo
- You can check if you already have the python version required by running `pyenv version`. If not you need to install with `pyenv install x.y.z` you can see all the available versions with `pyenv install --list`. I had the issue where `pyenv install --list` did not show the required version. I followed the 2nd advice in this [GH Issue](https://github.com/pyenv/pyenv-update) first to `brew upgrade pyenv` since I installed it w/ brew. However, this didn't work. Therefore I followed the first advice to get `pyenv update` to work by following [instruction to clone pyenv update](https://github.com/pyenv/pyenv-update) and then ran `pyenv update` which worked.
- Interestingly the command below will then control both the python version and your virtual env. Don't try to set the local python version with `pyenv local x.y.z`
- open in vscode `pyenv virtualenv 3.x.x name-of-repo` this will create a pyenv managed virtual environment inside the pyenv directory (not repo directory)
- however from an project you can just call `pyenv activate name-of-venv` to activateit
- for jupyter you will need to to
    1. `pip install ipykernel` in correct activated env
    2. `python -m ipykernel install --user --name name-of-env --display-name "name-of-env"`. Example: `python -m ipykernel install --user --name ds-raster-pipeline
s --display-name "ds-raster-pipelines"`
    4. Then when you open jupyter/jupyter lab the kernel will be recognized....**Note** you can have just one version of jupyter installed in you base pyenv version and when you open it you will be able to access the kernels after following steps 1 & 2. I only realized I hadn't done this so I had to run:
         a. `pyenv deactivate` - > `pyenv which python` (to make sure i was in pyenv/3.11.4 but not project specific venv) -> `pip install jupyterlab`

**update 2024-12-10:**  So somehow even if u create a pyenv env, activate it, use the ipykernel command line code above, sometimes it doesn't get properly recognized. It's helpful to check the kernel json to see where the kernel is pointing. In one case it was pointing at correct environment inside the base python `.pyenv/versions/3.12.4/envs/name-env/bin/python rather` than `.pyenv/versions/name-env/bin/python` (which is the shortcut).  This actually was caused the kernel not to work properly. So i manually adjusted teh json to the shortcut version. Anyways, a good troubleshooting method is to open the jupyter notebook, try to activate kernel and run:

```
!which pip
!pyenv which pip
!which python
```
 `
At some point something got funky with my pyenv virtualenv plugin. Whenever i tried to create a new virtual env  (i.e `pyenv vitualenv 3.12.4 abasdfa`) ... it woudl say something like `pyenv version note found -q flag`. The environment would be there after though when i run pyenv versions. If I activated the env and wrote `which python` or `python --version` it would say the wrong version however. Then if i installed ipykernel and followed instructions to create a kernel the kernel would have the wrong python version in `json` file. After investigating it seemed virtualenv was not really in the .pyenv/plugins.. directory. So i just decided to reinstall the pluging again 

Along the way i deleted all my venvs by accident :-) learning that:


pyenv uninstall xyz-envname seems to uninstall one of it's locations in the `pyenv versions` output, but not the other... not sure why .. to get rid of this you can do :

```
cd ~/.pyenv/versions/3.12.4/envs/
rm -rf xyz-envname
```
which will delete the whoel thing. I did this to the other python envs which i thought were just wastefull replicated here, woops deleted the environments enitrely.. i guess just a better method than pyenv install.

end of the day to update `pyenv` and `pyenv virtualenv` I went with: 

```
cd ~/.pyenv
git pull
```

Then tried chunk below, but folder wasn't there
```
cd ~/.pyenv/plugins/pyenv-virtualenv
git pull
```

so went with 

```
git clone https://github.com/pyenv/pyenv-virtualenv.git ~/.pyenv/plugins/pyenv-virtualenv
exec "$SHELL"


export PATH="$HOME/.pyenv/bin:$PATH"
eval "$(pyenv init --path)"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"

exec "$SHELL"

```

Another day: After creatinga new pyenv with
```
pyenv virtualenv 3.12.4 name-of-repo
```

i then run :

```
pyenv activate name-of-repo
which python
#>3.11.4
python --version
#> 3.11.4
```

I have to run :
```
source ~/.zshrc
```
and tehn it recognizes correct version

### VS CODE

getting quarto to work reasonably...
- get quarto extension
- running terminal `quarto` commands.. i.e `quarto preview` doesn't work... says "command not found"
- eventually added: to preferences "quarto.path": "/Applications/RStudio.app/Contents/Resources/app/quarto/bin/quarto"
- eventually added to .zschrc:
```
echo 'export PATH="/Applications/RStudio.app/Contents/Resources/app/quarto/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```
- lots of restarting terminal... top right next to scripts  there is a preview button that will eventually work after tinkering with the above

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

- Open multiple QGIS instances at once: in terminal - `open -n /Applications/QGIS.app`
- I updated QGIS and non of the layers in my local postgres db would load. Running `brew upgrade abseil` seemed to have resolved it! will report back if it broke other processes/apps

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
- **Important** When using `git remote set-url origin` or `git clone` you need to use ssh url and include your `Host` listed in config file (12:20 in video) within the ssh url. **Hint** leave personal/default just as github.com and you wont need to change the url. In this case will just modify when using other accounts 

### on linux

- ssh-keygen -t ed25519 -C `"youremail@domain.com"`
- Enter file in which to save the key (/home/{user}/.ssh/id_ed25519): `/home/{user}/.ssh/{custom_name}`
- eval "$(ssh-agent -s)"
- ssh-add ~/.ssh/{custom_name}  (should then say identity added)
- `cat ~/.ssh/{custom_name}.pub` copy contents to clipboard and make new key in GH setting
- test authentication with `ssh -T git@github.com`


## Git

- How to undo commit + push (from [here](https://happygitwithr.com/reset.html)) : `git reset --hard HEAD^` , then `git push -f`
 + `--hard` will restore to state of last commit
 + `--soft` will restore workind directory to state just before last commit (useful if you want to fix staging/commit messages)
 + to just unstage changes run `git reset HEAD` -- won't remove from directory just staging
- [Instructions on removing .DS_Store from git tracking](https://gist.github.com/lohenyumnam/2b127b9c3d1435dc12a33613c44e6308)
-  to quickly amend previous commit w/ new change (Note danger in doing this if actively collaborating): `git commit --amend --no-edit` -> `git push -f`

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
 - `caffeinate -i -s Rscript -e 'targets::tar_make()'`
 - `caffeinate -i -s Rscript run.R`
 
 Sometimes when running a background process in terminal that require a lot of files open in memory can run into error. In this case 
 - `ulimit -n 100000` # 100000 is just an example and likely way more than enough, probably smart to adjust to less.

 - After updatin R to silicon arm64 version I could now longer run R out of the terminal (i.e `caffeinate -i -s Rscript -e 'targets::tar_make()'`) . This seemed to be b/c it was not correctly identifying the `.libPaths()` . I messed around with `.zshrc` quite a bit, but eventually only solution was just `brew uninstall r` to get rid of the homebrew version. At the time I updated to arm version it was not on hombrew.
 
 
 ## Force RGB
 - [force RGB on m1/m2](https://gist.github.com/GetVladimir/c89a26df1806001543bef4c8d90cc2f8)

## Logitech
1+fn+F13 (should be able to find mac forum add link here)

## Shiny
- `browser()` does not work interactively to debug `flex_dashboard()` instead use `rmarkdown::run("my-dashboard.Rmd")` [GH issue](https://github.com/rstudio/flexdashboard/issues/115)

 

 

 


