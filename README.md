This is the installation of mongo db and mongosh into debian distro. please read before installation.

How to correct some of the errors found upon running mongosh :

  The server generated these startup warnings when booting
   2023-08-05T10:25:46.969+03:00: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine. See http://dochub.mongodb.org/core/prodnotes-filesystem
   2023-08-05T10:25:48.827+03:00: Access control is not enabled for the database. Read and write access to data and configuration is unrestricted
   2023-08-05T10:25:48.900+03:00: /sys/kernel/mm/transparent_hugepage/enabled is 'always'. We suggest setting it to 'never'
   2023-08-05T10:25:48.901+03:00: vm.max_map_count is too low




mv ~/.mongorc.js ~/.mongoshrc.js     


1. **Using XFS Filesystem with WiredTiger Storage Engine:**
   To use XFS filesystem with WiredTiger, follow these general steps:
   - First, ensure that you have XFS filesystem support enabled in your operating system.
   - Create a new partition or volume using XFS for your MongoDB data directory.
   - Mount the XFS partition or volume to a directory where you want to store your MongoDB data.
   - Update your MongoDB configuration file (usually `mongod.conf`) to point to the new data directory on the XFS filesystem.
   - Restart your MongoDB server for the changes to take effect.

2. **Enabling Access Control:**
   To enable access control for your MongoDB server, follow these steps:
   - Open your MongoDB configuration file (`mongod.conf`), which is typically located in the MongoDB configuration directory.
   - Look for the `security` section and uncomment or add the `authorization` parameter with the value `enabled`.
   - Save the configuration file and restart your MongoDB server.
   - After enabling access control, you'll need to create users and assign appropriate roles to control access to the database. Use the `db.createUser()` function in the MongoDB shell to create users with specific roles.

3. **Setting Transparent Huge Pages (THP) to 'never':**
   The process for disabling Transparent Huge Pages varies depending on the Linux distribution you're using. Here are general steps that should work for most systems:
   - Open a terminal or SSH into your server.
   - Check the current THP setting by running: `cat /sys/kernel/mm/transparent_hugepage/enabled`
   - If the output is `always`, you need to set it to `never`. Run the following command with administrative privileges:
     ```
     echo never | sudo tee /sys/kernel/mm/transparent_hugepage/enabled
     ```
   - Additionally, you can add the same command to your system's startup script to ensure the setting persists after a reboot.

4. **Increasing vm.max_map_count:**
   The process for increasing the `vm.max_map_count` kernel parameter also depends on your Linux distribution. Here are general steps:
   - Open a terminal or SSH into your server.
   - Check the current value of `vm.max_map_count` by running: `sysctl vm.max_map_count`
   - If the value is too low, you can temporarily increase it by running:
     ```
     sysctl -w vm.max_map_count=<new_value>
     ```
     Replace `<new_value>` with a higher number, for example, `262144`.
   - To make the change permanent, update the `sysctl.conf` or a file in the `/etc/sysctl.d/` directory with the following line:
     ```
     vm.max_map_count=<new_value>
     ```
     Save the file and run `sysctl -p` to apply the changes.

Always remember to carefully follow your system's documentation and backup any critical data before making significant changes to your configuration.

If you encounter any difficulties or uncertainties during the process, it's best to consult with a knowledgeable system administrator or MongoDB expert to ensure the changes are made correctly and safely.
