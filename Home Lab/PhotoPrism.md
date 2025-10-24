PhotoPrism User Folder Issues
Problem 1: No built-in per-user physical upload folders
Cause: PhotoPrism does not dynamically create/mount per-user upload folders inside the container.
Fix:
	•	Predefine fixed subfolders in the `originals` volume mount in docker-compose, e.g., map `/mnt/ext-hdd-8tb/photos/user1` as `/photoprism/originals/user1`.
	•	Users upload to their assigned folder manually or via external mounts (WebDAV, SMB).
	•	Use external scripts/automation for post-upload organization if needed.
Problem 2: CLI commands for user role promotion missing
Cause: Newer PhotoPrism lacks `users promote`; direct DB edits are unsafe and tables differ.
Fix:
	•	Use `photoprism users mod -s <username>` CLI command to promote a user to superadmin.
	•	If CLI unavailable, check for UI user management in Plus editions or upgrade PhotoPrism.
	•	Avoid direct manual database edits (schema may be opaque or different).

# Promote to Super Admin

```bash
┌─>   vv at  vv-minipc in @192.168.1.10 ~ via  v3.13.7 took 3s
❯ docker exec -it photoprism /bin/sh

\u@250703-plucky-slim:\w$ photoprism users ls

┌──────────────────┬──────────────┬───────┬────────────────┬─────────────┬───────────┬──────────┬─────────────┐
│       UID        │   Username   │ Role  │ Authentication │ Super Admin │ Web Login │  WebDAV  │ Upload Path │
├──────────────────┼──────────────┼───────┼────────────────┼─────────────┼───────────┼──────────┼─────────────┤
│ ut246q3g4n6f91kr │ admin        │ Admin │ Local          │ Yes         │ Enabled   │ Enabled  │             │
│ ut246u0vd8vxghxh │ therustifyer │ Admin │ OIDC           │ No          │ Enabled   │ Disabled │             │
└──────────────────┴──────────────┴───────┴────────────────┴─────────────┴───────────┴──────────┴─────────────┘

\u@250703-plucky-slim:\photoprism users mod -s therustifyer
\u@250703-plucky-slim:\w$ photoprism users ls

┌──────────────────┬──────────────┬───────┬────────────────┬─────────────┬───────────┬──────────┬─────────────┐
│       UID        │   Username   │ Role  │ Authentication │ Super Admin │ Web Login │  WebDAV  │ Upload Path │
├──────────────────┼──────────────┼───────┼────────────────┼─────────────┼───────────┼──────────┼─────────────┤
│ ut246q3g4n6f91kr │ admin        │ Admin │ Local          │ Yes         │ Enabled   │ Enabled  │             │
│ ut246u0vd8vxghxh │ therustifyer │ Admin │ OIDC           │ Yes         │ Enabled   │ Disabled │             │
└──────────────────┴──────────────┴───────┴────────────────┴─────────────┴───────────┴──────────┴─────────────┘
```