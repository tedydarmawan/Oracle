# Oracle Database Storage Structure

## Logical Storage Structure
### Block
- Unit ruang terkecil pada sebuah Database Oracle
- Block dibaca ke dalam buffer
- Didefinisikan dengan parameter db_block_size
- TIdak bisa diubah setelah dibuat

Anatomy
```
................
: Header Infor :
................
:  Free Space  :
................
:              :
:  Used Space  :
................
```

Kemungkinan Ukuran, 2K sampai 32K
- 2K - Jarang digunakan
- 4K - OLTP
- 8K - Default dan Hybrid
- 16K - Data Warehouse, binary large object
- 32K - Jarang digunakan

### Extent
- Unit yang memiliki ruang lebih besar daripada block
- Extent terdiri dari block

32K Extent, terdiri dari 4 Blok dengan ukuran 8K
```
...........
: 8K : 8K :
...........
: 8K : 8K :
...........
```

Ukuran dari extent bergantung pada ukuran blok yang ditentukan oleh DBA.
Di awal structure pasti akan ada initial extent, ketika initial extent full maka akan membentuk extent berikutnya. Ukuran extent berikutnya dapat diubah secara langsung.
```
...................................................................
: Intial :  Next  :  Next  :     Next    :           Next         :
: extent : extent : extent :    extent   :          extent        :
...................................................................
```

- Dictionary Managed Extent
- Locally Managed Extent (Recommended)

### Segment
- Unit yang memiliki ruang lebih besar daripada extent
- Segment terdiri dari extent

```
1 segment
...................................................................
: Intial :  Next  :  Next  :     Next    :           Next         :
: extent : extent : extent :    extent   :          extent        :
...................................................................
```

Tipe segment:
- Table segment
- Index segment
- Undo segment
- Temporary segment
- Table partition segment
- Index partition segment
- LOB/LOB Index (Large OBject)

### Tablespace
- Nama secara logika yang diberikan ke satu atau lebih datafile fisik.
- Smallfile tablespace merupakan tipe default
- Jumlah maksimal datafile per smallfile tablespace = 1022

- Data dictionary views
  - DBA_TABLESPACES
  - V$TABLESPACE
  
Create tablespace
``` sql
CREATE TABLESPACE ts_example
DATAFILE 'C:\APP\DEV\ORADATA\ORCL\dataexample01.dbf'
SIZE 100M;
```

Tambah datafile ke suatu tablespace
``` sql
ALTER TABLESPACE ts_example ADD DATAFILE
'C:\APP\DEV\ORADATA\ORCL\dataexample02.dbf'
SIZE 50M;
```

Ubah ukuran suatu datafile berdasarkan kolom FILE_ID di dictionary DBA_DATA_FILES
``` sql
ALTER DATABASE DATAFILE 6 RESIZE 100m;
```

Hapus tablespace
``` sql
DROP TABLESPACE ts_example INCLUDING CONTENTS AND DATAFILES;
```

## Physical Storage Structure
### Data File
- Unit yang memiliki ruang lebih besar dari segment
- Datafile terdiri dari segment

Tipe Data File
- Non-specific data
- Undo data
- Temporary data (tempfiles)

- Limit
  - Max size = 4.194.304 x block size
    - Contoh: 4194304 x 8K = 32GB (Ukuran maksimal)
  - Maksimal jumlah datafile dalam suatu DB = 65.533
  - Ukuran maksimal database = 8 Petabyte
  
- Data dicitionary views
  - DBA_DATA_FILES
  - DBA_TEMP_FILES
  - V$DATAFILE
  - V$TEMPFILE

Auto Extend Datafile

Setting extend secara otomatis memiliki beberapa keuntungan:
- Mengurangi intervensi pada saat tablespace penuh
- Memastikan aplikasi tidak akan berhenti ketika gagal untuk alokasi extent

```
CREATE TABLESPACE ts_example
DATAFILE 'C:\APP\DEV\ORADATA\ORCL\EXAMPLE01.DBF' SIZE 10M
AUTOEXTEND ON 
  NEXT 512K
  MAXSIZE UNLIMITED;
```

### Control File
### Online Redo Log
