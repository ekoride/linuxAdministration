# linuxAdministration
Linux Users, Groups and Permissions

Users

Add a User

To add a new user our system, execute the following command. Remember you must have sudo privileges in order to add an user to the system, else you’ll get a permission denied prompt. If the current user doesn’t have sudo privileges you can either enter the command from the root mode or use sudo along with the below command.

useradd -m username

The above command creates a new user and sets the user’s default group with the same name as user which is also it’s primary group. To check which groups a particular user belongs to we can use the following command.

groups username

Modify existing User accounts
If you want to modify the groups of the existing user we can use the ‘usermod’ command with -g and -G options.

-g : Change the primary group of an existing user.

-G : Add supplement group for an existing user.

usermod -g GroupName username

usermod -G GroupName username

usermod -a -G GroupName username

The first command simple changes the primary group of the given username to the group name specified.

Note: When changing the supplemental group of an existing user, if you don’t include the -a option, -G will replace any existing supplemental groups with the groups specified after usermod. Using -a with -G ensures that the new groups are added but existing groups are not replaced.

The usermod also have few additional options like

-d: Changes the user’s home directory.

-l: Changes the user’s login name.

-L: Locks the account so the user can’t log in.

Delete a User

The userdel command deletes a user from the system.

userdel username

userdel -r username

The userdel command doesn’t delete the files in the user’s home directory unless you use the -r option.

Note: Instead of deleting the user, you could consider deactivating their account with usermod -L. This prevents the user from logging in while still giving you access to their account and associated permissions. Can implement in an organization when an employee leaves, you can move the ownership to other users.

View Users

All the users can be found at /etc/passwd directory. You can use the cat command to list out all the users or you can use the below command.

getent passwd

Groups

Groups can be created with the groupadd command.

groupadd groupname1

groupadd groupname2

Add Users to a Group

sudo gpasswd -A user -M user,user,user groupname

The gpasswd command allows you to manage the creation of groups and members of groups — the -A flag defines group administrators and the -M flag defines the members of the group as a comma-separated list.

View Groups

All the groups are present in the /etc/group directory. You can use the cat command to display the groups or you can also view using the below command.

getent group

Permissions

Each file and directory in the Linux systems have permissions to access them. We can read(r), write(w) or execute(x) a given file. These are the 3 basic permissions of files in the linux file system. You can view the permissions of each file or folder in a directory using the command

ls -l
The output looks something similar to this

drwxr-xr-x  10 ek    staff        320 Oct 11 15:35 VirtualBox VMs

drwxr-xr-x  13 ek    staff        416 Jul 27 23:34 elasticsearch-8.8.2

-rw-r--r--   1 ek    staff  407699911 Jul 27 23:28 elasticsearch-8.8.2-darwin-x86_64.tar.gz

Let’s understand what the permissions of each file indicates from the output and see what permissions are given to these types of owners:

user: the owner of the file

group: a larger group that the owner is a part of

other: all other users on the system

The first character indicates if it is a directory or a file. ‘d’ indicates if it is a directory. ‘-’ indicates a file. The next 9 characters define the access permissions to the file.

The first three characters indicates the permissions given to the user, the owner of the file, while the next three indicates the permissions given to the group, and last 3 indicates the permissions given to all other users in the system.

Let’s look at the first line of the output. The first character indicates that it is a directory and not a file. The next three characters(rwx) indicates that the owner of the file has all the permissions okay the file. The next three(r-x) indicate that the group the owner is a part of has only read and execute permissions. The last three also indicates the read and execute permissions to all other users in the system.

Change Permissions

The chmod command requires two arguments. The first argument indicates how to change permissions, and the second argument indicates the file or directory that you want to change permissions for.

chmod u+rwx,g+rwx,o+rwx filename

chmod u-rwx,g-rwx,o-rwx filename

chmod u=r,g=r,o=r filename

Note: The last command overwrites existing permissions. For instance, if the user previously had write permissions, these write permissions are removed after you specify only read permissions with =.

These are few examples on how we can change the file permissions.

Assign Group Ownership

Previously we have assigned groups to users. We have also seen how to add additional groups to users. Then we have seen how to add users to a group. Now we’ll see how to assign a group for each file.

sudo chgrp group file

This means that only members of the group that owns the file can have the permissions that are specified.
