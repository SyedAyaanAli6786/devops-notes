# Class Notes – Shell Scripting & Linux (Today)



### Variables & Input

- Variable format: `NAME=value`
- Access: `$NAME`
- Take input: `read -p`

Example:

```
read name
echo$name

```



### Conditions & Loops

- `if [ condition ]` → decision making
- `f file` → check file exists
- `for` / `while` → repetition

Used for automation.



### Common Scripts Practiced

- Print date & time
- Check disk usage
- Check file exists
- Add numbers
- Check internet
- Backup directory
- Check user login
- System uptime
- Scripts make admin work easy.



### Docker / Bash Execution

- Bash-specific features don’t work with `sh`.
- Always use `./file.sh` for Bash scripts.
- `sh file.sh` may fail.



### Bash vs Sh

| Feature | bash | sh |
| --- | --- | --- |
| Arrays | Yes | No |
| String ops | Yes | No |
| Speed | Medium | Fast |
| Features | Advanced | Basic |
- Use `bash` for complex scripts.



### String Handling

- `${str}` → variable value
- `${str^^}` → Uppercase
- `${str,,}` → Lowercase
- `rev` → Reverse string
- Substring → `${str:pos:len}`



### Functions & Reuse

- Functions store reusable code.
- Scripts can import other scripts.

Example:

```
source utils.sh

```

Used for project automation.



### User Management Script

- `useradd`, `groupadd` used.
- Script checks:
    - Root access
    - Arguments
    - Existing user/group
- Professional scripts use error handling.



### Automation Assignment

- Created utility function.
- Used timestamp in folder name.
- Used `date` command.
- Created reusable scripts.
- Learned modular scripting.



### File Permissions

- rwx → read, write, execute
- chmod → change permission
- chown → change owner
- chgrp → change group

Example:

```
chmod 755 file.sh

```



## 🔐 ACL (Access Control List)

### What are ACLs?

- Give multiple users different permissions.
- More flexible than chmod.



### Important ACL Commands

- View → `getfacl file`
- Set → `setfacl`
- Remove → `setfacl -x`
- Clear → `setfacl -b`

Example:

```
setfacl -m u:john:rwfile.txt

```



### Default ACL

- Applies permissions to new files.
- Used in shared folders.



### Mask

- Limits maximum permission.
- Even if rw is given, mask can reduce it.



### ACL in Scripts

- Used for team folders.
- Automatically gives access.



## Key Points

- Shebang decides shell type
- Bash has more features than sh
- Always give execute permission
- Use functions for reuse
- Scripts should check errors
- ACL gives advanced permissions
- Automation saves time
- Never misuse `rm -rf`