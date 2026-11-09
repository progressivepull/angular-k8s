# Dockerfile Explanation

```dockerfile
FROM node:26
```

## FROM node:26

Uses the official Node.js 26 Docker image as the base image.

This image already contains:

- Linux operating system
- Node.js runtime
- npm package manager

Think of this as the starting point for your container.

---

```dockerfile
WORKDIR /app
```

## WORKDIR /app

Sets the working directory inside the container to `/app`.

If the directory does not exist, Docker creates it automatically.

Equivalent Linux commands:

```bash
mkdir -p /app
cd /app
```

All subsequent commands run from this directory.

---

```dockerfile
COPY package*.json ./
```

## COPY package*.json ./

Copies the following files from your project into the container:

- `package.json`
- `package-lock.json` (if it exists)

---

```dockerfile
RUN npm install
```

## RUN npm install

Installs all dependencies defined in `package.json`.

Creates a `node_modules` folder inside the container.

### Why is this done before copying the source code?

Docker caches image layers. If only application files change, Docker can reuse the dependency installation layer, making builds much faster.

---

```dockerfile
COPY . .
```

## COPY . .

Copies the entire project into the container.

Meaning:

- First `.` = current local project directory
- Second `.` = current container directory (`/app`)

---

```dockerfile
EXPOSE 4200
```

## EXPOSE 4200

Documents that the application listens on port **4200**.

This is commonly used by Angular development servers.

### Important

`EXPOSE` does **not** publish the port.

You still need:

```bash
docker run -p 4200:4200 my-app
```

---

```dockerfile
CMD ["npm", "start", "--", "--host", "0.0.0.0"]
```

## CMD ["npm", "start", "--", "--host", "0.0.0.0"]

| Argument | Explanation |
| --- | --- |
| CMD | Docker instruction that specifies the default command to run when the container starts. |
| npm | Node.js package manager. Executes npm commands. |
| start | Runs the start script defined in package.json. |
| -- | Separates npm arguments from arguments passed to the application. |
| --host | Angular CLI option that specifies which network interface the development server listens on. |
| 0.0.0.0 | Makes the server listen on all available network interfaces, allowing access from outside the container. |

Defines the default command executed when the container starts.

Docker runs:

```bash
npm start -- --host 0.0.0.0
```

For Angular projects, this typically starts:

```bash
ng serve --host 0.0.0.0
```

### Why use `0.0.0.0`?

By default, Angular often listens only on `localhost` inside the container.

Using `--host 0.0.0.0` allows the application to accept connections from outside the container through Docker port mapping.

---

# Complete Build Process

### Build the image

```bash
docker build -t angular-app .
```

### Run the container

```bash
docker run -p 4200:4200 angular-app
```

### Open the application

```text
http://localhost:4200
```

## Summary

1. Start from a Node.js image.
2. Set `/app` as the working directory.
3. Copy dependency files.
4. Install dependencies.
5. Copy application source code.
6. Document port 4200.
7. Start the Angular development server and make it accessible outside the container.
