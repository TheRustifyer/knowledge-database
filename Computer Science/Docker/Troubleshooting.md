
# Delete some container logs

```bash
sudo truncate -s 0 $(docker inspect --format='{{.LogPath}}' <container_id>)`
```

> [!explicación]
>  La sintaxis `$(...)` ejecuta el comando que está dentro y sustituye esa parte de la línea por su salida. Así te aseguras de que `truncate` siempre reciba la ruta correcta.

That's thanks to [[Logs path]]