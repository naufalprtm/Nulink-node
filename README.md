# Nulink-node
Node Tutorial
# Pemasangan NuLink Worker

Ikhtisar

NuLink Worker adalah node yang menyediakan layanan kriptografi dalam jaringan NuLink. Ini menyediakan layanan Proxy Re-encryption di jaringan Horus, dan akan menyediakan lebih banyak layanan seperti ABE, IBE, ZKP, dan FHE di NuLink mainnet. Staker perlu menjalankan node Worker untuk memenuhi syarat mendapatkan token sebagai imbalan.

Ada empat langkah untuk menjalankan NuLink Worker:

Buat Akun Worker.
Instal NuLink Worker.
Konfigurasi dan Jalankan node Worker.
Ikatkan node Worker dengan akun staking Anda.

# Persyaratan Sistem Minimum

Debian/Ubuntu (Recommended)
30GB available storage
4GB RAM
x86 architecture
Static IP address
Exposed TCP port 9151, make sure it's not occupied
Nodes can be run on cloud infrastructure.


# Instalasi

Pembaruan Dependensi:

```
sudo apt update && sudo apt upgrade -y
sudo apt install ufw

```
# Instal Docker Terbaru dan Tarik Gambar Horus Terbaru:
```
curl -fsSL https://get.docker.com | sh
sudo usermod -aG docker $USER
docker pull nulink/nulink:latest

```
# Penyiapan Firewall:
```
sudo ufw allow ssh
sudo ufw allow 9151/tcp
sudo ufw enable

```
# Menyiapkan Variabel Password:
Ubah keduanya dengan kata sandi yang Anda inginkan.
```
export NULINK_KEYSTORE_PASSWORD=PASSWORD SIMPAN NULINK ANDA
export NULINK_OPERATOR_ETH_PASSWORD=PASSWORD AKUN WORKER ANDA

```
# Buat Akun Worker:
Download GETH dan Ekstrak:
```
wget https://gethstore.blob.core.windows.net/builds/geth-linux-amd64-1.10.23-d901d853.tar.gz && tar -xvzf geth-linux-amd64-1.10.23-d901d853.tar.gz

```
# Ganti nama direktori:
Anda akan diminta untuk memasukkan dan mengonfirmasi kata sandi. Ingatlah kata sandi ini untuk penggunaan selanjutnya.
```
./geth account new --keystore ./keystore

```
# Buat Direktori NuLink:
```
cd $HOME
sudo mkdir nulink
cp $HOME/geth/keystore/* $HOME/nulink
sudo chmod -R 777 $HOME/nulink

```
# Inisialisasi Worker:
Ubah --signer dan --operator-address sesuai dengan milik Anda.
--signer keystore:///code/UTC--2022-09-13T01-14-32.465358210Z--8b1819341bec211a45a2186c4d0030681cccXXXX \
--operator-address <Alamat Operator> \
```
docker run -it --rm \
-p 9151:9151 \
-v $HOME/nulink:/code \
-v $HOME/nulink:/home/circleci/.local/share/nulink \
-e NULINK_KEYSTORE_PASSWORD \
nulink/nulink nulink ursula init \
--signer keystore:///code/UTC--2022-09-13T01-14-32.465358210Z--8b1819341bec211a45a2186c4d0030681cccXXXX \
--eth-provider https://data-seed-prebsc-2-s2.binance.org:8545 \
--network horus \
--payment-provider https://data-seed-prebsc-2-s2.binance.org:8545 \
--payment-network bsc_testnet \
--operator-address <Alamat Operator> \
--max-gas-price 10000000000

```
# Bond Worker
Kunjungi https://dashboard.testnet.nulink.org/.

Pastikan untuk menggunakan dompet yang berbeda, jangan gunakan dompet worker Anda!

Staking NLK:

Dapatkan beberapa tNLK dan tBNB. Anda bisa meminta tNLK dan tBNB seperti yang ditunjukkan pada gambar di bawah:

Setelah menerima tNLK dan tBNB, masuk ke halaman Staking dan lakukan staking beberapa NLK. Minimal staking adalah 1 NLK. Setujui transaksi jika diminta.

Jika ini adalah pertama kalinya, Anda akan diminta untuk menyetujui pengeluaran token terlebih dahulu.

Setelah itu, lakukan staking pada tNLK Anda.

Ikatkan Worker:

Gulir ke bawah dan Anda akan melihat Informasi Node.
Klik pada tombol "Bond Worker" dan masukkan Alamat Operator yang dihasilkan pada langkah Membuat Akun Worker. Setujui transaksi.
Tunggu beberapa menit dan segarkan halaman. Proses ikatan sekarang seharusnya selesai.


# jika ingin Melakukan Update Worker
```
docker kill ursula
docker rm ursula
docker pull nulink/nulink:latest
```
# Relaunch worker
```
docker run --restart on-failure -d \
--name ursula \
-p 9151:9151 \
-v $HOME/nulink:/code \
-v $HOME/nulink:/home/circleci/.local/share/nulink \
-e NULINK_KEYSTORE_PASSWORD \
-e NULINK_OPERATOR_ETH_PASSWORD \
nulink/nulink nulink ursula run --no-block-until-ready
```

































