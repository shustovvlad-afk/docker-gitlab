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

## Windows runner

Project `root/onecconnect` has a project runner with tag `windows`.
Install GitLab Runner on a Windows machine and run PowerShell as
Administrator.

Current test runner settings:

- GitLab URL: `http://192.168.0.104:8929`
- Runner directory: `C:\GitLab-Runner`
- Runner name: `local-windows-runner`
- Executor: `shell`
- Shell: `powershell`
- Tag: `windows`
- Token: `glrt-YUJk_tWdxHGeu55fPLLztG86MQpwOjEKdDozCnU6MQ8.01.171wt2yo6`

Fresh install and registration:

```powershell
$GITLAB_URL = "http://192.168.0.104:8929"
mkdir C:\GitLab-Runner
cd C:\GitLab-Runner
Invoke-WebRequest -Uri https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-windows-amd64.exe -OutFile gitlab-runner.exe
.\gitlab-runner.exe register --non-interactive --url $GITLAB_URL --clone-url $GITLAB_URL --token "glrt-YUJk_tWdxHGeu55fPLLztG86MQpwOjEKdDozCnU6MQ8.01.171wt2yo6" --executor "shell" --description "local-windows-runner" --tag-list "windows" --run-untagged="false" --locked="false"
.\gitlab-runner.exe install
.\gitlab-runner.exe start
```

If `C:\GitLab-Runner` already exists, check the current runner instead of
registering a duplicate:

```powershell
cd C:\GitLab-Runner
.\gitlab-runner.exe verify --config C:\GitLab-Runner\config.toml
.\gitlab-runner.exe status
```

Expected result:

- `Verifying runner... is valid`
- `gitlab-runner: Service is running`

If the Mac IP changes, replace `192.168.0.104` with the current IP address of
the Mac running GitLab.

Stop GitLab:

```sh
docker compose down
```

Remove GitLab volumes too:

```sh
docker compose down -v
```
