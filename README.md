# Build and Prep
```bash
git clone https://github.com/Hoverbear/scoop-queryengine.git
cd scoop-queryengine
docker build --rm=true -t queryengine .
docker pull mongo
```

# Run
```bash
docker run -P -d --name=queryenginedb mongo
docker run -d --link queryenginedb:queryenginedb -p 8080:8080 -v $(pwd):/app --name=queryengine queryengine
```

# Making certificates

In order to not have to accept a new cert every time, bake your own. [Source](https://library.linode.com/security/ssl-certificates/self-signed)
```bash
mkdir ./cert
openssl req -new -x509 -days 365 -nodes -out ./cert/server.crt -keyout ./cert/server.key
chmod 600 ./cert/*
```

# Troubleshooting
On SELinux (Fedora/CentOS/RHEL) you might need to set while in the project directory. [Source](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Resource_Management_and_Linux_Containers_Guide/sec-Sharing_Data_Across_Containers.html).

```bash
chcon -Rt svirt_sandbox_file_t $(pwd)
```
