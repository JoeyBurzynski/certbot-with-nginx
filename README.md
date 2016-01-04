# Configuring Let's Encrypt with Nginx
If you want an A+ score on Qualys [SSL Labs](https://www.ssllabs.com/ssltest/index.html), then this is what you'll need to do. You won't need any familiarity with [Let's Encrypt](https://letsencrypt.org/), the ACME spec, or SSL in general, just basic Nginx configuration.

0. Install `git` if you haven't already.
1. `git clone https://github.com/letsencrypt/letsencrypt`
2. `git clone https://github.com/eustasy/letsencrypt-with-nginx`
3. `cat letsencrypt-with-nginx/.bash_aliases >> .bash_aliases`
4. Execute `./Generate.sh` (you may need to mark it as executable first with `chmod 755 Generate.sh`). As it will warn, this will take a while. Have a seat.
5. When you've gone and made something in the 15 minutes that could well take, or you've just set up a new SSH session, replace the instances of `example.com` in `nginx.conf`, `nginx.verify.conf`, and `crontab` with your actual domain name. Also take a look at the `[OPTION]`s.
6. Now we need to link in the `nginx.verify.conf`, test it, and reload: `sudo ln -s nginx.verify.conf /etc/nginx/sites-enabled/nginx.verify.conf && sudo nginx -t && sudo service nginx reload`
7. Now it's time to get your certificates with `renew-ssl -d example.com -d www.example.com` It will ask for the root password, and an email address, so hang around, it shouldn't take more than a few seconds. Sub-domains will just be `renew-ssl -d sub.example.com`
8. Now we need to link in the actual site, test it, and reload: `sudo rm /etc/nginx/sites-enabled/nginx.verify.conf && sudo ln -s nginx.conf /etc/nginx/sites-enabled/nginx.conf && sudo nginx -t && sudo service nginx reload`
9. Install the edited `crontab` for automatic renewal. `cat crontab > /tmp/le-crontab; crontab -l >> /tmp/le-crontab && crontab /tmp/le-crontab`

![Screenshot from 2015-11-05 04:16:13.png](https://github.com/lewisgoddard/letsencrypt-with-nginx/raw/master/Screenshot from 2015-11-05 04:16:13.png "Screenshot from 2015-11-05 04:16:13.png")
