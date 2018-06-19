# grp Unix Group Database

## Querying All Groups

```python
# grp_getgrall.py
import grp
import textwrap

# Load all of the user data, sorted by username
all_groups = grp.getgrall()
interesting_groups = {
    g.gr_name: g
    for g in all_groups
    if not g.gr_name.startswith('_')
}
print(len(interesting_groups.keys()))

# Find the longest length for a few fields
name_length = max(len(k) for k in interesting_groups) + 1
gid_length = max(len(str(u.gr_gid))
                 for u in interesting_groups.values()) + 1

# Set the members field width to avoid table columns
# wrapping
members_width = 19

# Print report headers
fmt = ' '.join(['{:<{name_length}}',
                '{:{gid_length}}',
                '{:<{members_width}}',
                ])
print(fmt.format('Name',
                 'GID',
                 'Members',
                 name_length=name_length,
                 gid_length=gid_length,
                 members_width=members_width))
print('-' * name_length,
      '-' * gid_length,
      '-' * members_width)

# Print the data
prefix = ' ' * (name_length + gid_length + 2)
for name, g in sorted(interesting_groups.items()):
    # Format members to start in the column on the same line but
    # wrap as needed with an indent sufficient to put the
    # subsequent lines in the members column. The two indent
    # prefixes need to be the same to compute the wrap properly,
    # but the first should not be printed so strip it.
    members = textwrap.fill(
        ', '.join(g.gr_mem),
        initial_indent=prefix,
        subsequent_indent=prefix,
        width=members_width + len(prefix),
    ).strip()
    print(fmt.format(g.gr_name,
                     g.gr_gid,
                     members,
                     name_length=name_length,
                     gid_length=gid_length,
                     members_width=members_width))
```

## Group Memberships for a User

```python
# grp_groups_for_user.py
import grp

username = 'dhellmann'
group_names = set(
    g.gr_name
    for g in grp.getgrall()
    if username in g.gr_mem
)
print(username, 'belongs to:', ', '.join(sorted(group_names)))
```

## Finding a Group By Name

```python
# grp_getgrnam.py
import grp

name = 'admin'
info = grp.getgrnam(name)
print('Name    :', info.gr_name)
print('GID     :', info.gr_gid)
print('Password:', info.gr_passwd)
print('Members :', ', '.join(info.gr_mem))
```

## Finding a Group by ID

```python
# grp_getgrgid_process.py
import grp
import os

gid = os.getgid()
group_info = grp.getgrgid(gid)
print('Currently running with GID={} name={}'.format(
    gid, group_info.gr_name))
```

```python
# grp_getgrgid_fileowner.py
import grp
import os

filename = 'grp_getgrgid_fileowner.py'
stat_info = os.stat(filename)
owner = grp.getgrgid(stat_info.st_gid).gr_name

print('{} is owned by {} ({})'.format(
    filename, owner, stat_info.st_gid))
```
