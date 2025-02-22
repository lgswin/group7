# INFO8985-observable-monolith
Observable Wordpress

```bash
npm run postinstall
docker compose up -d
```

This brings up signoz on port 3301 in a codespace. It also brings up wordpress on port 8000. Wordpress is instrumented to work with otel.

I also used this for a demo I was doing. To do this, I added an nginx proxy in the nginx folder and added the container with `- ./nginx/docker-compose.yml` in the `docker-compose.yml` file.

## Assignment

A provider of a public service mesh needs this instrumented for billing. Please find a way to set the OTEL_SERVICE_NAME in `wordpress/docker-compose.yml` to have your repository name and github name in it. Please find a metric that can be used to determine the total compute time for this service.

## Marking

This is a group project.

|Item|Out Of|
|--|--:|
|get wordpress up and running from the wp-content folder|2|
|agree on an idea for your site and each team-member add 1 post|2|
|deploy on gh-pages|2|
|change the OTEL_SERVICE_NAME as in the requirements|2|
|identify a metric that can be used for the total compute time for your wordpress container|2|
|||
|total|10|

* Hints:
1) There is a workflow in the .github folder to deploy on gh-pages. You need to set the folder name of your project in the package.json file, and update the pages settings so that it deploys properly. You can use the script that sets the sitename for that.
2) you can get your sitename by doing `pwd` and manipulating that. It is also in `.git/config`.

I hope that this is fun!