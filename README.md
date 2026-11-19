# Лабораторная работа: JFrog Artifactory
**Работа выполнена:** Бурматовой Е.С.
**ОС:** macOS (Apple Silicon)
**Program:** Docker, JFrog Artifactory

## Цель работы:
Поднять свой менеджер артефактов, научиться работать с ним с помощью командной строки.

### Результаты выполненных заданий базового уровня:

**Task01**
```bash
(base) katrin@MacBook-Pro-Katrin artifactory-lab-students % docker compose ps
NAME                   IMAGE                                                   COMMAND                  SERVICE       CREATED         STATUS                   PORTS
artifactory            releases-docker.jfrog.io/jfrog/artifactory-oss:latest   "/usr/local/bin/fips…"   artifactory   4 minutes ago   Up 4 minutes (healthy)   0.0.0.0:8081-8082->8081-8082/tcp, [::]:8081-8082->8081-8082/tcp
artifactory-postgres   postgres:16-alpine                                      "docker-entrypoint.s…"   postgres      4 minutes ago   Up 4 minutes (healthy)   5432/tcp
```

**Task02**
```bash
(base) katrin@MacBook-Pro-Katrin artifactory-lab-students % curl -o /dev/null -w "%{http_code}\n" http://localhost:8082/ui/
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 24853    0 24853    0     0   284k      0 --:--:-- --:--:-- --:--:--  285k
200
```

**Task03**
<img width="1512" height="867" alt="Снимок экрана — 2026-11-18 в 21 04 07" src="https://github.com/user-attachments/assets/0650a21d-b864-4b74-a55f-c9d59637c997" />

**Task04**
<img width="1512" height="868" alt="Снимок экрана — 2026-11-18 в 21 13 41" src="https://github.com/user-attachments/assets/8eb25564-42ac-4894-a694-7f6684260864" />

**Task05**
```bash
(base) katrin@MacBook-Pro-Katrin artifactory-lab-students % curl -u admin:password http://localhost:8081/artifactory/api/repositories
[ {
  "key" : "example-repo-local",
  "description" : "Example artifactory repository",
  "type" : "LOCAL",
  "url" : "http://localhost:8081/artifactory/example-repo-local",
  "packageType" : "Generic"
}, {
  "key" : "myapp-local",
  "description" : "",
  "type" : "LOCAL",
  "url" : "http://localhost:8081/artifactory/myapp-local",
  "packageType" : "Generic"
} ]%
```

**Task06**
```bash
(base) katrin@MacBook-Pro-Katrin task06-build-app % tar -tzf myapp-1.0.0.tar.gz
./
./VERSION
./main.py
```

**Task07**
```bash
(base) katrin@MacBook-Pro-Katrin task07-upload % chmod +x upload.sh
./upload.sh
{
  "repo" : "myapp-local",
  "path" : "/myapp/1.0.0/myapp-1.0.0.tar.gz",
  "created" : "2026-11-18T19:37:09.956Z",
  "createdBy" : "admin",
  "downloadUri" : "http://localhost:8081/artifactory/myapp-local/myapp/1.0.0/myapp-1.0.0.tar.gz",
  "mimeType" : "application/x-gzip",
  "size" : "210",
  "checksums" : {
    "sha1" : "75ea78e8223a074317ee3dafcb225e093e6ce802",
    "md5" : "7977dcd7fda4d5590b82ff03c58e7acc",
    "sha256" : "3dd9bc9af3bc8364cb20cc6d21f66ed76d0eef96ff0c6656cac2d1a5583ebfe8"
  },
  "originalChecksums" : {
    "sha256" : "3dd9bc9af3bc8364cb20cc6d21f66ed76d0eef96ff0c6656cac2d1a5583ebfe8"
  },
  "uri" : "http://localhost:8081/artifactory/myapp-local/myapp/1.0.0/myapp-1.0.0.tar.gz"
}%
```

**Task08**
<img width="1512" height="866" alt="Снимок экрана — 2026-11-18 в 22 44 21" src="https://github.com/user-attachments/assets/3b48577f-c309-492f-8c53-63f65b7ecc43" />

**Task09**
```bash
(base) katrin@MacBook-Pro-Katrin task09-download % ls -la myapp-1.0.0.tar.gz
-rw-r--r--  1 katrin  staff  210 18 нояб. 22:48 myapp-1.0.0.tar.gz
```

**Task10**
```bash
(base) katrin@MacBook-Pro-Katrin task09-download % curl -s -u admin:password \
  http://localhost:8081/artifactory/api/storage/myapp-local/myapp/1.0.0/myapp-1.0.0.tar.gz \
  | grep sha256
    "sha256" : "3dd9bc9af3bc8364cb20cc6d21f66ed76d0eef96ff0c6656cac2d1a5583ebfe8"
    "sha256" : "3dd9bc9af3bc8364cb20cc6d21f66ed76d0eef96ff0c6656cac2d1a5583ebfe8"
```

**Task11**
<img width="1512" height="870" alt="Снимок экрана — 2026-11-18 в 23 12 32" src="https://github.com/user-attachments/assets/0a76167f-a4f8-4a97-bbe3-449afcf67577" />

**Task12**
```bash
(base) katrin@MacBook-Pro-Katrin task12-file-info % curl -s -u admin:password \
  http://localhost:8081/artifactory/api/storage/myapp-local/myapp/1.0.0/myapp-1.0.0.tar.gz
{
  "repo" : "myapp-local",
  "path" : "/myapp/1.0.0/myapp-1.0.0.tar.gz",
  "created" : "2026-11-18T19:37:09.956Z",
  "createdBy" : "admin",
  "lastModified" : "2026-11-18T19:37:09.956Z",
  "modifiedBy" : "admin",
  "lastUpdated" : "2026-11-18T19:37:09.993Z",
  "downloadUri" : "http://localhost:8081/artifactory/myapp-local/myapp/1.0.0/myapp-1.0.0.tar.gz",
  "mimeType" : "application/x-gzip",
  "size" : "210",
  "checksums" : {
    "sha1" : "75ea78e8223a074317ee3dafcb225e093e6ce802",
    "md5" : "7977dcd7fda4d5590b82ff03c58e7acc",
    "sha256" : "3dd9bc9af3bc8364cb20cc6d21f66ed76d0eef96ff0c6656cac2d1a5583ebfe8"
  },
  "originalChecksums" : {
    "sha256" : "3dd9bc9af3bc8364cb20cc6d21f66ed76d0eef96ff0c6656cac2d1a5583ebfe8"
  },
  "uri" : "http://localhost:8081/artifactory/api/storage/myapp-local/myapp/1.0.0/myapp-1.0.0.tar.gz"
}%
```

**Task13**
```bash
(base) katrin@MacBook-Pro-Katrin task13-pipe % chmod +x info.sh
./info.sh
3dd9bc9af3bc8364cb20cc6d21f66ed76d0eef96ff0c6656cac2d1a5583ebfe8
```

**Task14**
```bash
(base) katrin@MacBook-Pro-Katrin task14-checksum % chmod +x checked-upload.sh
./checked-upload.sh
{
  "repo" : "myapp-local",
  "path" : "/myapp/1.0.1-checked/myapp-1.0.1.tar.gz",
  "created" : "2026-11-19T07:48:11.814Z",
  "createdBy" : "admin",
  "downloadUri" : "http://localhost:8081/artifactory/myapp-local/myapp/1.0.1-checked/myapp-1.0.1.tar.gz",
  "mimeType" : "application/x-gzip",
  "size" : "218",
  "checksums" : {
    "sha1" : "e06c4bb57482fbcecf3556b3e8eb15ba561f3cdc",
    "md5" : "7cf7e9449db3533e89695a3fc0158f83",
    "sha256" : "ffe12518dff08638d90eaee67fb4de7b1aed92615ecf3145da714568a3d6cf1d"
  },
  "originalChecksums" : {
    "sha256" : "ffe12518dff08638d90eaee67fb4de7b1aed92615ecf3145da714568a3d6cf1d"
  },
  "uri" : "http://localhost:8081/artifactory/myapp-local/myapp/1.0.1-checked/myapp-1.0.1.tar.gz"
}
HTTP-код: 201
(base) katrin@MacBook-Pro-Katrin task14-checksum % nano checked-upload.sh
(base) katrin@MacBook-Pro-Katrin task14-checksum % chmod +x checked-upload.sh
./checked-upload.sh
{
  "errors" : [ {
    "status" : 409,
    "message" : "Checksum policy 'LocalRepoChecksumPolicy: CLIENT' rejected the artifact 'myapp-local:myapp/1.0.1-checked/myapp-1.0.1.tar.gz'. Checksums info: ChecksumsInfo{checksums={SHA-1=ChecksumInfo{type=SHA-1, original='null', actual='e06c4bb57482fbcecf3556b3e8eb15ba561f3cdc'}, MD5=ChecksumInfo{type=MD5, original='null', actual='7cf7e9449db3533e89695a3fc0158f83'}, SHA-256=ChecksumInfo{type=SHA-256, original='0000', actual='ffe12518dff08638d90eaee67fb4de7b1aed92615ecf3145da714568a3d6cf1d'}}}"
  } ]
}
HTTP-код: 409
```

**Task15**
```bash
(base) katrin@MacBook-Pro-Katrin task15-aql % curl -s -u admin:Buldogislove_2025 -X POST \
  -H "Content-Type: text/plain" \
  -d 'items.find({"repo": "myapp-local", "path": {"$match": "myapp/1.0.0*"}})' \
  http://localhost:8081/artifactory/api/search/aql

{
"results" : [ {
  "repo" : "myapp-local",
  "path" : "myapp/1.0.0",
  "name" : "myapp-1.0.0.tar.gz",
  "type" : "file",
  "size" : 210,
  "created" : "2026-11-18T19:37:09.956Z",
  "created_by" : "admin",
  "modified" : "2026-11-18T19:37:09.956Z",
  "modified_by" : "admin",
  "updated" : "2026-11-18T19:37:09.993Z"
} ],
"range" : {
  "start_pos" : 0,
  "end_pos" : 1,
  "total" : 1
}
}
```

## Все действия отражены в прикрепленном файле:
**[Открыть полный отчет (PDF)](./Лабораторная%20работа-JFrog%20Artifactory_Бурматова%20ЕС.pdf)**
