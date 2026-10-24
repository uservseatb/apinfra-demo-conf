# apinfra-demo-conf

This repository contains the **default boilerplate configuration files** pulled by the `apinfra` agent right after installation. They demonstrate a working GitOps deployment cycle out of the box.

## What is deployed?

Instead of running anything hidden, this configuration tells your server to deploy a completely transparent, minimal hello-world-style application: **[apinfra-demo](https://github.com/uservseatb/apinfra-demo)**.

* **The Demo App:** A single-file web server written in Go that serves a lightweight, static `index.html` welcome page.
* **Resource Footprint:** Virtually zero RAM and CPU overhead.
* **Security:** The image runs in an isolated Docker network behind your host's Nginx reverse proxy. No host ports are exposed.

## Configuration Structure

The repository structure follows the standard `apinfra` environment layout:

```text
dev/
  ├── settings.yml               # Wires the demo app to port 3000 (Plain HTTP mode)
  └── demo/           
        └── docker-compose.yml   # Pulls the apinfra-demo Docker container
```

### `settings.yml` snippet used here:
```yaml
version: "1.0"
projects:
  - name: demo
    enabled: true
    proxy:
      inbound:
        port: 3000                           # Exposes the welcome page at http://<SERVER_IP>:3000
      target:
        service-name: web                    # Matches the service in docker-compose.yml
        port: 80                             # Internal container port
    config:
      cert-issue-type: none                  # Port routing uses plain HTTP by default
```

## How to use your own configuration

This repository is public and read-only for demo purposes. Once you verify that your server serves the default welcome page at `http://<YOUR_SERVER_IP>:3000`, you should replace this configuration with your own application configs:

1. Create your own public or private Git repository.
2. Mirror the directory structure found in the `dev/` folder here.
3. Update the `repo` string inside `/home/ap/.apinfra` on your target server to point to your new repository.

For a full reference on configuration schemas and deployment rules, check out the **[Connecting Your Own Project (Server Configuration)](https://github.com/uservseatb/apinfra#connecting-your-own-project-server-configuration)** section in the core apinfra repository.


## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.
