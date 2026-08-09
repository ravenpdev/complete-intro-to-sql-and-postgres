# Complete Intro to SQL & PostgreSQL

**Requirements**

- docker
- postgres:14

```bash
docker pull postgres:14
```

```bash
docker run -e POSTGRES_PASSWORD=lol --name=pg --rm -d -p 5432:5432 postgres:14
```

```bash
docker exec -u postgres -it pg psql
```
