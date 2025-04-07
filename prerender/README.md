# Prerender
- https://github.com/prerender/prerender
- https://github.com/prerender/prerender-node
- https://github.com/tvanro/prerender-alpine
- https://hub.docker.com/r/tvanro/prerender-alpine
## Usage
```bash
git clone git@github.com:prerender/prerender.git
cd prerender
rm -frv .git* test .pre* LICENSE
docker-compose build prerender
docker-compose up
# test
curl http://localhost:3000/render?url=https://www.example.com/
```
