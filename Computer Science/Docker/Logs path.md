# Find what is the path of the logs of some container

```bash
docker inspect --format='{{.LogPath}}' <container_id>
```

Example output:

```bash
/var/lib/docker/containers/d562e58512538d62830e3db080900ffd77f2b0db12b535574ad0292ff17df655/d562e58512538d62830e3db080900ffd77f2b0db12b535574ad0292ff17df655-json.log
```