## Update report

Notes from Alexey Feb 1, 2018:
The newly added user (admin) uses the same password as our normal chisel password.
The mysql user (root) is the same password as the server (so the Digital Ocean username and password).

### Updating the OS

The update was pretty straight forward. Considering we had Ubuntu 14.04 (LTS), I followed these steps to upgrade to the next LTS version, which is 16.04:
https://www.digitalocean.com/community/tutorials/how-to-upgrade-to-ubuntu-16-04-lts

There were no issues during this process, and after reboot, the server worked as before (with the old ghost version).

### Updating Ghost

Considering that Ghost is a based on Node.js, this was the next thing that would need to be updated. The old version of Ghost (which we had installed) was using Node 0.12 and that was the version installed on our server.
I removed it and instead installed NVM (Node version manager), which allows to easily update and use multiple versions of Node on a single machine --- this would make future upgrades simpler and easier. However, later, I realized Ghost was not compatible with NVM (even after I had installed a system-wide NVM).

After removing it, I had followed this guide in installing Ghost:
https://docs.ghost.org/docs/install

At first, I tried to keep things as they were, but it didn't allow installing Ghost, thus eventually ended following the above guide to the letter. This includes creating another user account on the server (admin), and installing MySQL. Having a non-root user account is actually safer, and better. Since now if someone hacks into Ghost, they don't get access into our server. 

After some minor issues with the `ghost-cli` installation script which was giving me errors (those were fixed by clearing the cache for Yarn), we had Ghost installed.

This guide was helpful as well:
https://www.danwalker.com/running-ghost-on-a-5-digital-ocean-vps/

The next step was to bring in the content from the backup of the older Ghost. We first imported the `json` file, and then I copied the images folder into `content` folder (as mentioned in the guide).

One new thing that I added was an SSL certificate for the website, by installing Certbot, and adding a cron job to automatically renew it on a regular basis.

### Updating Slate (Ghost Theme built for CHISEL)

After copying the Slate sub-folder into the new Ghost installation, and trying to apply the theme, we got some errors due to changes in syntax. Luckily, the error message mentioned what were the errors and in which files. I had used the error messages to go into the mentioned files and changed to the new syntax (most of the changes were from `{{image}}` to `{{feature_image}}` or to `{{profile_image}}` based on context) --- see screenshot of the error message.

We had some other visual issues, but they they were actually caused during the import process of the `json` backup file. The issue was that we had empty lines within the HTML code of some of our posts. I removed the empty lines to resolve these issues.

