Membuat environment container mysql-server yang fleksibel dan dinamis
====

## Motivasi

Untuk memudahkan dalam mengelola environment yang praktis dan apakah aplikasi dapat dijalankan dengan versi MySQL yang berbeda, disini kita akan membuat versi mysql yang dinamis dan praktis yang bisa kita pilih sesuai versi MySQL.

Saya membuatnya dengan meracik `docker-compose` untuk mempermudah pengujian apakah aplikasi dapat dimulai dengan versi yang berbeda.

## Yang Disupport MySQL version

- MySQL v5.5
- MySQL v5.6
- MySQL v5.7
- MySQL v8.0
- mariadb v10.0
- mariadb v10.1
- mariadb v10.2
- mariadb v10.3

## Penggunaan

### Install docker

https://docs.docker.com/engine/install

### Install mysql-client

`tidak butuh untuk menginstall mysql client` .

saat memanggil connect-xxx.sh, `mysql yang dipasang di container akan dicek dan dijalankan secara lokal` .

### Clone this repository

```bash
git clone git@github.com:treetips/docker-compose-all-mysql.git
```

### Jalankan mysql docker containers

```bash
$ docker-compose up -d
```

### Hubungkan mysql-server lainnya pada docker container

```bash
$ ./connect-mysql-5-5.sh
$ ./connect-mysql-5-6.sh
$ ./connect-mysql-5-6.sh
$ ./connect-mysql-8-0.sh
$ ./connect-mariadb-10-0.sh
$ ./connect-mariadb-10-1.sh
$ ./connect-mariadb-10-2.sh
$ ./connect-mariadb-10-3.sh
```

Tunggu startup MySQL selesai sebelum menghubungkan.

![waiting for running](docs/startup_mysql.gif)

## Optional

### Customize mysql client settings

```bash
vi ./my.cnf
```

Konfigurasikan pengaturan selain port yang sama.

### Customize mysql server settings

```bash
$ vi ./mysql5.5/conf.d/my.cnf
$ vi ./mysql5.6/conf.d/my.cnf
$ vi ./mysql5.7/conf.d/my.cnf
$ vi ./mysql8.0/conf.d/my.cnf
$ vi ./mariadb10.0/conf.d/my.cnf
$ vi ./mariadb10.1/conf.d/my.cnf
$ vi ./mariadb10.2/conf.d/my.cnf
$ vi ./mariadb10.3/conf.d/my.cnf
```

### Customize default schema, user, password

Jika Anda ingin mengubah skema DB, nama pengguna, kata sandi, atau kata sandi root, edit `.env`.

```yaml
DB_DATABASE=work
DB_USER=worker
DB_PASSWORD=worker
DB_ROOT_PASSWORD=root
```

### Remove logs

Jika Anda ingin menghapus `general.log` dan `error.log` dan `slow-query.log`, jalankan `clear_logs.sh` .

### Customize init scripts

https://hub.docker.com/_/mysql/#initializing-a-fresh-instance

#### Execution order at startup

1. `./common/initdb.d/common-init.(sh|sql)` (dieksekusi secara umum untuk semua kontainer)
1. `./(mysql|mariadb)X.X/initdb.d/xxx.(sh|sql)` (dieksekusi untuk setiap wadah)

Dengan menyesuaikan ini, data awal dapat diisi saat startup.