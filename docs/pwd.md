# pwd Unix Password Database


## Querying All Users

```python
# pwd_getpwall.py
import pwd
import operator

# Load all of the user data, sorted by username
all_user_data = pwd.getpwall()
interesting_users = sorted(
    (u for u in all_user_data
     if not u.pw_name.startswith('_')),
    key=operator.attrgetter('pw_name')
)

# Find the longest lengths for a few fields
username_length = max(len(u.pw_name)
                      for u in interesting_users) + 1
home_length = max(len(u.pw_dir)
                  for u in interesting_users) + 1
uid_length = max(len(str(u.pw_uid))
                 for u in interesting_users) + 1

# Print report headers
fmt = ' '.join(['{:<{username_length}}',
                '{:>{uid_length}}',
                '{:<{home_length}}',
                '{}'])
print(fmt.format('User',
                 'UID',
                 'Home Dir',
                 'Description',
                 username_length=username_length,
                 uid_length=uid_length,
                 home_length=home_length))
print('-' * username_length,
      '-' * uid_length,
      '-' * home_length,
      '-' * 20)

# Print the data
for u in interesting_users:
    print(fmt.format(u.pw_name,
                     u.pw_uid,
                     u.pw_dir,
                     u.pw_gecos,
                     username_length=username_length,
                     uid_length=uid_length,
                     home_length=home_length))
```

## Querying User By Name

```python
# pwd_getpwnam.py
import pwd
import sys

username = sys.argv[1]
user_info = pwd.getpwnam(username)

print('Username:', user_info.pw_name)
print('Password:', user_info.pw_passwd)
print('Comment :', user_info.pw_gecos)
print('UID/GID :', user_info.pw_uid, '/', user_info.pw_gid)
print('Home    :', user_info.pw_dir)
print('Shell   :', user_info.pw_shell)
```

## Querying User By UID

```python
# pwd_getpwuid_fileowner.py
import pwd
import os

filename = 'pwd_getpwuid_fileowner.py'
stat_info = os.stat(filename)
owner = pwd.getpwuid(stat_info.st_uid).pw_name

print('{} is owned by {} ({})'.format(
    filename, owner, stat_info.st_uid))
```

```python
# pwd_getpwuid_process.py
import pwd
import os

uid = os.getuid()
user_info = pwd.getpwuid(uid)
print('Currently running with UID={} username={}'.format(
    uid, user_info.pw_name))
```
