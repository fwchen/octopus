<img src="https://s3.ax1x.com/2021/02/20/y5c1eA.png" alt="y5c1eA.png" border="0" height="120" align="right" />

# Octopus

_Simple, scalable project management system._

![Build](https://img.shields.io/badge/build-passing-brightgreen)
![Coverage](https://img.shields.io/badge/coverage-90%25-green)
![Quality](https://img.shields.io/badge/code%20quality-A-brightgreen)
![License](https://img.shields.io/badge/license-MIT-green)
![Last Commit](https://img.shields.io/github/last-commit/fwchen/octopus)


[![Discuss on Github](https://img.shields.io/badge/discuss%20on-GitHub-orange)](https://github.com/fwchen/mobx/discussions)

---

## Hosting Octopus on Your Own Server
If you are looking to evaluate using Octopus, the fastest way is our hosted version. If you are looking to develop Octopus, read our Github README.

To host Octopus yourself, you need:

- A MySQL database.
- A MongoDB database.
- A Linux server or platform as a service that supports Docker
- A domain with SSL/TLS. Let’s Encrypt works great.
We suggest using either Docker Compose, DigitalOcean App Platform.

To learn more about updating Octopus, visit our [update](./docs/update.md) guide.

### Run on any Linux server with Docker
This method assumes you know how to set up DNS records, install Linux packages, and configure nginx.

- Add DNS record pointing to machine and ensure ports 80 and 443 are open. For example octopus.yourcompany.com
- Install Docker and Docker Compose.
- Install MySQL locally or use a service like Amazon RDS.
- Copy docker-compose.yml to your server.
- Edit the docker-compose.yml and optionally edit the environmental variables listed in it, documented below under “Configurations”. You must at least set the database url and SMTP server.
- Run `docker-compose up -d` to start Octopus.
- Install nginx or your favorite web server or load balancer.
- Configure virtual hosts in nginx. Use proxy_pass to forward requests to the Docker based service.
- Run the django migrations (do this on every upgrade). In the container run ./manage.py migrate. If using docker-compose, you can run docker-compose run --rm web ./manage.py migrate.
- Install certbot to set up SSL. This is required.

Example nginx configuration in /etc/nginx/sites-enabled/default BEFORE running certbot.

``` nginx
server { 
        server_name mydomain.com; 
 
        location / { 
          proxy_pass http://127.0.0.1:8000; 
          proxy_set_header Host $host; 
        } 
} 
```
- Finally just go to mydomain.com and you should see the frontend.



## Contributing to Octopus
This page is intended to give new contributors an overview of Octopus.

Octopus is comprised of several Git repos, stored in my Github. You can find more specific and complete documentation in each project, but here is a summary of the key repositories that make Passit work:

- [octopus-core](https://github.com/fwchen/octopus-core.git) - Java/SpringBoot - This provides an API and basic storage services for user data. The backend does no crypto, except in unit tests. It does provide basic group access control list features. Even if we didn’t encrypt user data, the backend would provide a traditional server that ensures users are only allowed to create, edit, and view what they should have access to.
- [octopus-web](https://github.com/fwchen/octopus-web.git) - TypeScript, React, and ngrx/store (Redux) - This project is our user interface. It powers our web pages.
- [octopus-gateway](https://github.com/fwchen/octopus-core.git) - Java/SprintBoot/WebFlux


## Reporting issues
If you have a bug or feature request to report - you may report it gitlab project where the issue is found. If the issue involves multiple parts or you don’t know where it belongs - you may report it to the Passit meta repo.

Making a good bug report
Include steps to reproduce the issue, what you expected to happen, and what happened instead.

Making a good merge request
Passit has pretty good test coverage. When you make a change or fix a bug, you should add or modify a unit test to show how your change works. For example if you fixed a bug - the unit test should fail before your patch and pass after it.

### What if I’m not a programmer?
If you want to work on Passit but nothing above sounds interesting or accessible to you, that’s okay! There are ways you could get involved:

#### Security auditing
Auditing/penetration testing is really great for us; the more we get, the more secure and reputable we will be.

#### Design
We’d be interested in help with regards to UX design in particular. If you have a good sense of what makes a web app work well and you’re good at expressing those ideas through wireframing, we’d be interested in your help.

#### Marketing
If you would like to be a public advocate for Passit, get in touch with us and we’ll see what we can work out.

#### QA
If you’d like to help us test future releases, all you’d need is the ability to access the Internet and a willingness to meticulously work through a spreadsheet.

