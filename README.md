# 如何使用 vscode debug odoo17 docker

情境如下, 已經使用了 odoo docker, 然後要如何透過 vscode 去 debug.

首先要安裝 vscode 的擴充套件

[Python Debugger](https://marketplace.visualstudio.com/items?itemName=ms-python.debugpy)

假如你希望可以修改 code 自動重啟, 你的 odoo 容器需要安裝 `pip3 install watchdog`

先來看 [docker-compose.yml](docker-compose.yml)

```yml
version: '3.5'
services:
  web17:
    image: odoo:17.0
    depends_on:
      - db
    command:
      # odoo --dev all # 一般啟動

      python3 -m debugpy --listen 0.0.0.0:19009 /usr/bin/odoo -c /etc/odoo/odoo.conf --dev all

      # 不使用 odoo.conf
      # python3 -m debugpy --listen 0.0.0.0:19009 /usr/bin/odoo --db_user=odoo --db_host=db --db_password=odoo
    ports:
      - "8017:8069"
      - "19009:19009"
    volumes:
      - odoo-web-data:/var/lib/odoo
      - ./config:/etc/odoo
      - ./addons:/mnt/extra-addons
```

command 的部份就是使用 debugpy 監聽一個 port (19009), 接著把 odoo 啟動,

因為要監聽 port, 所以也要設定 `- "19009:19009"`.

直接 `docker compose up -d` 即可.

接著看 [launch.json](https://github.com/twtrubiks/odoo-docker-tutorial/blob/vscode_debug_docker_odoo17/.vscode/launch.json)

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "odoo attach debugpy",
            "type": "debugpy",
            "request": "attach",
            "port": 8017,
            "host": "localhost",
            "debugServer": 19009,
            "pathMappings": [
                {
                    "localRoot": "${workspaceFolder}/addons",
                    "remoteRoot": "/mnt/extra-addons",
                }
            ],
        }
    ]
}
```

`"type"` 這邊使用 "debugpy"

`request` 因為是外部(docker)的, 所以使用 "attach"

可參考 [Python Debugger](https://github.com/twtrubiks/vscode_python_note?tab=readme-ov-file#python-debugger)

`port` 這邊我們是開 8017 port

`host` 因為本機就是 localost

`debugServer` 設定剛剛監聽的 port (19009)

`pathMappings` 這邊還蠻重要的, 分別設定 `localRoot` 以及 `remoteRoot`,

如果設定不正確會無法進中斷點.

基本上直接在 vscode 中 f5 就可以順利進入中斷點了.