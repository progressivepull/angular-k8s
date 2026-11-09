# angular-k8s

## Project Structure

<img src="media/image1.png" style="width:4.5in;height:4.0in" />

## Build Container

```bash
docker-compose build
```
or

```bash
docker compose build
```

## Start Application
```bash
docker compose up
```
or detached mode:

```bash
docker compose up -d
```
## Access Angular

Open:

[http://localhost:4200](http://localhost:4200)

You should see the Angular application running from Docker.

## Verify Container

List running containers:

```bash
docker ps
```
Expected output:

<img src="media/image2.png" style="width:8.5in;height:2.5in" />

## Table of Contents
* [Dockerfile Explanation](./Dockerfile_Explanation.md)