# Spacecookie Docker

[Spacecookie](https://github.com/sternenseemann/spacecookie) is a [Gopher protocol server](https://en.wikipedia.org/wiki/Gopher_(protocol)).

I like to run this behind my host's Tor:

```bash
docker network create --subnet=172.18.0.0/16 servicenet
docker build -t spacecookie .
docker run -d -v $(pwd)/gopher-content/:/srv/gopher --restart=always --net servicenet --hostname=spacecookie --ip=172.18.0.68 spacecookie
```

Sometimes when you update the files you may need to use this:

```
docker exec -t b72 bash -c "chown -R spacecookie:spacecookie /srv/gopher"
```

It's very important to set your host to the domain you'll be using for the
gopherhole (not localhost). For me it was my `.onion` address.

## Spacecookie version

This repo is setup so the latest GitHub main branch from Spacecookie is always
used. If you want to upgrade to the latest version simply rebuild the image
like this:

```
docker build --no-cache -t spacecookie .
```

## With git and burrow

Put `id_rsa.pub` to be used for your git server in the working directory...

You can push to a `git` repo that automatically builds with `burrow`...

```
# on John's computer
$ cd myproject
$ git init
$ git add .
$ git commit -m 'Initial commit'
$ git remote add origin git@172.18.0.69:/srv/git/gopherhole.git
$ git push origin master
```

https://git-scm.com/book/en/v2/Git-on-the-Server-Setting-Up-the-Server

```


At this point, the others can clone it down and push changes back up just as easily:

$ git clone git@gitserver:/srv/git/project.git
$ cd project
$ vim README
$ git commit -am 'Fix for README file'
$ git push origin master

```
