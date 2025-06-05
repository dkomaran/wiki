Summarized and further minimized (not ideal, but quickest):

1. Create a directory for you local repository, e.g. /home/user/repo.
2. Move the RPMs into that directory.
3. Fix some ownership and filesystem permissions:
     `chown -R root.root /home/user/repo`
4. Install the createrepo package if not installed yet, and run  
    `createrepo /home/user/repo`  
    `chmod -R o-w+r /home/user/repo
5. Create a repository configuration file, e.g. /etc/yum.repos.d/myrepo.repo containing  
    `[local]`  
    `name=My Awesome Repo`  
    `baseurl=file:///home/user/repo`  
    `enabled=1`  
    `gpgcheck=0`
6. Install your package using  
    `yum install packagename`

**yum --noplugins update glibc**

     If you get a python error
