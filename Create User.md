# Create User
``` sql
CREATE USER <username> IDENTIFIED BY <method>
DEFAULT TABLESPACE <ts_name>
TEMPORARY TABLESPACE <tts_name>
QUOTA <quota>
PROFILE <profile_name>
PASSWORD <expire>
ACCOUNT <lock | unlock>
```

# Alter Password User
``` sql
ALTER USER <username> IDENTIFIED BY <password>
```

# Drop user
``` sql
DROP USER <username> CASCADE
```

# Unlock User
``` sql
ALTER USER <username> ACCOUNT UNLOCK;
```

# System Privileges
- System privileges - diterapkan di seluruh sistem, tidak spesifik terhadap objek tertentu
- Menggunakan GRANT command untuk memberikan system privileges

- CREATE SESSION, digunakan agar user dapat login ke dalam database
- ALTER DATABASE, digunakan agar user dapat memanipulasi struktur fisik dari database (create tablespace, alter datafile dan lainnya)
- ALTER SYSTEM, digunakan agar user dapat mengkonfigurasi sistem database untuk mengubah parameter database.
- CREATE/ALTER TABLE, digunakan agar user dapat membuat tabel dan memodifikasi tabel pada schemanya
- CREATE ANY TABLE, digunakan agar user dapat membuat table diberbagai schema
- DROP ANY TABLE, digunakan agar user dapat menghapus table diberbagai schema
- SELECT ANY TABLE, digunakan agar user dapat memperoleh data dari berbagai tabel di berbagai schema
- UNLIMITED TABLESPACE, digunakan agar user dapat membuat objek diberbagai schema

