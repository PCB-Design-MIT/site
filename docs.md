---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: docs
description: buckle up kiddo you're about to learn what kind of _SCHENANIGANS_ go into making this whole show work.

# Author box
author:
    title: fischer's source of serotonin
    title_url: 'https://bongo.cat'
    external_url: true
    description: because he needed that while putting this all together

# Micro navigation
micro_nav: true

---

# serrr veerrrrr

Oh god welcome to the party. I've done everything that I can to make this as _Not Super Bad_ as possible, but there's a few cursed things.

The website server lives at pcb.mit.edu (which is probably how you're reading this), lives in Joe Steinmeyer's office (I think), and is a HP ProDesk 400 G1 with an i5-4690S and 8GB of RAM running Ubuntu 22.04. That means that our little adventure is hosted on this monster of precision supercomputing:

![](https://cdn3.volusion.com/jraru.wkahj/v/vspfiles/photos/200884-2.jpg?v-cache=1457011800)

The domain name configuration was done by Fletch ([fletch1@eecs.mit.edu](mailto:fletch1@eecs.mit.edu)) at Joe's request. Fletch is EECS's internal person for doing some IT things outside of IS&T - I think he lives under something called the Educational Computing Facility, but I don't have permissions to view that page on eecs.mit.edu, so you probably don't either.

As for the site, it's generated using [Jekyll](https://jekyllrb.com/) (if you've used github pages before, same stuff) which turns Markdown files stored in a Git repo into HTML and CSS, which are then served by [nginx](https://www.nginx.com/), which is a HTTP server.

All the website source lives in `/home/git/site` - which notably implies that there's a user named `git`. This is fairly common practice (if you clone something over SSH from GitHub, you'll notice that you clone `git@github.com:<user>/<project>.git` - the `git@` part shows that you're actually logging in as a user named `git`). The site is automatically regenerated whenever there's a push to the `main` branch of the repo, this is done with something called a _post-commit hook_, which is just a shell script that git executes after the repo's been pushed to. The setup process for this is described in the [Jekyll Docs](https://jekyllrb.com/docs/deployment/automated/), and all it does is:

- clones the repo in a temp folder in `/tmp`
- uses Jekyll to build the site, and puts the output files in `/var/www/pcb`, which is where nginx has been configured to look for HTML/CSS to serve over HTTP, which makes the site. It's worth noting that the 🔥 __entire__ 🔥`/var/www/pcb` directory is nuked and rebuilt when you do this. This means that there's nuance to how we serve large static files (videos, large images, .pdfs of project writeups, etc), but more on that in a second.

Speaking of nginx, the setup is pretty simple. You just install it, and then update `/etc/nginx/sites-enabled/default` to let nginx know that requests to pcb.mit.edu should be routed to files inside `/var/www/pcb`. You'll also notice some stuff in there that's managed by Let's Encrypt, no real reason to touch that. Most importantly in this file you'll notice that there's a few aliases for other places on the server. This causes nginx to redirect your request if you try to hit something that's been aliased - for instance, if you try to hit anything under `pcb.mit.edu/static/videos`, instead of taking you to files stored in `/var/www/pcb`, it'll take you to files stored in `/var/www/videos`, which is a folder I made to, well, store videos. Here's a backup of the file to show you what this looks like:

```
server {
    root /var/www/pcb;
    index index.html index.htm index.nginx-debian.html;
    server_name pcb.mit.edu;
    location / {
        # First attempt to serve request as file, then
        # as directory, then fall back to displaying a 404.
        try_files $uri $uri/ =404;
        autoindex on;
        disable_symlinks off;
    }

    error_page 404 /404.html;
    location = /404.html {
            internal;
    }

    location /videos/ {
            alias /var/www/videos/;
    }

    location /slides/ {
            alias /var/www/slides/;
    }

    location /media/ {
            alias /var/www/media/;
    }


    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/pcb.mit.edu/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/pcb.mit.edu/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}

server {
    if ($host = pcb.mit.edu) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    server_name pcb.mit.edu;
    listen 80;
    return 404; # managed by Certbot


}
```

This means that the `/var/www/pcb` folder can store the files that make up the site itself (HTML and CSS) and be often nuked and remade, but the large static files just stay in the same place because they don't need to be rebuilt. This means that they stay out of the Git repo (version-controlling large files, especially non-text files, makes git very unhappy) and don't have to be copied across the disk all the time (from `/home/git/site/` to `/var/www/pcb`). And for a potato-quality server that's got a 100GB hard drive in it, that makes it so that the website doesn't take an hour to build. And also makes sure we're not duplicating files on disk.

<div class="callout callout--danger">
    Be careful here - none of the large static files are backed up (yet), so if you delete them somehow, there's no way to get them back. Superuser permissions are needed while working in this directory - this is why.
</div>

All this gets you an HTTP site. To get SSL certificates installed you'll need to use [certbot](https://certbot.eff.org/) to get Let's Encrypt to issue you a certificate, which you can do using the instructions on the [certbot docs](https://certbot.eff.org/instructions?ws=nginx&os=ubuntufocal). This is actually pretty straightforward, only annoying thing is that certbot runs as a Snap, which requires you to have `snapd` installed.

There's not too much else on the server - I did install [fzf](https://github.com/junegunn/fzf), which lets you search your bash history really easily with Control+R. The keyboard shortcuts to get it working are activated by some magic in your `.bashrc`, so feel free to copy the last few lines of mine (under `/home/fischerm/.bashrc`) into yours if you'd like.

# Al Teee Uhmmm

- Licenses: students use educational licenses, we use Pro licenses that were Bequeathed™ to us by Rea Callender ([rea.callender@altium.com](rea.callender@altium.com)), who's the VP of education at Altium. We used these Pro licenses to make the Altium 365 workspace that we've been using for the class.

- VMs: fischer struggled with these for a long time. We use something that IS&T calls a [Virtual Computer Lab](http://kb.mit.edu/confluence/display/istcontrib/Horizon+Virtual+Desktops+and+Computer+Labs+Landing+Page), which lives at [vdi.mit.edu](https://vdi.mit.edu). This is a reconfigurable pool of VMs, which can get assigned to kerberoses. In the fall of 2022 I took a clean Windows 10 machine, cleaned it up, and installed Altium on it. This image was called `PCB-FAB`
, and a copy was spun up for every person in the class, regardless of if they were on Windows or not. At the time we thought this would alleviate some logistical headaches on our end - we wouldn't need to figure out who needs a VM and who doesn't - but I think the VMs run on shared resources and this made the VMs slower than they could have been if we hadn't had a bunch of unnecessary machines running. We struggled with getting these online since our ticket to get the VMs provisioned wasn't answered for like a week, but we eventually found the right contact in IS&T who's been super responsive on Slack and it's been all smooth sailing since then. His name's John Doherty, ([jdoherty@mit.edu](mailto:jdoherty@mit.edu)). We also struggled a bit with getting IS&T to listen to us in the first place since we're not instructors, but Joe Stienmeyer [jodalyst@mit.edu](mailto:jodalyst@mit.edu) was happy to throw his name on things and get us shuffled through.

Rea's pretty quick about responding to email (usually ~1 day) and if you got questions, it's probably best to just ask him - he's pretty well connected and threw a bunch of connections at us over our interactions.

i think we should heavily consider using KiCad next year, but that's just me.

# lecture recordings

Originally we thought we were going to be using IS&T's [lightweight lecture capture](https://ist.mit.edu/lecture-capture) thing, but we were assigned to be in 34-101, which doesn't support it. That room's got it's own dedicated AV booth and a weird setup, so that falls under the domain of MIT AV, not IS&T. We set this up in like an hour before the first lecture by calling MIT AV, and getting hold of a guy named Mark who completely saved our bacon by directing us to Elaine Mello ([emello@mit.edu](mailto:emello@mit.edu)) in MIT ODL who runs that particular setup. She configures the automated lecture recording and hooks it up to Panopto, which shows up in a Canvas page she made. We didn't pass that along to the students (because like why) but we distrubuted the lecture recording by hitting the share button in the top bar of Panopto, changing the video to be accessible to anyone with the link, and then popping that link onto the lecture page. We would have just downloaded it and put it on the website, but it's kinda hard to merge the video streams together.

someone came by before lecture every day with a USB-C to HDMI adapter and a lavalier microphone, and we returned those at the end of the lecture.

# piazza

anyone with an MIT email can just make a class on piazza with basically zero extra steps. it's actually pretty easy - MIT's got a site license and I think it automatically grabs that with the @mit.edu in your email.
