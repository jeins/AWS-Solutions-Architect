# Module 01 - AWS Fundamentals

**Jalur:** AWS Certified Solutions Architect - Associate (SAA-C03)  
**Bahasa:** Indonesia, istilah teknis AWS tetap dalam bahasa Inggris  
**Target pembaca:** Software Engineer / Senior Backend Developer (Java & Spring Boot)  
**Bentuk:** Ringkasan pembelajaran komprehensif, bukan terjemahan literal repository sumber.

## Tujuan pembelajaran

Setelah modul ini, pembaca diharapkan mampu:

1. Memilih Region, Availability Zone, Edge Location, Local Zone, atau Wavelength berdasarkan kebutuhan.
2. Membedakan Console, CLI, SDK, CloudFormation, dan CloudShell.
3. Menerapkan keenam pilar AWS Well-Architected Framework.
4. Menentukan batas tanggung jawab AWS dan pelanggan pada EC2, RDS, dan S3.
5. Memahami AWS Accounts, Organizations, OUs, SCP, Control Tower, RAM, dan cost management.
6. Memilih layanan compute, storage, database, networking, security, dan monitoring yang sesuai.

**Navigasi:** [Quiz 27 soal](QUIZ.md) | [Kunci dan alasan](ANSWER-KEY.md) | [Contoh arsitektur](ARCHITECTURE.md)

---

## 1. AWS Global Infrastructure

### 1.1 AWS Region

*Region* adalah area geografis yang menjadi lokasi layanan AWS. Region memuat beberapa Availability Zones (AZ). Contoh kode Region: `ap-southeast-3` (Jakarta), `eu-central-1` (Frankfurt), dan `us-east-1` (N. Virginia).

Hal yang dipertimbangkan ketika memilih Region:

| Pertimbangan | Implikasi |
|---|---|
| Data residency dan compliance | Pastikan lokasi penyimpanan/pemrosesan memenuhi kewajiban regulasi dan kebijakan organisasi. |
| Latency | Region yang dekat dengan pengguna sering membantu, tetapi tetap ukur jalur jaringan sebenarnya. |
| Ketersediaan layanan | Tidak semua layanan, tipe instance, dan fitur tersedia di setiap Region. |
| Biaya | Harga compute, storage, dan data transfer berbeda menurut Region. |
| Disaster recovery | Strategi pemulihan dapat melibatkan Region kedua dan replikasi eksplisit. |

**Catatan penting:** Memilih Region tidak otomatis menjamin seluruh aliran data tetap berada di sana. Audit konfigurasi replikasi, endpoint, log, backup, dan layanan global agar sesuai ketentuan.

### 1.2 Availability Zone (AZ)

Satu AZ terdiri atas satu atau lebih data center yang dirancang memiliki isolasi kegagalan dari AZ lain dalam Region yang sama. AZ memiliki infrastruktur daya, pendingin, dan konektivitas independen, serta terhubung dengan AZ lain melalui jaringan berlatensi rendah.

**Contoh:** Spring Boot ditempatkan pada dua AZ di belakang Application Load Balancer (ALB). Ketika target pada AZ A gagal, ALB dapat meneruskan request ke target sehat di AZ B. Arsitektur ini masih perlu memastikan AZ B cukup kapasitas, database mendukung failover, dan data tidak disimpan secara eksklusif pada disk lokal salah satu instance.

- **Single-AZ**: sederhana, murah, tetapi memiliki risiko lebih besar akibat kegagalan satu AZ.
- **Multi-AZ**: mendukung high availability pada level AZ, biasanya membutuhkan lebih banyak resource dan konfigurasi.
- **Multi-Region**: dipertimbangkan untuk pemulihan terhadap gangguan skala Region, data sovereignty, atau kebutuhan global tertentu; menambah kompleksitas data dan operasi.

Multi-AZ bukan sinonim multi-region. Dalam pertanyaan yang hanya menyebut kegagalan satu AZ/data center, Multi-AZ biasanya pilihan yang proporsional.

### 1.3 Edge Locations dan CloudFront

Edge Locations adalah lokasi jaringan AWS yang digunakan oleh layanan edge, termasuk Amazon CloudFront. CDN mengurangi latency dengan menyimpan cache konten lebih dekat ke pengguna.

**Contoh:** Produk e-commerce menyimpan foto di S3 sebagai *origin*, lalu CloudFront mendistribusikan gambar ke pengguna global. Aplikasi Spring Boot yang membuat transaksi tidak otomatis berpindah ke Edge Location. Konten dinamis dan API tetap memerlukan rancangan origin yang tepat.

| Teknologi | Fokus |
|---|---|
| CloudFront/Edge Locations | Distribusi konten, caching, dan fungsi tertentu di edge |
| Region | Lokasi utama workload dan layanan regional |
| Availability Zone | Isolasi kegagalan infrastruktur dalam satu Region |

### 1.4 Local Zones

AWS Local Zones merupakan perluasan infrastruktur suatu Region ke lokasi geografis yang lebih dekat dengan area pengguna, terutama ketika workload membutuhkan latensi rendah. Layanan yang tersedia dan pilihan deployment harus dicek untuk Local Zone terkait.

### 1.5 Wavelength Zones

AWS Wavelength dirancang untuk workload yang membutuhkan kedekatan dengan jaringan operator telekomunikasi/5G. Contoh kasus: pemrosesan real-time dari perangkat mobile melalui jaringan operator yang mendukung Wavelength.

**Pengingat ujian:** Jika fokusnya pengguna di area metropolitan tanpa syarat integrasi 5G operator, pertimbangkan Local Zones. Jika disebut *telecom 5G edge*, pertimbangkan Wavelength.

---

## 2. AWS Management Tools

### 2.1 AWS Management Console

Console menyediakan antarmuka web untuk menelusuri dan mengonfigurasi layanan. Cocok untuk eksplorasi, troubleshooting, dan tindakan sesekali. Perubahan manual yang terus-menerus berisiko menyebabkan *configuration drift* dibanding deployment terotomasi.

### 2.2 AWS Command Line Interface (CLI)

AWS CLI dipakai untuk melakukan operasi administratif dari terminal dan automation. CLI mengakses API AWS dan membutuhkan kredensial serta izin yang benar.

```bash
aws sts get-caller-identity
aws configure list-profiles
aws s3 ls --profile dev
```

Praktik baik: gunakan IAM Identity Center atau temporary credentials dengan profile, jangan hardcode access key jangka panjang di script atau commit ke Git.

### 2.3 AWS SDK for Java

AWS SDK digunakan aplikasi untuk memanggil API AWS. Backend Java dapat menggunakan SDK for Java v2 dan *default credentials provider chain*, yang secara otomatis dapat memperoleh credentials dari environment yang didukung, termasuk IAM Role di compute AWS.

```java
import software.amazon.awssdk.services.s3.S3Client;

try (S3Client s3 = S3Client.create()) {
    s3.listBuckets().buckets()
      .forEach(bucket -> System.out.println(bucket.name()));
}
```

Contoh hanya ilustrasi dan membutuhkan dependency SDK, Region resolution, network, serta izin `s3:ListAllMyBuckets` yang sesuai. Hindari menyimpan `accessKeyId` dan `secretAccessKey` langsung di kode.

### 2.4 AWS CloudFormation

CloudFormation adalah layanan *Infrastructure as Code (IaC)* AWS. Resource didefinisikan dalam template YAML/JSON, lalu dikelola sebagai *stack*.

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: S3 bucket example; name generated by AWS
Resources:
  DocumentsBucket:
    Type: AWS::S3::Bucket
    Properties:
      PublicAccessBlockConfiguration:
        BlockPublicAcls: true
        IgnorePublicAcls: true
        BlockPublicPolicy: true
        RestrictPublicBuckets: true
Outputs:
  BucketName:
    Value: !Ref DocumentsBucket
```

Keuntungan: perubahan dapat direview, versioned, direplikasi ke environment lain, dan diperiksa sebelum deployment. *Change sets* membantu melihat perubahan yang direncanakan. Sebagian pembaruan resource dapat memicu *replacement*; rollback tidak selalu menghilangkan konsekuensi data sehingga perlu kebijakan perlindungan dan backup.

### 2.5 AWS CloudShell dan Systems Manager

- **CloudShell:** terminal berbasis browser untuk tugas CLI, menggunakan konteks autentikasi AWS Console yang didukung.
- **Systems Manager:** rangkaian fitur operasional seperti Session Manager, Run Command, Patch Manager, dan Parameter Store untuk mengelola resource.

CloudShell adalah tempat menjalankan perintah; Systems Manager adalah layanan operasi dan manajemen fleet/resource. Keduanya bukan sinonim.

---

## 3. AWS Well-Architected Framework

Framework ini membantu mengevaluasi desain dan operasi workload dengan enam pilar. Masing-masing pilar mempunyai trade-off; tidak ada satu layanan yang otomatis menjamin seluruh pilar terpenuhi.

| Pilar | Fokus | Contoh keputusan pada Spring Boot |
|---|---|---|
| Operational Excellence | Operasi sebagai kode, perubahan kecil, observability, belajar dari insiden | Pipeline CI/CD, runbook, rollback, structured logs |
| Security | Identitas, traceability, defense in depth, perlindungan data | IAM roles, least privilege, encryption, audit |
| Reliability | Recovery, HA, fault isolation, scaling | Multi-AZ, ALB health checks, database failover |
| Performance Efficiency | Penggunaan resource yang tepat dan respons terhadap perubahan demand | Benchmark query, connection pool, caching, load test |
| Cost Optimization | Nilai bisnis per biaya dan pengendalian pengeluaran | Rightsizing, pricing model, usage tagging |
| Sustainability | Efisiensi energi dan pemanfaatan resource | Autoscaling, managed services, optimasi utilization |

### 3.1 Operational Excellence

Perlakukan operasi sebagai kode: infrastructure definition, deployment pipeline, alarm, dan runbook sebaiknya reproducible. Hindari perubahan manual yang tidak terdokumentasi di production. Lakukan postmortem tanpa fokus pada kesalahan personal agar pencegahan lebih kuat.

### 3.2 Security

Gunakan identitas yang kuat, least privilege, pemisahan lingkungan, logging dan monitoring, encryption in transit/at rest, serta rencana incident response. Kunci enkripsi dan kebijakan akses tetap perlu dikonfigurasi sesuai kebutuhan.

### 3.3 Reliability

Desain untuk mengatasi kegagalan komponen. Multi-AZ dan Auto Scaling adalah alat, bukan jaminan; backup, testing, dependency failure, dan capacity reserve tetap diperlukan.

### 3.4 Performance Efficiency

Pilih kapasitas berdasarkan metrics, bukan asumsi. Contoh: sebelum memperbesar RDS, cek indeks PostgreSQL, query plans, connection pooling, dan traffic characteristics.

### 3.5 Cost Optimization

Hitung biaya rencana dengan Pricing Calculator dan tinjau biaya aktual dengan Cost Explorer. Pertimbangkan Savings Plans/Reserved Instances sesuai karakter penggunaan dan syarat produk.

### 3.6 Sustainability

Kurangi resource idle, maksimalkan utilisasi yang wajar, dan pertimbangkan managed/serverless service apabila sesuai kebutuhan. Hindari optimasi energi yang merusak reliability atau SLA.

**Ujian:** Reliability berfokus pada pulih dari kegagalan; Performance Efficiency pada pemanfaatan resource untuk memenuhi demand; Operational Excellence pada kualitas operasi dan perubahan; Cost Optimization pada biaya relatif terhadap nilai.

---

## 4. AWS Shared Responsibility Model

AWS menangani **security OF the cloud**, sedangkan pelanggan bertanggung jawab atas **security IN the cloud**. Batas detail berubah sesuai model layanan.

| Aspek | AWS | Pelanggan |
|---|---|---|
| Fasilitas fisik data center | Ya | Tidak |
| Hypervisor/host EC2 | Ya | Tidak |
| Guest OS EC2 dan patch | Tidak | Ya |
| Security Group, IAM permissions | Infrastruktur kontrol | Konfigurasi dan penggunaan |
| OS terkelola RDS | Ya | Tidak |
| Pengguna DB, schema, akses network RDS | Tidak | Ya |
| Infrastruktur S3 | Ya | Tidak |
| Bucket policies, klasifikasi object, pengaturan akses | Tidak | Ya |

**Contoh:** AWS menambal hypervisor EC2, tetapi pelanggan harus mengelola patch guest OS EC2. Pada Amazon RDS, AWS mengelola lapisan OS dan aspek patching layanan, sementara pelanggan tetap menentukan akses, autentikasi, dan strategi perlindungan data.

Bantahan terhadap miskonsepsi: menggunakan managed service bukan berarti seluruh tanggung jawab keamanan berpindah ke AWS.

---

## 5. AWS Account Management

### 5.1 AWS Account dan Root User

Satu AWS Account menyediakan batas administrasi, billing, dan isolasi resource. Root User adalah identitas sangat sensitif dengan kemampuan luas untuk akun tersebut. Jangan gunakan root untuk kegiatan harian; aktifkan MFA dan amankan recovery contact. Untuk akses manusia sehari-hari, AWS merekomendasikan federated access/IAM Identity Center dengan temporary credentials bila memungkinkan.

### 5.2 AWS Organizations dan Organizational Unit

AWS Organizations mengelola banyak akun secara terpusat. Accounts dapat dikelompokkan ke dalam Organizational Units (OUs) dan diberi guardrails berbeda untuk Production, Development, Security, dan Sandbox.

```text
AWS Organization
+-- Management Account (billing / governance)
+-- Production OU
|   +-- App Account
|   +-- Data Account
+-- Development OU
|   +-- Dev Account
+-- Security OU
    +-- Log Archive Account
```

**Consolidated billing** dapat menggabungkan biaya berbagai member accounts dan, jika memenuhi ketentuan, mendukung pricing benefits tertentu. Pemisahan akun tetap bermanfaat untuk isolasi blast radius dan tugas tim.

### 5.3 Service Control Policies (SCPs)

SCP adalah permission guardrail tingkat organisasi. SCP dapat dipasang pada organization root, OU, atau member account, dan diwariskan mengikuti hierarki. **SCP tidak memberikan permission**, hanya membatasi ruang maksimum yang mungkin diizinkan oleh kebijakan lain.

Contoh: developer memakai IAM Role dengan `AdministratorAccess`, tetapi Development OU memiliki SCP berikut:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyTerminateEC2",
      "Effect": "Deny",
      "Action": "ec2:TerminateInstances",
      "Resource": "*"
    }
  ]
}
```

Akibatnya, `ec2:TerminateInstances` ditolak untuk principal yang dicakup SCP, meskipun IAM mengizinkan tindakan tersebut. `AdministratorAccess` bukan akses tanpa batas.

**Pengecualian penting:** SCP tidak membatasi identitas pada management account dan tidak membatasi AWS service-linked roles. Guardrail tingkat organisasi harus diuji agar tidak menghambat deployment dan operasi sah. Untuk pembatasan Region menggunakan `aws:RequestedRegion`, perhatikan layanan global dan API lintas-Region; jangan menyalin policy contoh tanpa pengecualian dan verifikasi.

### 5.4 AWS Control Tower

AWS Control Tower membantu membuat dan mengelola *landing zone* multi-account di atas layanan seperti Organizations. Fitur meliputi account provisioning (Account Factory), governance controls, dan baseline yang lebih terstandar.

**Perbandingan:** Organizations memberi pondasi pengelolaan akun/kebijakan; Control Tower memberi opinionated setup dan governance workflow untuk mempercepat pengelolaan multi-account.

### 5.5 AWS Resource Access Manager (RAM)

AWS RAM memungkinkan jenis resource AWS tertentu dibagikan ke account lain. Misalnya, Networking Account dapat membagikan VPC subnet yang memenuhi persyaratan kepada Application Account sehingga EC2 aplikasi menggunakan jaringan bersama namun ownership resource aplikasi tetap pada akun aplikasi. Tidak semua jenis resource dapat dibagikan; periksa daftar layanan yang didukung.

### 5.6 Billing dan Cost Management

| Alat | Tujuan |
|---|---|
| AWS Pricing Calculator | Merencanakan dan memodelkan estimasi biaya workload/arsitektur |
| AWS Cost Explorer | Menganalisis biaya, usage, dan tren; membuat forecast berbasis riwayat |
| AWS Budgets | Menentukan budget dan alert terhadap threshold |
| AWS Cost and Usage Report / Data Exports | Data granular untuk analisis, audit, dan BI |
| AWS Cost Anomaly Detection | Mendeteksi pola biaya tidak biasa dan memberi alert |

**Contoh:** Sebelum menjalankan dua instance Spring Boot, ALB, RDS Multi-AZ, dan S3, gunakan Pricing Calculator. Setelah sistem berjalan, gunakan Cost Explorer untuk mencari penyebab perbedaan antara perkiraan dan biaya aktual. Harga serta cakupan Free Tier AWS dapat berubah; periksa ketentuan pada akun dan tanggal yang relevan.

---

## 6. AWS Service Categories

Memilih layanan harus dimulai dari kebutuhan, bukan dari familiaritas nama produk.

### 6.1 Compute

| Layanan | Gunakan ketika... | Trade-off |
|---|---|---|
| EC2 | Membutuhkan kontrol server/OS/runtime | Patching dan operasi guest OS |
| Lambda | Memproses fungsi berbasis event | Batas eksekusi, cold start, desain stateless |
| ECS | Menjalankan container tanpa harus mengelola Kubernetes | Perlu desain task, service, networking, scaling |
| EKS | Organisasi membutuhkan Kubernetes | Kompleksitas operasional Kubernetes |
| Elastic Beanstalk | Ingin penyederhanaan deployment aplikasi konvensional | Fleksibilitas dan kontrol berbeda dari setup manual |

ECS adalah orchestrator container. AWS Fargate adalah opsi kapasitas compute yang dapat dipakai bersama ECS/EKS.

### 6.2 Storage

| Layanan | Jenis | Kasus |
|---|---|---|
| S3 | Object storage | PDF invoice, gambar, object backup |
| EBS | Block storage | Disk EC2 |
| EFS | Shared filesystem | File bersama banyak Linux instance |
| S3 Glacier storage classes | Archival | Data jarang diakses, perhatikan retrieval |

S3 bukan disk POSIX biasa. Simpan object di S3 dan metadata transaksional di PostgreSQL bila desain mendukung.

### 6.3 Database

| Layanan | Model | Cocok untuk |
|---|---|---|
| RDS | Managed relational | PostgreSQL/MySQL dan transaksi SQL |
| Aurora | AWS-managed relational | Kebutuhan relational dengan fitur storage/scaling Aurora |
| DynamoDB | Key-value/document NoSQL | Pola akses terdefinisi, latency dan scaling tertentu |
| Redshift | Data warehouse | Analytics dan pelaporan skala besar |

Jangan memilih DynamoDB sekadar karena kata 'scalability'; periksa access patterns dan kebutuhan JOIN/transaction. Jangan memilih Redshift sebagai pengganti langsung RDS OLTP.

### 6.4 Networking

- **Amazon VPC:** ruang jaringan virtual untuk subnet, routing, dan kontrol konektivitas.
- **Route 53:** DNS dan routing berbasis kebijakan DNS/health check.
- **CloudFront:** CDN dan edge caching.
- **Direct Connect:** koneksi jaringan dedicated ke AWS.

Public/private subnet ditentukan oleh routing dan akses jaringan, bukan sekadar nama subnet. Database transaction biasanya ditempatkan pada private subnet dan hanya menerima koneksi yang diperlukan.

### 6.5 Security dan Identity

- **IAM:** mengendalikan principal dan permissions AWS.
- **KMS:** mengelola keys dan operasi kriptografi melalui AWS KMS.
- **WAF:** inspeksi/filter HTTP(S) pada supported resources.
- **GuardDuty:** threat detection berbasis telemetry dan analitik.

WAF bukan pengganti validasi input atau parameterized SQL queries. GuardDuty bukan sumber tunggal audit API dan bukan firewall universal.

### 6.6 Management dan Monitoring

| Kebutuhan | Layanan utama |
|---|---|
| Metrics, log, alarm, operational dashboards | CloudWatch |
| Audit aktivitas AWS API | CloudTrail |
| Configuration history dan compliance resource | AWS Config |
| Operasi fleet, Session Manager, patching | Systems Manager |

Untuk EC2 memory metrics tertentu, CloudWatch Agent perlu dikonfigurasi karena tidak seluruh OS metrics tersedia sebagai default EC2 metrics. S3 object-level data events pada CloudTrail perlu diaktifkan ketika diperlukan; tidak otomatis tercatat dalam Event History management events.

---

## 7. Exam Tips dan Pola Pemilihan Layanan

| Frasa pada scenario | Kandidat paling relevan |
|---|---|
| Failure satu AZ/data center | Multi-AZ, dengan health checks dan kapasitas tersedia |
| Failure satu Region | Multi-Region DR, jika RTO/RPO mensyaratkan |
| Static global content, low latency | CloudFront |
| Telco 5G edge computing | Wavelength |
| Low-latency metropolitan compute | Local Zones |
| Automated/reproducible AWS infrastructure | CloudFormation |
| AWS API audit | CloudTrail |
| Metrics dan alerts | CloudWatch |
| Configuration compliance | AWS Config |
| Estimate planned deployment cost | AWS Pricing Calculator |
| Analyze historical AWS spending | Cost Explorer |
| Notify on cost threshold | AWS Budgets |
| Multi-account permission guardrail | SCP melalui Organizations |
| Ready-made multi-account landing zone | Control Tower |
| Cross-account share of supported subnet | RAM |

**Cara menganalisis soal:** identifikasi kebutuhan wajib, bedakan 'can' vs 'best', pilih layanan yang memenuhi seluruh syarat dengan kompleksitas dan biaya proporsional, lalu eliminasi pilihan yang hanya menyelesaikan sebagian masalah.

## 8. Latihan akhir modul

- [Kerjakan Quiz 27 soal](QUIZ.md) tanpa membuka jawabannya.
- [Periksa jawaban dan alasan singkat](ANSWER-KEY.md).
- [Pelajari studi kasus terintegrasi](ARCHITECTURE.md).

## Referensi utama

1. [AWS Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/).
2. [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html).
3. [Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/).
4. [AWS Organizations - SCP](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html).
5. [AWS Pricing Calculator](https://calculator.aws/) dan [Cost Explorer](https://docs.aws.amazon.com/cost-management/latest/userguide/ce-what-is.html).

> Isi disusun ulang dalam bahasa Indonesia sebagai catatan mentoring. Tidak dimaksudkan untuk menggantikan dokumentasi AWS yang terus berkembang atau menyatakan seluruh contoh berlaku universal.
