# Module 01 - Answer Key dan Penjelasan Singkat

**Untuk:** [Quiz 27 soal](QUIZ.md). Buka dokumen ini setelah menyelesaikan quiz.

> Jawaban di bawah adalah kunci untuk **soal adaptasi** dalam repository ini, bukan klaim bahwa urutan nomor/opsinya identik dengan kunci repository sumber. Semua pilihan dikaji berdasarkan premis yang tertulis pada `QUIZ.md`.

| No. | Kunci | Penjelasan singkat |
|---|---|---|
| 1 | **B** | Multi-AZ mengurangi risiko kegagalan satu AZ/data center; Multi-Region lebih kompleks dari syarat soal. |
| 2 | **B** | CloudFront meng-cache objek dekat pengguna, menggunakan S3 sebagai origin. |
| 3 | **C** | Keamanan fisik data center adalah tanggung jawab AWS. |
| 4 | **B** | PaaS mengabstraksi pengelolaan runtime/infrastruktur untuk deployment aplikasi terkelola. |
| 5 | **B** | Region dan pengendalian replikasi/transfer data berkaitan langsung dengan data residency. |
| 6 | **C** | Reliability berfokus pada ketahanan, pemulihan kegagalan, dan penyesuaian kapasitas. |
| 7 | **C** | Pricing Calculator memodelkan biaya sebelum deployment; Cost Explorer menganalisis spend/usage historis. |
| 8 | **C** | Local Zones cocok untuk komputasi dekat area metropolitan; Wavelength terkait edge operator 5G. |
| 9 | **B** | AWS Organizations mengatur banyak akun, OU, serta organization policies. |
| 10 | **A** | Strong identity foundation termasuk Security Pillar. |
| 11 | **C** | IAM Role menyediakan temporary credentials tanpa menyimpan secret permanen dalam script. |
| 12 | **A + C** | Region dekat pengguna dapat membantu latency; pemilihan Region mendukung data residency. |
| 13 | **B** | Design for Failure menuntut recovery dan degradasi yang terkendali ketika komponen gagal. |
| 14 | **C** | Edge network mendukung CDN dan beberapa kemampuan komputasi edge, tidak identik dengan AZ. |
| 15 | **B** | AWS Budgets digunakan untuk alert pada threshold anggaran. |
| 16 | **B** | AWS mengelola patching hypervisor yang mendasari EC2; guest OS umumnya pelanggan. |
| 17 | **B** | AWS Wavelength berfokus pada edge komputasi dalam jaringan telekomunikasi. |
| 18 | **B** | Performance Efficiency mengevaluasi pemanfaatan resource dan kemampuan merespons demand. |
| 19 | **B** | Organizations memberikan consolidated billing antar member accounts. |
| 20 | **B** | Beberapa AZ meningkatkan high availability dan fault isolation dalam Region. |
| 21 | **B** | Organizations adalah pondasi multi-account billing dan governance. |
| 22 | **C** | SCP dapat menjadi guardrail preventif Region, dengan penanganan layanan global dan pengecualian API yang tepat. |
| 23 | **C** | SCP membatasi izin maksimum yang dapat digunakan principal dalam member accounts. |
| 24 | **B** | Control Tower menyediakan landing zone dan governance baseline/account provisioning. |
| 25 | **C** | RAM mendukung pembagian subnet VPC tertentu lintas account. |
| 26 | **B** | Consolidated billing menggabungkan usage yang memenuhi syarat untuk manfaat pricing tertentu. |
| 27 | **B** | SCP dapat melarang action `organizations:LeaveOrganization` pada member accounts. |

## Pendalaman dua jebakan layanan

### Q7 - Pricing Calculator vs Cost Explorer

- **Pricing Calculator:** estimasi sebelum deployment atau *what-if cost modeling*. Kita menentukan contoh instance, jam operasional, storage, dan transfer data.
- **Cost Explorer:** telaah penggunaan dan biaya yang sudah tercatat, tren, breakdown per layanan/akun, serta forecast dari data historis.
- **AWS Budgets:** notifikasi ketika belanja atau penggunaan mencapai threshold.
- **Cost and Usage Report / Data Exports:** data billing granular untuk analisis lanjut.

**Contoh praktis:** Tim akan men-deploy dua EC2 Spring Boot, satu ALB, RDS PostgreSQL Multi-AZ, dan S3. Biaya rencana dihitung menggunakan Pricing Calculator. Setelah beberapa bulan, biaya RDS melampaui asumsi; Cost Explorer membantu mengidentifikasi kenaikannya. Untuk alert batas belanja, gunakan Budgets.

**Jebakan:** Cost Explorer juga mendukung forecast, tetapi forecast tersebut bukan pengganti penuh untuk pemodelan konfigurasi arsitektur baru sebelum deployment.

### Q8 - Local Zones vs Wavelength

- **Local Zones:** extension dari Region ke lokasi geografis lebih dekat untuk memenuhi kebutuhan latency metropolitan tertentu.
- **Wavelength:** komputasi di dekat edge jaringan operator seluler yang mendukung layanan tersebut.

**Contoh praktis:** Backend real-time rendering untuk pengguna kota tertentu dapat mempertimbangkan Local Zones apabila tersedia. Game/mobile AR yang membutuhkan jalur 5G operator berlatensi sangat rendah dan tersedia secara lokal dapat mempertimbangkan Wavelength. Keduanya bukan solusi otomatis untuk high availability.

## Perbandingan yang sering tertukar

| Topik | Pilihan yang paling erat dengan persyaratan |
|---|---|
| AWS API audit | CloudTrail |
| Metrics, alarm, dashboard | CloudWatch |
| Configuration compliance | AWS Config |
| Managed relational transactions | RDS/Aurora |
| Warehouse analytics | Redshift |
| Cloud storage untuk objek | S3 |
| Block storage EC2 | EBS |
| Shared filesystem | EFS |
| Multi-account governance foundation | Organizations |
| Managed landing zone + account factory | Control Tower |
| Cross-account resource sharing | RAM |

## Mengapa opsi lain kurang tepat? (ringkasan prinsip)

1. **Soal 1:** Edge Locations mengoptimalkan content delivery, bukan penempatan backend Multi-AZ; Multi-Region dapat memenuhi resilience lebih luas tetapi tidak proporsional untuk persyaratan soal.
2. **Soal 3 dan 16:** AWS bertanggung jawab atas infrastruktur fisik dan hypervisor; pelanggan tetap bertanggung jawab atas guest OS pada EC2 serta konfigurasi akses.
3. **Soal 5:** GuardDuty mendeteksi ancaman dan CloudTrail mencatat aktivitas API, bukan secara otomatis memberlakukan lokasi data.
4. **Soal 11:** Root keys dan secret jangka panjang yang disimpan dalam script menambah risiko; IAM Role + temporary credentials meminimalkan masalah tersebut.
5. **Soal 22:** AWS Config bersifat evaluatif/detective pada skenario ini, sedangkan SCP dapat menolak API call yang tidak diizinkan; pembatasan `aws:RequestedRegion` harus dirancang hati-hati untuk API global dan lintas-Region.
6. **Soal 23:** SCP tidak memberikan izin. Explicit Deny pada SCP mengalahkan Allow dari IAM untuk principal yang terkena SCP; management account dan service-linked roles memiliki pengecualian.

## Referensi

- [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)
- [AWS Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/)
- [AWS Organizations SCP](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html)
- [AWS Pricing Calculator](https://calculator.aws/)
- [AWS Cost Explorer](https://docs.aws.amazon.com/cost-management/latest/userguide/ce-what-is.html)
