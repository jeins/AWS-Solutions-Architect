# Module 01 - Practice Quiz (27 Soal)

**AWS Fundamentals | SAA-C03 | Bahasa Indonesia**

> Quiz ini adalah adaptasi orisinal dari soal yang digunakan dalam sesi mentoring, mengikuti cakupan Module 01 dan menggunakan terminologi AWS. Ini **bukan soal ujian resmi AWS**.

**Aturan:** Pilih satu jawaban terbaik, kecuali nomor 12 yang meminta **dua** jawaban. Kerjakan dahulu seluruh 27 soal, lalu buka [ANSWER-KEY.md](ANSWER-KEY.md).

## Q1 - High Availability

Sebuah aplikasi web harus tetap tersedia meskipun satu data center AWS mengalami kegagalan. Strategi manakah yang paling proporsional terhadap syarat tersebut?

- A. Menempatkan backend hanya pada beberapa Edge Locations
- B. Menempatkan backend di beberapa Availability Zones dalam satu Region
- C. Selalu menggunakan beberapa Regions untuk setiap aplikasi
- D. Menempatkan seluruh workload pada satu Local Zone

## Q2 - Global Content Delivery

Sebuah website melayani pengguna di berbagai benua. Gambar dan file statis disimpan di S3. Solusi mana yang paling tepat untuk mempercepat pengambilan konten secara global?

- A. S3 dengan Cross-Region Replication
- B. CloudFront dengan S3 sebagai origin
- C. S3 Transfer Acceleration
- D. EC2 di setiap Region

## Q3 - Shared Responsibility

Dalam Shared Responsibility Model, komponen keamanan mana yang sepenuhnya menjadi tanggung jawab AWS?

- A. Pengaturan enkripsi data pelanggan di S3
- B. Patching guest operating system EC2
- C. Keamanan fisik pusat data AWS
- D. Konfigurasi EC2 Security Groups

## Q4 - Cloud Service Model

Startup ingin men-deploy aplikasi web konvensional pada platform yang mengabstraksi pengelolaan server, OS, dan runtime, seperti model platform deployment terkelola. Model layanan apakah yang paling sesuai?

- A. Infrastructure as a Service (IaaS)
- B. Platform as a Service (PaaS)
- C. Software as a Service (SaaS)
- D. Function as a Service (FaaS)

## Q5 - Data Residency

Perusahaan harus menjaga data workload tetap berada pada wilayah geografis yang disetujui regulator. Pendekatan dasar apa yang harus dilakukan?

- A. Mengaktifkan GuardDuty saja
- B. Memilih Region yang sesuai dan mengendalikan transfer lintas Region
- C. Mengaktifkan Organizations tanpa mengatur lokasi workload
- D. Mengaktifkan CloudTrail saja

## Q6 - Reliability Pillar

Dalam Well-Architected Framework, pilar mana yang menitikberatkan pemulihan dari kegagalan dan penyesuaian kapasitas untuk mempertahankan fungsi workload?

- A. Operational Excellence
- B. Security
- C. Reliability
- D. Performance Efficiency

## Q7 - Planned Cost Estimation

Sebelum membuat infrastruktur AWS, tim keuangan ingin memperkirakan pengeluaran berdasarkan instance, storage, dan kebutuhan data transfer. Tool apa yang paling sesuai?

- A. AWS Cost Explorer
- B. AWS Budgets
- C. AWS Pricing Calculator
- D. AWS Cost and Usage Report

## Q8 - Metropolitan Edge Computing

Aplikasi pemrosesan interaktif membutuhkan komputasi dengan latency sangat rendah bagi pengguna pada satu kawasan metropolitan. Tidak ada persyaratan integrasi khusus jaringan operator 5G. Infrastruktur yang paling cocok?

- A. AWS Region biasa
- B. Availability Zone biasa
- C. AWS Local Zone
- D. AWS Wavelength Zone

## Q9 - Multi-Account Management

Perusahaan ingin mengelola banyak AWS Accounts secara terpusat, termasuk pengelompokan akun dan kebijakan organisasi. Layanan utamanya adalah?

- A. AWS IAM
- B. AWS Organizations
- C. AWS Control Tower
- D. AWS Systems Manager

## Q10 - Security Design Principle

Manakah yang merupakan prinsip desain pada Security Pillar dari AWS Well-Architected Framework?

- A. Membangun fondasi identitas yang kuat
- B. Meluncurkan deployment global dalam hitungan menit
- C. Menghapus biaya operasional data center
- D. Membuat feedback loops untuk operasi

## Q11 - Temporary Credentials

Automation script menggunakan AWS CLI untuk menjalankan tugas administratif. Metode autentikasi mana yang lebih aman dibanding menaruh access keys jangka panjang di script?

- A. Menggunakan root credentials
- B. Menyimpan access key IAM User pada script
- C. Menggunakan IAM Role dengan temporary credentials
- D. Menggunakan access key ID tanpa secret key

## Q12 - Choosing an AWS Region (PILIH DUA)

Pilih **DUA** manfaat yang dapat diperoleh dengan memilih AWS Region yang sesuai untuk menempatkan workload.

- A. Potensi latency lebih rendah bagi pengguna dekat Region
- B. Replikasi otomatis ke semua Region
- C. Mendukung persyaratan data residency
- D. Multi-Region selalu lebih murah daripada satu Region
- E. Failover antar-Region otomatis tanpa konfigurasi

## Q13 - Design for Failure

Tim ingin menerapkan prinsip Design for Failure. Desain aplikasi seperti apa yang paling sesuai?

- A. Hanya memakai instance paling besar
- B. Menangani kegagalan komponen dengan recovery dan graceful degradation
- C. Menempatkan semua komponen pada satu AZ
- D. Menunggu AWS Support memperbaiki setiap gangguan

## Q14 - Edge Locations

Manakah pernyataan yang tepat tentang pemanfaatan AWS Edge Locations?

- A. Hanya bisa digunakan untuk caching CloudFront
- B. Jumlahnya selalu lebih sedikit daripada jumlah Regions
- C. Mendukung distribusi konten dan fungsi edge computing tertentu
- D. Memiliki fungsi yang identik dengan Availability Zones

## Q15 - Cost Threshold Alert

Tim ingin menerima notifikasi saat biaya AWS bulanan mencapai ambang anggaran tertentu. Layanan mana yang paling sesuai?

- A. AWS Cost Explorer
- B. AWS Budgets
- C. AWS Pricing Calculator
- D. AWS Trusted Advisor

## Q16 - EC2 Hypervisor Patching

Pada Amazon EC2, siapakah yang bertanggung jawab atas patch keamanan hypervisor yang mendasari virtual machine?

- A. Pelanggan AWS
- B. AWS
- C. AWS dan pelanggan secara bersama untuk patch yang sama
- D. Vendor aplikasi pihak ketiga

## Q17 - Telecom 5G Edge

Sebuah aplikasi mobile membutuhkan komputasi real-time yang terintegrasi dengan edge jaringan operator 5G. Infrastruktur AWS apa yang paling spesifik?

- A. AWS Local Zones
- B. AWS Wavelength
- C. AWS Outposts
- D. AWS Direct Connect

## Q18 - Performance Efficiency

Pilar apa yang paling berkaitan dengan prinsip menghindari perkiraan kapasitas manual melalui pengukuran, elastisitas, dan penyesuaian resource?

- A. Cost Optimization
- B. Performance Efficiency
- C. Reliability
- D. Operational Excellence

## Q19 - Consolidated Billing

Setiap tim developer memerlukan AWS Account terpisah, tetapi perusahaan ingin satu sistem penagihan yang terkonsolidasi. Apa yang paling tepat?

- A. AWS Control Tower saja
- B. AWS Organizations dengan Consolidated Billing
- C. Banyak IAM Users dalam satu akun
- D. AWS Resource Groups

## Q20 - Multiple Availability Zones

Apa tujuan utama penggunaan beberapa Availability Zones pada arsitektur dalam satu AWS Region?

- A. Menyediakan pilihan harga berdasarkan AZ
- B. Meningkatkan fault tolerance dan high availability
- C. Mengaktifkan jenis API berbeda per AZ
- D. Menghilangkan biaya transfer data antar-AZ

## Q21 - Centralized Governance

Sebuah organisasi memiliki puluhan AWS Accounts. Tim pusat ingin menggabungkan billing dan menerapkan kebijakan tingkat organisasi. Layanan mana yang tepat?

- A. AWS IAM
- B. AWS Organizations
- C. AWS Control Tower
- D. AWS Systems Manager

## Q22 - Restrict AWS Regions

Tim keamanan ingin mencegah member accounts membuat resource regional di luar dua Region yang disetujui. Kontrol pencegahan terpusat yang paling tepat adalah?

- A. IAM Policy terpisah di setiap member account
- B. AWS Config rule untuk mendeteksi ketidakpatuhan
- C. SCP dengan pembatasan `aws:RequestedRegion` dan pengecualian layanan yang sesuai
- D. AWS Firewall Manager untuk memblokir semua Region

## Q23 - Nature of SCP

Manakah pernyataan yang tepat tentang Service Control Policies pada AWS Organizations?

- A. SCP memberikan IAM permission secara langsung
- B. SCP membatasi identitas pada management account
- C. SCP menentukan batas maksimum izin pada member accounts
- D. SCP hanya dapat dipasang pada satu account, bukan OU

## Q24 - Landing Zone

Perusahaan membutuhkan landing zone multi-account, account provisioning yang terstandar, dan governance controls siap pakai. Pilihan layanan paling sesuai?

- A. AWS Organizations tanpa tambahan konfigurasi
- B. AWS Control Tower
- C. CloudFormation StackSets saja
- D. AWS Service Catalog saja

## Q25 - VPC Subnet Sharing

Networking Account memiliki VPC dan subnet. Application Accounts perlu membuat EC2 pada subnet yang sama tanpa menggandakan infrastruktur jaringan. Layanan yang digunakan?

- A. VPC Peering
- B. AWS Transit Gateway
- C. AWS Resource Access Manager (RAM)
- D. AWS PrivateLink

## Q26 - Volume Discounts

Beberapa akun dalam satu AWS Organization memenuhi ketentuan diskon penggunaan gabungan untuk layanan tertentu, meskipun masing-masing akun tidak mencapai ambang volume sendiri. Mengapa?

- A. Setiap organisasi mendapat diskon flat otomatis
- B. Consolidated Billing menggabungkan penggunaan yang memenuhi syarat
- C. Seluruh diskon hanya berlaku untuk Management Account
- D. SCP otomatis mengaktifkan potongan harga

## Q27 - Leave Organization

Agar member accounts tidak dapat keluar dari AWS Organization dengan menjalankan tindakan `LeaveOrganization`, kontrol organisasi apa yang dapat membatasi tindakan tersebut?

- A. IAM Policy biasa pada Management Account
- B. Service Control Policy (SCP)
- C. Resource Control Policy (RCP)
- D. Organizational Lock

---

## Lembar jawaban kosong

```text
01: __   02: __   03: __   04: __   05: __   06: __   07: __
08: __   09: __   10: __   11: __   12: __+__ 13: __   14: __
15: __   16: __   17: __   18: __   19: __   20: __   21: __
22: __   23: __   24: __   25: __   26: __   27: __
```

**Lanjut:** [Kunci dan pembahasan](ANSWER-KEY.md).
