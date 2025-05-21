| HTTP Method | Endpoint                          | Deskripsi & Role Akses                        |
|-------------|-----------------------------------|-----------------------------------------------|
| POST        | /api/mahasiswa                    | Tambah data mahasiswa (**admin**)             |
| PUT         | /api/mahasiswa/:id                | Update data mahasiswa (**admin**)             |
| DELETE      | /api/mahasiswa/:id                | Hapus data mahasiswa (**admin**)              |
| POST        | /api/books                        | Tambah data buku (**admin**)                  |
| PUT         | /api/books/:id                    | Update data buku (**admin**)                  |
| DELETE      | /api/books/:id                    | Hapus data buku (**admin**)                   |
| POST        | /api/peminjaman                   | Tambah data peminjaman (**staff**)            |
| POST        | /api/pengembalian                 | Tambah data pengembalian (**staff**)          |
| GET         | /api/mahasiswa/peminjaman-ku      | Lihat riwayat peminjaman (**mahasiswa**)      |
| GET         | /api/staff                        | Lihat semua staff (**admin**)                 |
| GET         | /api/staff/:id                    | Lihat detail staff (**admin**)                |
| POST        | /api/staff                        | Tambah staff (**admin**)                      |
| PUT         | /api/staff/:id                    | Update staff (**admin**)                      |
| DELETE      | /api/staff/:id                    | Hapus staff (**admin**)                       |
| GET         | /api/admin/peminjaman-terlambat   | Lihat peminjaman terlambat (**admin/staff**)  |
