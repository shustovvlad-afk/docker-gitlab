# Local GitLab

Run GitLab CE locally with Docker Compose.

```sh
docker compose up -d
```

GitLab can take several minutes to become ready on the first start.

This compose file uses reduced local settings for a single-user GitLab:
one Puma process, lower Sidekiq concurrency, and no bundled Prometheus, KAS,
or container registry.

Open:

- Web: `http://localhost:8929`
- SSH clone port: `2224`

Initial login:

- User: `root`
- Password: `12345678Qaz`

To use a custom root password on first startup:

```sh
GITLAB_ROOT_PASSWORD='AnotherStrong!Passw0rd' docker compose up -d
```

Windows PowerShell:

```powershell
$env:GITLAB_ROOT_PASSWORD="AnotherStrong!Passw0rd"; docker compose up -d
```

Check logs:

```sh
docker logs -f local-gitlab
```

Stop GitLab:

```sh
docker compose down
```

Remove GitLab volumes too:

```sh
docker compose down -v
```
