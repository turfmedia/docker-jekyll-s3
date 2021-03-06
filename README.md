Contains jekyll 3.0.1 and s3_website 2.12.2

To allow jekyll watch, filesystem notifications are visible inside the container via watcher.rb script.

### Create a docker-compose.yml

```
jekyll: &base
  image: altieres/docker-jekyll-s3
  volumes:
    - .:/usr/src/app/site
  ports:
    - "4000:4000"

site:
  <<: *base
  working_dir: /usr/src/app/site
  command: bash -c 'jekyll build; s3_website push'
```

### Setup Jekyll

#### Generate base site

```
docker-compose run site jekyll new . -f
```

#### Run your site locally

```
docker-compose up jekyll
```

- You should use *up*, not *run*
- Open your browser, pointin to your [docker-machine ip]:4000


### Setup AWS S3 website

#### Generate base config

```
docker-compose run site s3_website cfg create
```

- Update s3_website.yml with your amazon credentials.

#### Create your bucket upon your configuration

```
docker-compose run site s3_website cfg apply
```

#### Push your transpiled jekyll site whenever you need

```
docker-compose run site
```
