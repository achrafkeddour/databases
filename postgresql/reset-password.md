If you're using Windows, you can still reset the PostgreSQL password or set one if it wasn’t configured during installation. Here's how to do it:

1. **Open Command Prompt as Administrator**:
   - Press `Windows + X` and select **Command Prompt (Admin)** or **Windows PowerShell (Admin)**.

2. **Switch to the PostgreSQL bin Directory**:
   - Navigate to the PostgreSQL bin directory. The path is usually something like:
     ```
     cd "C:\Program Files\PostgreSQL\<version>\bin"
     ```
     Replace `<version>` with the actual version number of PostgreSQL installed on your system.

3. **Access the PostgreSQL Shell**:
   - Run the following command to open the PostgreSQL prompt as the postgres user:
     ```
     psql -U postgres
     ```
     If it asks for a password and you don't know it, skip to the next step to reset it.

4. **Reset the Password**:
   - If you can’t log in because you don’t know the password, you can reset it:
   - Open **pg_hba.conf** file in a text editor. This file is usually located in the data directory of your PostgreSQL installation, like:
     ```
     C:\Program Files\PostgreSQL\<version>\data\pg_hba.conf
     ```
   - Find the line that looks like this:
     ```
     host    all             all             127.0.0.1/32            md5
     ```
   - Change `md5` to `trust` for the `local` connection, so it looks like:
     ```
     host    all             all             127.0.0.1/32            trust
     ```
   - Save the file and restart the PostgreSQL service. You can do this by running:
     ```
     net stop postgresql-x64-<version>
     net start postgresql-x64-<version>
     ```

5. **Change the Password**:
   - Now log into PostgreSQL without a password:
     ```
     psql -U postgres
     ```
   - Once logged in, reset the password:
     ```
     ALTER USER postgres PASSWORD 'new_password';
     ```
   - Exit the PostgreSQL prompt:
     ```
     \q
     ```

6. **Revert Changes in `pg_hba.conf`**:
   - Edit `pg_hba.conf` again and change `trust` back to `md5` to ensure proper security.
   - Restart the PostgreSQL service again.

After this, your new password should work, and you can connect using the `psql` shell with the updated password.

Let me know if you need further assistance!
