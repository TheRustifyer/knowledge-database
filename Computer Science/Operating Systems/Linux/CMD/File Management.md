
# Move operations

## Change the extension of all of the files in a folder

For example, change all the `.png` file extensions in a directory for `.jpeg` for all the files.

```bash
for f in *.png; do mv "$f" "${f%.png}.jpeg"; done
```

•	`${f%.png}` strips the `.png` extension and adds `.jpeg`.
•	All `.png` files in the directory will be renamed to `.jpeg` extension.

> [!note]
>  If you want to preview (dry-run) the renamed files before moving, use:
>  ```bash
>  for f in *.png; do echo mv "$f" "${f%.png}.jpeg"; done
>```

# Delete operations

## Delete all files containing some string in the filename

For this operation, the best option is just to wildcard all the elements

```bash
rm *something*
```

> [!warning] Escape whitespaces
> Probably the filename will contain some whitespace, and you can easily remove them just by escaping them with an inverse pipe
> ```bash
rm *-something\ here* 
>```
