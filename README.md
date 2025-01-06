# UAS_2101550006_LULUK-TRI-UTAMI_2101550006

### login.php ###
```
<?php
session_start();
require_once 'config.php';

if(isset($_POST['login'])) {
    $username = $_POST['username'];
    $password = $_POST['password'];
    
    // Gunakan fungsi PASSWORD() untuk mencocokkan dengan hash di database
    $query = "SELECT * FROM users WHERE username='$username' AND password=PASSWORD('$password')";
    $result = mysqli_query($conn, $query);
    
    if(mysqli_num_rows($result) == 1) {
        $row = mysqli_fetch_assoc($result);
        $_SESSION['user_id'] = $row['id'];
        $_SESSION['username'] = $row['username'];
        $_SESSION['name'] = $row['name'];
            
        header("Location: index.php");
        exit();
    } else {
        $error = "Username atau Password salah!";
    }
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login</title>
</head>
<body>
    <div style="width: 100%; max-width: 400px; margin: 100px auto; padding: 20px; border: 1px solid #ccc; border-radius: 10px; background-color: #f9f9f9;">
        <h2 style="text-align: center; color: #007bff;">Login Form</h2>

        <?php if(isset($error)) { ?>
            <p style="color: red; text-align: center;"><?php echo $error; ?></p>
        <?php } ?>
        
        <form action="" method="POST" style="text-align: center;">
            <div style="margin-bottom: 15px;">
                <label for="username" style="font-weight: bold; color: #333;">Username:</label>
                <input type="text" id="username" name="username" required style="padding: 10px; width: 80%; border: 1px solid #ccc; border-radius: 5px;">
            </div>

            <div style="margin-bottom: 20px;">
                <label for="password" style="font-weight: bold; color: #333;">Password:</label>
                <input type="password" id="password" name="password" required style="padding: 10px; width: 80%; border: 1px solid #ccc; border-radius: 5px;">
            </div>

            <div>
                <input type="submit" name="login" value="Login" style="padding: 10px 20px; background-color: #007bff; color: white; border: none; border-radius: 5px; cursor: pointer;">
            </div>
        </form>
    </div>
</body>
</html>
```

### config.php ###
```
<?php
$host = "localhost";
$dbuser = "root";
$dbpass = "";
$dbname = "clinicsstore";

$conn = mysqli_connect($host, $dbuser, $dbpass, $dbname);
if (!$conn) {
    die("Connection failed: " . mysqli_connect_error());
}
?>
```

### index.php ###
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Daftar Klinik Kesehatan</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Nunito:wght@400;600&display=swap" rel="stylesheet">
<style>
    body {
        font-family: 'Nunito', sans-serif;
        background-color: #f8f9fa;
        margin-top: 70px; /* Menyesuaikan agar konten tidak terhalang navbar yang fixed */
        margin-left: 0;
        margin-right: 0;
    }

    /* Styling Navbar */
    .navbar {
        background-color: #003366; /* Dark Blue */
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        width: 100%; /* Memastikan navbar memenuhi layar penuh */
        z-index: 1000;
        padding: 15px 0; /* Padding navbar */
    }

    .navbar-brand {
        font-size: 2rem;
        font-weight: bold;
        color: white !important;
    }

    .navbar-brand:hover {
        color: #ffcc00 !important;
    }

    .navbar-nav .nav-link {
        color: white !important;
        font-size: 1.2rem;
        padding: 12px 25px;
        transition: background-color 0.3s ease;
    }

    /* Hover effect for navbar links */
    .navbar-nav .nav-link:hover {
        color: #ffcc00 !important;
        background-color: #00509e;
        border-radius: 5px;
    }

    .navbar-nav .nav-link.logout-btn {
        background-color: #dc3545;  /* Red color for logout */
        color: white;
        padding: 10px 25px;
        border-radius: 25px;
        font-size: 1.2rem;
        transition: background-color 0.3s ease;
    }

    .navbar-nav .nav-link.logout-btn:hover {
        background-color: #c82333;
    }

    /* Container - Full width */
    .container {
        width: 100%; /* Memastikan container menggunakan lebar penuh */
        padding: 20px;
        box-sizing: border-box; /* Menyesuaikan padding tanpa mempengaruhi ukuran */
    }

    h1 {
        font-size: 3rem;
        color: #495057;
        text-align: center;
        margin-bottom: 40px;
    }

    /* Card */
    .card {
        border-radius: 15px;
        box-shadow: 0 2px 15px rgba(0, 0, 0, 0.1);
        margin-bottom: 40px;
    }

    /* Table */
    .table {
        background-color: white;
        border-radius: 10px;
        overflow: hidden;
        width: 100%; /* Memastikan tabel memenuhi lebar layar */
    }

    .table th, .table td {
        vertical-align: middle;
        padding: 15px 20px; /* Padding tabel agar lebih luas */
    }

    .table th {
        background-color: #003366;
        color: white;
    }

    .table td {
        background-color: #f8f9fa;
    }

    /* Modal Content */
    .modal-content {
        border-radius: 10px;
    }

    .form-control {
        border-radius: 10px;
        padding: 12px;
    }

    /* Tombol */
    .btn-primary, .btn-success, .btn-warning, .btn-danger {
        border-radius: 25px;
        padding: 12px 30px;
    }

    .btn-primary {
        background-color: #003366;
    }

    .btn-primary:hover {
        background-color: #00509e;
    }

    .btn-success {
        background-color: #28a745;
    }

    .btn-success:hover {
        background-color: #218838;
    }

    .btn-warning {
        background-color: #ffc107;
    }

    .btn-warning:hover {
        background-color: #e0a800;
    }

    .btn-danger {
        background-color: #dc3545;
    }

    .btn-danger:hover {
        background-color: #c82333;
    }

    .modal-header {
        background-color: #f1f1f1;
        border-bottom: 1px solid #ddd;
    }

    .modal-footer {
        background-color: #f1f1f1;
    }

    /* Search Bar */
    .search-bar {
        display: flex;
        justify-content: space-between;
        margin-bottom: 25px;
    }

    .search-bar .form-control {
        max-width: 350px;
    }

    /* Button Group */
    .btn-group {
        display: flex;
        justify-content: flex-end;
    }

    /* Contact Section */
    .contact-section {
        padding: 50px 0;
        background-color: #ffffff;
        width: 100%; /* Menjamin section ini juga memenuhi lebar layar */
        margin: 0;
    }

    .contact-info {
        font-size: 1.2rem;
        color: #6c757d;
        text-align: center;
        margin-bottom: 40px;
    }

    .form-section {
        margin-top: 40px;
    }

    .form-section h3 {
        font-size: 1.8rem;
        margin-bottom: 30px;
    }

    /* Footer Section */
    .footer {
        text-align: center;
        padding: 20px 0;
        background-color: #f8f9fa;
        color: #6c757d;
        font-size: 1rem;
        margin-top: 40px;
        width: 100%;
        box-sizing: border-box; /* Pastikan footer mengambil full width */
    }
</style>


</head>
<body class="container py-4">
    <!-- Navbar with Home Link -->
   <nav class="navbar navbar-expand-lg navbar-light">
    <a class="navbar-brand" href="#">Daftar Klinik Kesehatan</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav ms-auto">
            <li class="nav-item">
                <a class="nav-link active" href="home.php">Home</a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="index.php">Daftar Klinik</a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="contact.php">Kontak</a>
            </li>
            <!-- Tambahkan tombol logout di sini -->
            <li class="nav-item">
                <a class="nav-link logout-btn" href="javascript:logout()">Logout</a>
            </li>
        </ul>
    </div>
</nav>

    <h1>Selamat Datang di Daftar Klinik</h1>

    <div class="card p-4 mb-4">
        <h5>Kenapa menggunakan Daftar Klinik?</h5>
        <p>Website ini menyediakan daftar lengkap klinik yang dapat diakses oleh masyarakat. Anda dapat mencari klinik berdasarkan ID, nama, atau lokasi. Tambahkan klinik baru, edit informasi klinik yang sudah ada, atau hapus data klinik yang tidak diperlukan.</p>
    </div>

    <div class="card p-4 mb-4">
        <div class="search-bar">
            <div class="col">
                <input type="text" id="searchInput" class="form-control" placeholder="Cari berdasarkan ID">
            </div>
            <div class="btn-group">
                <button onclick="searchClinic()" class="btn btn-primary">Cari</button>
                <button type="button" class="btn btn-success" data-bs-toggle="modal" data-bs-target="#clinicModal">
                    Tambah Klinik
                </button>
            </div>
        </div>
    </div>

    <table class="table table-striped table-hover">
        <thead>
            <tr>
                <th>ID</th>
                <th>Nama</th>
                <th>Alamat</th>
                <th>Telepon</th>
                <th>Jadwal</th>
                <th>Aksi</th>
            </tr>
        </thead>
        <tbody id="clinicList">
            <!-- Clinics will be loaded here -->
        </tbody>
    </table>

    <!-- Modal for Add/Edit Clinic -->
    <div class="modal fade" id="clinicModal" tabindex="-1" aria-labelledby="clinicModalLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="modalTitle">Tambah Klinik</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <form id="clinicForm">
                        <input type="hidden" id="clinicId">
                        <div class="mb-3">
                            <label for="name" class="form-label">Nama</label>
                            <input type="text" class="form-control" id="name" required>
                        </div>
                        <div class="mb-3">
                            <label for="address" class="form-label">Alamat</label>
                            <input type="text" class="form-control" id="address" required>
                        </div>
                        <div class="mb-3">
                            <label for="phone" class="form-label">Telepon</label>
                            <input type="text" class="form-control" id="phone" required>
                        </div>
                        <div class="mb-3">
                            <label for="schedule" class="form-label">Jadwal</label>
                            <input type="text" class="form-control" id="schedule" required>
                        </div>
                    </form>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Batal</button>
                    <button type="button" class="btn btn-primary" onclick="saveClinic()">Simpan</button>
                </div>
            </div>
        </div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js"></script>
    <script>
        const API_URL = 'http://localhost/rest_clinics/clinics_api.php';
        let clinicModal;

        document.addEventListener('DOMContentLoaded', function() {
            clinicModal = new bootstrap.Modal(document.getElementById('clinicModal'));
            loadClinics();
        });

        function loadClinics() {
            fetch(API_URL)
                .then(response => response.json())
                .then(clinics => {
                    const clinicList = document.getElementById('clinicList');
                    clinicList.innerHTML = '';
                    clinics.forEach(clinic => {
                        clinicList.innerHTML += ` 
                            <tr>
                                <td>${clinic.id}</td>
                                <td>${clinic.name}</td>
                                <td>${clinic.address}</td>
                                <td>${clinic.phone}</td>
                                <td>${clinic.schedule}</td>
                                <td class="btn-group-action">
                                    <button class="btn btn-sm btn-warning me-1" onclick="editClinic(${clinic.id})">Edit</button>
                                    <button class="btn btn-sm btn-danger" onclick="deleteClinic(${clinic.id})">Hapus</button>
                                </td>
                            </tr>
                        `;
                    });
                })
                .catch(error => alert('Error loading clinics: ' + error));
        }

        function logout() {
            // Logic for logging out (clear session, redirect, etc.)
            alert('You have been logged out');
            window.location.href = 'login.php';  // Redirect to login page
        }

        function searchClinic() {
            const id = document.getElementById('searchInput').value;
            if (!id) {
                loadClinics();
                return;
            }

            fetch(`${API_URL}/${id}`)
                .then(response => response.json())
                .then(clinic => {
                    const clinicList = document.getElementById('clinicList');
                    if (clinic.message) {
                        alert('Clinic not found');
                        return;
                    }
                    clinicList.innerHTML = ` 
                        <tr>
                            <td>${clinic.id}</td>
                            <td>${clinic.name}</td>
                            <td>${clinic.address}</td>
                            <td>${clinic.phone}</td>
                            <td>${clinic.schedule}</td>
                            <td class="btn-group-action">
                                <button class="btn btn-sm btn-warning me-1" onclick="editClinic(${clinic.id})">Edit</button>
                                <button class="btn btn-sm btn-danger" onclick="deleteClinic(${clinic.id})">Hapus</button>
                            </td>
                        </tr>
                    `;
                })
                .catch(error => alert('Error searching clinic: ' + error));
        }

        function editClinic(id) {
            fetch(`${API_URL}/${id}`)
                .then(response => response.json())
                .then(clinic => {
                    document.getElementById('clinicId').value = clinic.id;
                    document.getElementById('name').value = clinic.name;
                    document.getElementById('address').value = clinic.address;
                    document.getElementById('phone').value = clinic.phone;
                    document.getElementById('schedule').value = clinic.schedule;
                    document.getElementById('modalTitle').textContent = 'Edit Klinik';
                    clinicModal.show();
                })
                .catch(error => alert('Error loading clinic details: ' + error));
        }

        function deleteClinic(id) {
            if (confirm('Apakah Anda yakin ingin menghapus klinik ini?')) {
                fetch(`${API_URL}/${id}`, {
                    method: 'DELETE'
                })
                .then(response => response.json())
                .then(data => {
                    alert('Klinik berhasil dihapus');
                    loadClinics();
                })
                .catch(error => alert('Error deleting clinic: ' + error));
            }
        }

        function saveClinic() {
            const clinicId = document.getElementById('clinicId').value;
            const clinicData = {
                name: document.getElementById('name').value,
                address: document.getElementById('address').value,
                phone: document.getElementById('phone').value,
                schedule: document.getElementById('schedule').value
            };

            const method = clinicId ? 'PUT' : 'POST';
            const url = clinicId ? `${API_URL}/${clinicId}` : API_URL;

            fetch(url, {
                method: method,
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(clinicData)
            })
            .then(response => response.json())
            .then(data => {
                alert(clinicId ? 'Klinik berhasil diperbarui' : 'Klinik berhasil ditambahkan');
                clinicModal.hide();
                loadClinics();
                resetForm();
            })
            .catch(error => alert('Error saving clinic: ' + error));
        }

        function resetForm() {
            document.getElementById('clinicId').value = '';
            document.getElementById('clinicForm').reset();
            document.getElementById('modalTitle').textContent = 'Tambah Klinik';
        }

        document.getElementById('clinicModal').addEventListener('hidden.bs.modal', resetForm);
    </script>
</body>
</html>
```

### home.php ###
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Home - Daftar Klinik Kesehatan</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Nunito:wght@400;600&display=swap" rel="stylesheet">
    <style>
    body {
        font-family: 'Nunito', sans-serif;
        background-color: #f8f9fa;
        margin-top: 50px;
    }

    /* Styling Navbar */
    .navbar {
        background-color: #003366; /* Dark Blue */
        position: fixed;
        width: 100%;
        top: 0;
        z-index: 1000;
        padding: 10px 0;
    }

    .navbar-brand {
        font-size: 1.8rem;
        font-weight: bold;
        color: white !important;
    }

    .navbar-brand:hover {
        color: #ffcc00 !important;
    }

    .navbar-nav .nav-link {
        color: white !important;
        font-size: 1.1rem;
        padding: 10px 20px;
        transition: background-color 0.3s ease;
    }

    /* Hover effect for navbar links */
    .navbar-nav .nav-link:hover {
        color: #ffcc00 !important;
        background-color: #00509e;
        border-radius: 5px;
    }

    .navbar-nav .nav-link.logout-btn {
        background-color: #dc3545;  /* Red color for logout */
        color: white;
        padding: 8px 20px;
        border-radius: 25px;
        font-size: 1.1rem;
        transition: background-color 0.3s ease;
    }

    /* Hover effect for logout button */
    .navbar-nav .nav-link.logout-btn:hover {
        background-color: #c82333;  /* Darker red */
        color: white;
    }

    /* Hero Section */
    .hero-section {
        background: url('background.jpg') no-repeat center center/cover;
        color: #ffcc00; /* Updated text color */
        padding: 100px 0;
        text-align: center;
        margin-top: 75px;
        position: relative;
    }

    .hero-section::after {
        content: "";
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, 0.5); /* Black overlay with transparency */
        z-index: 1;
    }

    .hero-section h1, .hero-section p, .hero-section a {
        position: relative;
        z-index: 2;
    }

    .hero-section h1 {
        font-size: 3rem;
        margin-bottom: 20px;
    }

    .hero-section p {
        font-size: 1.2rem;
        margin-bottom: 30px;
    }

    .footer {
        background-color: #f1f1f1;
        padding: 20px;
        text-align: center;
    }

    /* Card Styles */
    .card {
        box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        border-radius: 15px;
        margin-bottom: 40px;
    }

    .card-body h5 {
        font-size: 1.5rem;
        margin-bottom: 15px;
    }

    .card-body p {
        font-size: 1rem;
        color: #6c757d;
    }

    </style>
</head>
<body>

     <!-- Navbar -->
    <nav class="navbar navbar-expand-lg navbar-light">
        <a class="navbar-brand" href="home.php">Daftar Klinik Kesehatan</a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav ms-auto"> <!-- Menu akan berada di sebelah kiri -->
                <li class="nav-item">
                    <a class="nav-link" href="home.php">Home</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="index.php">Daftar Klinik</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link active" href="contact.php">Kontak</a>
                </li>
                <!-- Tombol Logout di dalam Navbar -->
                <li class="nav-item">
                    <a class="nav-link logout-btn" href="login.php" onclick="logout()">Logout</a>
                </li>
            </ul>
        </div>
    </nav>

    <!-- Hero Section: Introduction -->
    <div class="hero-section">
        <h1>Selamat Datang Di Daftar Klinik Kesehatan</h1>
        <p>Temukan klinik kesehatan terbaik yang menyediakan layanan medis untuk Anda dan keluarga. Dengan informasi lengkap, jadwal buka, dan lokasi yang mudah dijangkau.</p>
        <a href="index.php" class="btn btn-light btn-lg">Lihat Daftar Klinik</a>
    </div>

    <!-- Pengenalan Klinik Kesehatan -->
    <div class="container">
        <div class="card">
            <div class="card-body">
                <h5 class="card-title">Apa itu Klinik Kesehatan?</h5>
                <p class="card-text">
                    Klinik kesehatan adalah fasilitas layanan medis yang menyediakan perawatan kesehatan primer. Klinik ini bisa menyediakan berbagai jenis layanan, mulai dari pemeriksaan kesehatan rutin hingga perawatan untuk penyakit tertentu.
                </p>
                <p class="card-text">
                    Klinik kesehatan penting karena mereka membantu masyarakat untuk mendapatkan perawatan medis yang terjangkau, mudah diakses, dan tepat waktu. Mereka sering menjadi tempat pertama yang dikunjungi oleh pasien yang membutuhkan pertolongan medis.
                </p>
            </div>
        </div>

        <div class="card">
            <div class="card-body">
                <h5 class="card-title">Fungsi dan Manfaat Klinik Kesehatan</h5>
                <p class="card-text">
                    Klinik kesehatan memiliki peran yang sangat penting dalam masyarakat, terutama untuk meningkatkan kualitas hidup dan kesehatan masyarakat. Beberapa fungsi dan manfaat klinik kesehatan antara lain:
                </p>
                <ul>
                    <li><strong>Penyuluhan Kesehatan:</strong> Klinik memberikan edukasi dan informasi kesehatan untuk mencegah penyakit.</li>
                    <li><strong>Perawatan Penyakit Umum:</strong> Klinik melayani pengobatan penyakit ringan hingga menengah, seperti flu, batuk, atau infeksi.</li>
                    <li><strong>Pemeriksaan Rutin:</strong> Klinik menyediakan layanan pemeriksaan kesehatan untuk deteksi dini penyakit.</li>
                    <li><strong>Pengobatan Lanjutan:</strong> Klinik menjadi tempat untuk kontrol penyakit kronis seperti diabetes, hipertensi, dll.</li>
                    <li><strong>Fasilitas Medis yang Terjangkau:</strong> Klinik lebih terjangkau dibandingkan rumah sakit besar, memudahkan akses masyarakat.</li>
                </ul>
            </div>
        </div>

        <div class="card">
            <div class="card-body">
                <h5 class="card-title">Kenapa Memilih Klinik Kesehatan?</h5>
                <p class="card-text">
                    Klinik kesehatan dapat menjadi solusi bagi Anda yang membutuhkan layanan medis cepat dan mudah diakses. Selain itu, klinik biasanya memiliki biaya yang lebih terjangkau dan proses pelayanan yang lebih cepat daripada rumah sakit besar. Klinik juga memiliki tenaga medis yang terlatih dan fasilitas yang cukup memadai untuk menangani masalah kesehatan yang tidak terlalu kompleks.
                </p>
                <p class="card-text">
                    Melalui website ini, Anda bisa dengan mudah mencari dan menemukan klinik kesehatan terbaik di daerah Anda. Cukup masukkan ID klinik atau nama untuk melihat informasi lebih lanjut.
                </p>
            </div>
        </div>
    </div>

    <!-- Footer Section -->
    <div class="footer">
        <p>&copy; 2025 Daftar Klinik Kesehatan. All Rights Reserved.</p>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js"></script>
    <script>
        function logout() {
            // Logic for logging out (clear session, redirect, etc.)
            alert('Anda telah keluar');
            window.location.href = 'login.php';  // Redirect to login page
        }
    </script>

</body>
</html>

```
### contact.php ###
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kontak - Daftar Klinik Kesehatan</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Nunito:wght@400;600&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Nunito', sans-serif;
            background-color: #f8f9fa;
            margin-top: 50px;
        }

        .navbar {
            background-color: #003366; /* Dark Blue */
            position: fixed;
            width: 100%;
            top: 0;
            z-index: 1000;
            padding: 10px 0;
        }
        .navbar-brand {
            font-size: 1.8rem;
            font-weight: bold;
            color: white !important;
        }

        .navbar-brand:hover {
            color: #ffcc00 !important;
        }

        .navbar-nav .nav-link {
            color: white !important;
            font-size: 1.1rem;
            padding: 10px 20px;
            transition: background-color 0.3s ease;
        }

        /* Hover effect for navbar links */
        .navbar-nav .nav-link:hover {
            color: #ffcc00 !important;
            background-color: #00509e;
            border-radius: 5px;
        }

        .navbar-nav .nav-link.logout-btn {
            background-color: #dc3545;  /* Red color for logout */
            color: white;
            padding: 8px 20px;
            border-radius: 25px;
            font-size: 1.1rem;
            transition: background-color 0.3s ease;
        }

        /* Hover effect for logout button */
        .navbar-nav .nav-link.logout-btn:hover {
            background-color: #c82333;  /* Darker red */
            color: white;
        }

        /* Contact Section */
        .contact-section {
            margin-top: 100px; /* Tambahkan margin untuk memberi jarak antara navbar dan konten */
            padding: 60px 0;
            background-color: #f1f1f1;
        }

        .contact-section h1 {
            text-align: center;
            font-size: 2.5rem;
            margin-bottom: 20px;
        }

        .contact-info {
            text-align: center;
            font-size: 1.2rem;
            color: #6c757d;
            margin-bottom: 40px;
        }

        /* Contact Information */
        .container .row .col-md-4 h3 {
            font-size: 1.5rem;
            margin-bottom: 10px;
        }

        .container .row .col-md-4 p {
            font-size: 1.1rem;
            color: #333;
        }

        /* Form Section */
        .form-section {
            background-color: #fff;
            padding: 40px;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }

        .form-section h3 {
            text-align: center;
            font-size: 2rem;
            margin-bottom: 30px;
        }

        .form-group {
            margin-bottom: 20px;
        }

        .submit-btn {
            display: block;
            width: 100%;
            background-color: #003366;
            color: white;
            padding: 10px;
            font-size: 1.2rem;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }

        .submit-btn:hover {
            background-color: #00509e;
        }

        .footer {
            background-color: #f1f1f1;
            padding: 20px;
            text-align: center;
        }
    </style>
</head>
<body>

    <!-- Navbar -->
    <nav class="navbar navbar-expand-lg navbar-light">
        <a class="navbar-brand" href="home.php">Daftar Klinik Kesehatan</a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav ms-auto">
                <li class="nav-item">
                    <a class="nav-link" href="home.php">Home</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="index.php">Daftar Klinik</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link active" href="contact.php">Kontak</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link logout-btn" href="login.php" onclick="logout()">Logout</a>
                </li>
            </ul>
        </div>
    </nav>

    <!-- Contact Section -->
    <div class="contact-section">
        <h1>Kontak Kami</h1>
        <p class="contact-info">Jika Anda memiliki pertanyaan atau ingin berkolaborasi, silakan hubungi kami melalui informasi di bawah atau gunakan formulir untuk mengirim pesan.</p>

        <!-- Contact Information -->
        <div class="container">
            <div class="row">
                <div class="col-md-4">
                    <h3>Alamat</h3>
                    <p>Jl. Kesehatan No.123, Jakarta, Indonesia</p>
                </div>
                <div class="col-md-4">
                    <h3>Email</h3>
                    <p>info@daftarklinik.com</p>
                </div>
                <div class="col-md-4">
                    <h3>Telepon</h3>
                    <p>(021) 1234-5678</p>
                </div>
            </div>
        </div>

        <!-- Contact Form -->
        <div class="container form-section">
            <h3>Kirim Pesan</h3>
            <form method="post" action="submit_contact.php" onsubmit="confirmSubmission(event)">
                <div class="form-group">
                    <input type="text" class="form-control" id="name" name="name" placeholder="Nama" required>
                </div>
                <div class="form-group">
                    <input type="email" class="form-control" id="email" name="email" placeholder="Email" required>
                </div>
                <div class="form-group">
                    <textarea class="form-control" id="message" name="message" rows="4" placeholder="Pesan" required></textarea>
                </div>
                <button type="submit" class="btn submit-btn">Kirim Pesan</button>
            </form>
        </div>
    </div>

    <!-- Footer Section -->
    <div class="footer">
        <p>&copy; 2025 Daftar Klinik Kesehatan. All Rights Reserved.</p>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js"></script>
    <script>
        function logout() {
            // Logic for logging out (clear session, redirect, etc.)
            alert('Anda telah keluar');
            window.location.href = 'login.php';  // Redirect to login page
        }

        function confirmSubmission(event) {
            event.preventDefault();  // Prevent form submission
            alert('Pesan Anda telah berhasil dikirim. Terima kasih!');
            
            // Redirect to contact page after alert
            window.location.href = 'contact.php';
        }
    </script>

</body>
</html>

```
### logout.php ###
```
<?php
session_start();  // Mulai sesi jika belum dimulai

// Hapus semua variabel sesi
session_unset();

// Hapus sesi itu sendiri
session_destroy();

// Kirimkan respons sukses
echo json_encode(['success' => true]);
?>

```

### clinics_api.php ###
```
<?php
header("Content-Type: application/json; charset=UTF-8");
header("Access-Control-Allow-Origin: *");
header("Access-Control-Allow-Methods: GET, POST, PUT, DELETE");
header("Access-Control-Allow-Headers: Content-Type, Access-Control-Allow-Headers, Authorization, X-Requested-With");

$method = $_SERVER['REQUEST_METHOD'];
$request = [];

if (isset($_SERVER['PATH_INFO'])) {
    $request = explode('/', trim($_SERVER['PATH_INFO'],'/'));
}

function getConnection() {
    $host = 'localhost';
    $db   = 'clinicsstore';
    $user = 'root';
    $pass = ''; // Ganti dengan password MySQL Anda jika ada
    $charset = 'utf8mb4';

    $dsn = "mysql:host=$host;dbname=$db;charset=$charset";
    $options = [
        PDO::ATTR_ERRMODE            => PDO::ERRMODE_EXCEPTION,
        PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
        PDO::ATTR_EMULATE_PREPARES   => false,
    ];
    try {
        return new PDO($dsn, $user, $pass, $options);
    } catch (\PDOException $e) {
        throw new \PDOException($e->getMessage(), (int)$e->getCode());
    }
}

function response($status, $data = NULL) {
    header("HTTP/1.1 " . $status);
    if ($data) {
        echo json_encode($data);
    }
    exit();
}

$db = getConnection();

switch ($method) {
    case 'GET':
        if (!empty($request) && isset($request[0])) {
            $id = $request[0];
            $stmt = $db->prepare("SELECT * FROM clinics WHERE id = ?");
            $stmt->execute([$id]);
            $clinics = $stmt->fetch();
            if ($clinics) {
                response(200, $clinics);
            } else {
                response(404, ["message" => "clinics not found"]);
            }
        } else {
            $stmt = $db->query("SELECT * FROM clinics");
            $clinics = $stmt->fetchAll();
            response(200, $clinics);
        }
        break;
    
    case 'POST':
        $data = json_decode(file_get_contents("php://input"));
        if (!isset($data->name) || !isset($data->address) || !isset($data->phone) || !isset($data->schedule)) {
            response(400, ["message" => "Missing required fields"]);
        }
        $sql = "INSERT INTO clinics (name, address, phone, schedule) VALUES (?, ?, ?, ?)";
        $stmt = $db->prepare($sql);
        if ($stmt->execute([$data->name, $data->address, $data->phone, $data->schedule ])) {
            response(201, ["message" => "clinics created", "id" => $db->lastInsertId()]);
        } else {
            response(500, ["message" => "Failed to create clinics"]);
        }
        break;
    
    case 'PUT':
        if (empty($request) || !isset($request[0])) {
            response(400, ["message" => "clinics ID is required"]);
        }
        $id = $request[0];
        $data = json_decode(file_get_contents("php://input"));
        if (!isset($data->name) || !isset($data->address) || !isset($data->phone) || !isset($data->schedule)) {
            response(400, ["message" => "Missing required fields"]);
        }
        $sql = "UPDATE clinics SET name = ?, address = ?, phone = ?, schedule = ? WHERE id = ?";
        $stmt = $db->prepare($sql);
        if ($stmt->execute([$data->name, $data->address, $data->phone, $data->schedule, $id])) {
            response(200, ["message" => "clinics updated"]);
        } else {
            response(500, ["message" => "Failed to update clinics"]);
        }
        break;
    
    case 'DELETE':
        if (empty($request) || !isset($request[0])) {
            response(400, ["message" => "clinics ID is required"]);
        }
        $id = $request[0];
        $sql = "DELETE FROM clinics WHERE id = ?";
        $stmt = $db->prepare($sql);
        if ($stmt->execute([$id])) {
            response(200, ["message" => "clinics deleted"]);
        } else {
            response(500, ["message" => "Failed to delete clinics"]);
        }
        break;
    
    default:
        response(405, ["message" => "Method not allowed"]);
        break;
}
?>
```
