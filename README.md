# Playmaker

![screenshot](https://github.com/NoMore201/playmaker/raw/master/example.png)

## Table of Content

* [Description & Features](#desc)
* [Usage](#usage)
* [Alternatives](#diff)

<a name="desc"/>

## Description & Features

Playmaker is a fdroid repository manager, which lets you download/update apps from the play store using your google account
and configure repository with app you download. After you setup the server, repository will be available at the address 
`http[s]://<playmaker_host>/fdroid`, and you can start downloading apps from play store.

Server uses [googleplay-api](https://github.com/NoMore201/googleplay-api) library, which is the python equivalent of the Java [play-store-api](https://github.com/yeriomin/play-store-api) library used by YalpStore.

Features:
* Download apks from google play store to your collection. Update them or delete if they are not needed anymore.
* Manage the fdroid repository. You can update it manually, as you add/remove apks to your collection.
* Non-blocking UI, you can browse the collection or search for an app while the server is updating the fdroid
repository.
* Responsive UI, usable also from a mobile device

<a name="usage"/>

## Usage

### Requirements

Playmaker requires HTTPS to avoid mitm attacks, since it needs to send base64 encoded google credentials to the server.
You can use you own certs through environment variables (see below)
On first launch, playmaker will ask your for your google credentials. To avoid authentication problems, like captcha requests,
it's recommended to setup app specific password, and securing your account with 2-factor auth.

### Running

Since this app requires a lot of heavy dependencies, like Android SDK and fdroidserver, it is recommended to use the docker image.
You can use a pre-built image on [docker hub](https://hub.docker.com/r/nomore201/playmaker/builds/) or build by yourself using provided `Dockerfile`.
There are some environemnt variables you'll want to use:

- `GOOGLE_EMAIL` (not recommended): if you want to supply credentials directly to docker, skipping the login page
- `GOOGLE_PASSWORD` (not recommended): if you want to supply credentials directly to docker, skipping the login page
- `HTTPS_CERTFILE`: path of the https certificate file
- `HTTPS_KEYFILE`: path of the https key file
- `LANG_LOCALE`: set a specific locale. Defaults to the system one if not set
- `LANG_TIMEZONE`: set a specific timezone. Defaults to `Europe/Berlin` if not set

The docker run command will look like this:
```
docker run -d --name playmaker \
    -p 5000:5000 \
    -v /srv/fdroid:/data/fdroid \
    -e HTTPS_CERTFILE="/srv/https.crt" \
    -e HTTPS_KEYFILE="/srv/https.key" \
    -e LANG_LOCALE="it_IT" \
    -e LANG_TIMEZONE="Europe/Rome" \
    nomore201/playmaker
```

### Virtualenv

If you want to run it in a virtualenv rather than using docker, remember that you need to install fdroidserver,
android SDK and define the ANDROID_HOME env variable (see the Dockerfile as a reference).
Instruction on how to install fdroidserver [here](https://f-droid.org/docs/Installing_the_Server_and_Repo_Tools/)

<a name="diff"/>

## Alternatives

### YalpStore

[YalpStore](https://github.com/yeriomin/YalpStore) is an open source alternative to the play store. It works very well and it requires you to install only the app, but it requires one of the following thing to be able to install/update apks:

- enable **Unknown Sources**
- have **root** privileges

While if you use playmaker and the fdroid [privileged extension](https://gitlab.com/fdroid/privileged-extension), fdroid will be able to install/update app without root privileges or enabling unknown sources.
