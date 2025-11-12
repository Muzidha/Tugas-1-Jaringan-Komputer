# Tugas-1-Jaringan-Komputer

# Tugas 1 Subnetting & Routing - Komunikasi Data dan Jaringan Komputer

Repository ini berisi pengerjaan Tugas 1 Mata Kuliah Komunikasi Data dan Jaringan Komputer, yang mencakup perancangan topologi, perhitungan VLSM, agregasi rute CIDR, dan file konfigurasi Cisco Packet Tracer.

-   **Nama:** Muhammad Ziddan Habibi
-   **NRP:** 5027241122

---

## 1. Perhitungan Base Network

Sesuai ketentuan, base network dihitung secara unik berdasarkan NRP mahasiswa.

-   **NRP:** `5027241122`
-   **Perhitungan:** `5027241122 mod 256 = 162`
-   **Base Network:** `10.162.0.0`

---

## 2. Desain Topologi Jaringan

Topologi dirancang menggunakan Cisco Packet Tracer untuk memenuhi kebutuhan jaringan di Kantor Pusat dan Kantor Cabang yang terhubung melalui link WAN (disimulasikan dengan Cloud).

-   **Kantor Pusat:** Berisi 1 Router, 1 Switch, dan host yang mewakili 5 subnet (Sekretariat, Kurikulum, Guru & Tendik, Sarana, Server).
-   **Kantor Cabang:** Berisi 1 Router dan host yang mewakili 1 subnet (Pengawas Sekolah).
-   **WAN:** Menghubungkan Router Pusat dan Router Cabang.

<img width="1687" height="734" alt="image" src="https://github.com/user-attachments/assets/689e9022-499d-42fa-ab51-b618deed273f" />


---

## 3. Tabel Perhitungan VLSM

Perhitungan VLSM dimulai dari base network `10.162.0.0` dan diurutkan berdasarkan kebutuhan host terbesar. Link WAN (Point-to-Point) ditambahkan dengan kebutuhan 2 host (/30).

| Subnet | Kebutuhan | Prefix | Network ID | Subnet Mask | IP Available | Broadcast | Available Host |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Sekretariat | 380 | /23 | `10.162.0.0` | `255.255.254.0` | `10.162.0.1` - `10.162.1.254` | `10.162.1.255` | 510 |
| B Kurikulum | 220 | /24 | `10.162.2.0` | `255.255.255.0` | `10.162.2.1` - `10.162.2.254` | `10.162.2.255` | 254 |
| B Guru & Tendik | 95 | /25 | `10.162.3.0` | `255.255.255.128` | `10.162.3.1` - `10.162.3.126` | `10.162.3.127` | 126 |
| B Sarana & Prasarana| 45 | /26 | `10.162.3.128`| `255.255.255.192` | `10.162.3.129` - `10.162.3.190` | `10.162.3.191` | 62 |
| B Pengawas Sekolah | 18 | /27 | `10.162.3.192`| `255.255.255.224` | `10.162.3.193` - `10.162.3.222` | `10.162.3.223` | 30 |
| Server dan admin | 6 | /29 | `10.162.3.224`| `255.255.255.248` | `10.162.3.225` - `10.162.3.230` | `10.162.3.231` | 6 |

---

## 4. Tabel Agregasi Rute (CIDR / Supernetting)

Untuk efisiensi tabel routing, jaringan di setiap lokasi diagregasi (supernetting).

| Lokasi | Daftar Subnet | Rentang Alamat | Supernet (Agregasi) |
| :--- | :--- | :--- | :--- |
| **Kantor Pusat** | Sekretariat, Kurikulum, Guru, Sarana, Server | `10.162.0.0` s/d `10.162.3.231` | **`10.162.0.0/22`** |
| **Kantor Cabang** | Pengawas Sekolah | `10.162.3.192` s/d `10.162.3.223`| **`10.162.3.192/27`** |


---

## 5. Detail Konfigurasi

-   **Default Gateway:** Alamat IP valid pertama dari setiap subnet digunakan sebagai IP interface router (Default Gateway).
-   **Host:** Alamat IP valid kedua (atau seterusnya) digunakan untuk host (PC/Server).
-   **DNS Server:** Semua host menggunakan IP dari `Server0` sebagai DNS Server.

### Contoh Konfigurasi (PC0 - Jaringan Sekretariat)
