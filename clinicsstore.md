### SQL ###

```sql
-- phpMyAdmin SQL Dump
-- version 5.1.1
-- https://www.phpmyadmin.net/
--
-- Host: 127.0.0.1
-- Waktu pembuatan: 06 Jan 2025 pada 07.08
-- Versi server: 10.4.22-MariaDB
-- Versi PHP: 8.0.13

SET SQL_MODE = "NO_AUTO_VALUE_ON_ZERO";
START TRANSACTION;
SET time_zone = "+00:00";


/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!40101 SET NAMES utf8mb4 */;

--
-- Database: `clinicsstore`
--

-- --------------------------------------------------------

--
-- Struktur dari tabel `clinics`
--

CREATE TABLE `clinics` (
  `id` int(11) NOT NULL,
  `name` varchar(255) NOT NULL,
  `address` varchar(255) NOT NULL,
  `phone` varchar(15) NOT NULL,
  `schedule` varchar(225) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

--
-- Dumping data untuk tabel `clinics`
--

INSERT INTO `clinics` (`id`, `name`, `address`, `phone`, `schedule`) VALUES
(1, 'Klinik Sehat Sentosa', 'Jl. Merdeka No. 12, Jakarta', '0863-8635-3764', 'Senin-Jumat 08:00-17:00'),
(2, 'Klinik Harapan Bunda', 'Jl. Sudirman No. 45, Bandung', '0874-7293-9283', 'Senin-Sabtu 09:00-18:00'),
(3, 'Klinik Pratama Medika', 'Jl. Veteran No. 21, Surabaya', '0836-2635-2983', 'Senin-Minggu 07:00-15:00'),
(4, 'Klinik Kasih Ibu', 'Jl. Ahmad Yani No. 78, Yogyakarta', '0893-1253-8362', 'Senin-Jumat 08:00-16:00'),
(5, 'Klinik Sejahtera', 'Jl. Pahlawan No. 10, Semarang', '0825-3415-6353', 'Senin-Jumat 08:00-17:00, Sabtu 08:00-12:00'),
(7, 'Klinik Pratama Medika Sejahtera', 'JL. Merdeka no 13, Semarang Selatan', '0863-9373-9373', 'Senin-Jumat 08:00-17:00'),
(9, 'Klinik Pratama Medika Sejahtera', 'JL. Merdeka no 13, Semarang Selatan', '0832-6151-7465', 'Senin-Jumat 08:00-16:30'),
(10, 'Klinik Medika Sanjaya', 'JL. Pandansari, Boja Kendal', '0821-4748-3647', 'Setiap Hari 08.00 - 14.00'),
(11, 'Klinik Persada', 'Boja Kendal', '0867-5467-8756', 'Selasa - Sabtu 08:00-14:00'),
(12, 'Klinik Sejahtera Sehat', 'Jalan Bandung Raya- Bandung', '0815-9792-215', 'Setiap Hari Jam 06:00 - 12:00');

-- --------------------------------------------------------

--
-- Struktur dari tabel `users`
--

CREATE TABLE `users` (
  `id` int(11) NOT NULL,
  `username` varchar(50) NOT NULL,
  `password` varchar(255) NOT NULL,
  `name` varchar(100) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

--
-- Dumping data untuk tabel `users`
--

INSERT INTO `users` (`id`, `username`, `password`, `name`) VALUES
(1, 'admin', '*4ACFE3202A5FF5CF467898FC58AAB1D615029441', 'Administrator'),
(2, 'luluk', '*962C374F5BDEE4DCFE89A77C155DE2E06BE52390', 'Users');

--
-- Indexes for dumped tables
--

--
-- Indeks untuk tabel `clinics`
--
ALTER TABLE `clinics`
  ADD PRIMARY KEY (`id`);

--
-- Indeks untuk tabel `users`
--
ALTER TABLE `users`
  ADD PRIMARY KEY (`id`);

--
-- AUTO_INCREMENT untuk tabel yang dibuang
--

--
-- AUTO_INCREMENT untuk tabel `clinics`
--
ALTER TABLE `clinics`
  MODIFY `id` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=13;

--
-- AUTO_INCREMENT untuk tabel `users`
--
ALTER TABLE `users`
  MODIFY `id` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=4;
COMMIT;

/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
/*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;

```
