# Count lines

## Counting all the lines returned by a command

```bash
<some_command> | wc -l
```

#### Example

To count the number of files returned by your `find` command (e.g., all `.jpg` files recursively), you can pipe the output to `wc -l`

```bash
sudo find /mnt/ext-hdd-8tb/photos -type f -name "*.jpg" | wc -l
```

This command counts (`wc -l`) the number of lines output by `find`, which equals the number of files matching the pattern.

# Files and directories

## Copy

```bash
cp <original_file> <target_file>
```

> [!note]
> Add the `-r` flag if you want to recursively copy. This is useful (and mandatory) for copying directories

> [!warning]
> Remember that you may want to add the `-f` flag to force-copy the file or directory into the target place. This is useful (and mandatory) when there's already a file or directory with the same name already in the target.

## Find

files by it's filename

### Legacy (Unix find util)

Using the legacy `find` Unix program in my setup generally involves to use `sudo`. That's because if I am on a `zsh` shell, legacy `find` will be overwritten to use `fd-find`, a Rust rewrite of the tool.

#### Example: Find all files that are `.jpg` files

```bash
sudo find /mnt/ext-hdd-8tb/photos -type f -name "*.jpg"
```

## Find files given multiple matching criteria

To match for example, files that contains different desired strings in their filename:

```bash
sudo find /mnt/ext-hdd-8tb/photos -type f \( -iname "*.jpg" -o -iname "*.jpeg" \)
```

This will find all the `.jpg` and `.jpeg` files recursively within a folder
## Delete

```bash
rm -rf <filename>
```
### [[File Management|Delete all files that contains a string in it's name]]
