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

## GG
| Jenis Pengujian | Jumlah Test | Berhasil | Gagal | Catatan |
|-----------------|-------------|-----------|--------|----------|
| Component Test | 15 | 14 | 1 | 1 test gagal pada validasi input |
| Integration Test | 10 | 10 | 0 | Semua integrasi berjalan baik |
| System Testing | 20 | 19 | 1 | 1 test gagal pada edge case peminjaman |


## Test Case
| No | Fitur | Endpoint | Method | Role | Input | Expected Output | Status |
|----|-------|-----------|---------|------|--------|-----------------|---------|
| 1 | Login | /api/auth/login | POST | - | {username: "admin", password: "admin123"} | {token: "jwt_token", user: {id, username, role}} | Pass |
| 2 | Login Invalid | /api/auth/login | POST | - | {username: "wrong", password: "wrong"} | {message: "User tidak ditemukan"} | Pass |
| 3 | Register Mahasiswa | /api/auth/register | POST | - | {username, password, kode_mahasiswa, nama_mahasiswa, jk_mahasiswa, jurusan_mahasiswa, no_tel_mahasiswa} | {message: "Registrasi berhasil", data: {id, username, role}} | Pass |
| 4 | Register Duplicate | /api/auth/register | POST | - | {username: "existing"} | {message: "Username sudah digunakan"} | Pass |
| 5 | Get All Books | /api/books | GET | - | - | [{id, judul_buku, penulis, stok, ...}] | Pass |
| 6 | Get Book by ID | /api/books/:id | GET | - | id: 1 | {id, judul_buku, penulis, stok, ...} | Pass |
| 7 | Create Book (Admin) | /api/books | POST | admin | {judul_buku, penulis, penerbit, tahun_terbit, jumlah_buku, kategori} | {message: "Buku berhasil ditambahkan", data: {...}} | Pass |
| 8 | Create Book (Non-Admin) | /api/books | POST | staff | {book_data} | 403 Forbidden | Pass |
| 9 | Update Book | /api/books/:id | PUT | admin | {judul_buku, penulis, ...} | {message: "Buku berhasil diupdate"} | Pass |
| 10 | Delete Book | /api/books/:id | DELETE | admin | - | {message: "Buku berhasil dihapus"} | Pass |
| 11 | Get All Mahasiswa | /api/mahasiswa | GET | - | - | [{id, nama_mahasiswa, kode_mahasiswa, ...}] | Pass |
| 12 | Get Mahasiswa by ID | /api/mahasiswa/:id | GET | - | id: 1 | {id, nama_mahasiswa, kode_mahasiswa, ...} | Pass |
| 13 | Create Mahasiswa | /api/mahasiswa | POST | admin | {nama_mahasiswa, kode_mahasiswa, ...} | {message: "Mahasiswa berhasil ditambahkan"} | Pass |
| 14 | Update Mahasiswa | /api/mahasiswa/:id | PUT | admin | {nama_mahasiswa, ...} | {message: "Mahasiswa berhasil diupdate"} | Pass |
| 15 | Delete Mahasiswa | /api/mahasiswa/:id | DELETE | admin | - | {message: "Mahasiswa berhasil dihapus"} | Pass |
| 16 | Get All Staff | /api/staff | GET | admin | - | [{id, nama_staff, jabatan_staff, ...}] | Pass |
| 17 | Get Staff by ID | /api/staff/:id | GET | admin | id: 1 | {id, nama_staff, jabatan_staff, ...} | Pass |
| 18 | Create Staff | /api/staff | POST | admin | {nama_staff, jabatan_staff, ...} | {message: "Staff berhasil ditambahkan"} | Pass |
| 19 | Update Staff | /api/staff/:id | PUT | admin | {nama_staff, ...} | {message: "Staff berhasil diupdate"} | Pass |
| 20 | Delete Staff | /api/staff/:id | DELETE | admin | - | {message: "Staff berhasil dihapus"} | Pass |
| 21 | Create Peminjaman | /api/peminjaman | POST | staff | {id_buku, id_mahasiswa} | {message: "Peminjaman berhasil dibuat"} | Pass |
| 22 | Create Peminjaman (Invalid) | /api/peminjaman | POST | staff | {id_buku: "invalid"} | {message: "Data tidak valid"} | Pass |
| 23 | Create Pengembalian | /api/pengembalian | POST | staff | {id_peminjaman} | {message: "Pengembalian berhasil dibuat"} | Pass |
| 24 | Get Peminjaman Terlambat | /api/admin/peminjaman-terlambat | GET | admin,staff | - | [{id, mahasiswa, buku, tanggal_pinjam, ...}] | Pass |
| 25 | Get Mahasiswa Peminjaman | /api/mahasiswa/peminjaman-ku | GET | mahasiswa | - | {peminjaman: [...], pengembalian: [...]} | Pass |
| 26 | Fetch Book by ISBN | /api/fetch-book/:isbn | GET | - | isbn: "978-3-16-148410-0" | {title, authors, publishDate, ...} | Pass |
| 27 | Fetch Invalid ISBN | /api/fetch-book/:isbn | GET | - | isbn: "invalid" | {message: "Book not found"} | Pass |
| 28 | Access Without Token | /api/books | GET | - | - | 401 Unauthorized | Pass |
| 29 | Access With Invalid Token | /api/books | GET | - | token: "invalid" | 403 Forbidden | Pass |
| 30 | Access Wrong Role | /api/staff | GET | mahasiswa | valid_token | 403 Forbidden | Pass |
## Test Case untuk Edge Cases
| No | Scenario | Input | Expected Output | Status |
|----|----------|--------|-----------------|---------|
| 1 | Peminjaman Buku Habis | {id_buku: "out_of_stock"} | {message: "Stok buku habis"} | Pass |
| 2 | Peminjaman Melebihi Limit | {id_mahasiswa: "exceeded_limit"} | {message: "Melebihi batas peminjaman"} | Pass |
| 3 | Pengembalian Buku Rusak | {id_peminjaman, kondisi: "rusak"} | {message: "Denda dikenakan"} | Pass |
| 4 | Update Stok Negatif | {stok: -1} | {message: "Stok tidak valid"} | Pass |
| 5 | Register Invalid Phone | {no_tel_mahasiswa: "invalid"} | {message: "Nomor telepon tidak valid"} | Pass |
## Test Case untuk Validasi Input
| No | Field | Invalid Input | Expected Output | Status |
|----|-------|---------------|-----------------|---------|
| 1 | Username | "" | {message: "Username required"} | Pass |
| 2 | Password | "123" | {message: "Password minimal 6 karakter"} | Pass |
| 3 | Email | "invalid" | {message: "Email tidak valid"} | Pass |
| 4 | Phone | "abc" | {message: "Nomor telepon tidak valid"} | Pass |
| 5 | ISBN | "123" | {message: "ISBN tidak valid"} | Pass |
Test Case untuk Error Handling
| No | Scenario | Expected Behavior | Status |
|----|----------|-------------------|---------|
| 1 | Database Error | Return 500 with error message | Pass |
| 2 | Network Timeout | Return timeout error | Pass |
| 3 | Invalid JSON | Return 400 with parse error | Pass |
| 4 | Missing Required Fields | Return 400 with validation error | Pass |
| 5 | Server Error | Return 500 with generic error | Pass |


