# Index.php

<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kalkulator PHP Sederhana</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f4f7f6;
            margin: 0;
            color: #333;
        }
        .container {
            background-color: #ffffff;
            padding: 2rem;
            border-radius: 12px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.05);
            width: 100%;
            max-width: 400px;
        }
        h1 {
            text-align: center;
            color: #2c3e50;
            margin-top: 0;
        }
        form {
            display: flex;
            flex-direction: column;
            gap: 1rem;
        }
        .form-group {
            display: flex;
            flex-direction: column;
        }
        .form-group label {
            margin-bottom: 0.5rem;
            font-weight: 600;
        }
        .form-group input,
        .form-group select {
            padding: 0.75rem;
            border: 1px solid #ddd;
            border-radius: 8px;
            font-size: 1rem;
        }
        button {
            padding: 0.85rem;
            background-color: #3498db;
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: 700;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        button:hover {
            background-color: #2980b9;
        }
        .result {
            margin-top: 1.5rem;
            padding: 1rem;
            background-color: #eaf6ff;
            border: 1px solid #b8dcfd;
            border-radius: 8px;
            text-align: center;
            font-size: 1.2rem;
            font-weight: 600;
            color: #1a5c92;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>Kalkulator PHP</h1>

        <?php
        // Logika PHP untuk kalkulator
        $hasil = null;
        $angka1 = '';
        $angka2 = '';
        $operasi = '';

        if ($_SERVER["REQUEST_METHOD"] == "POST") {
            // Ambil data dari form dengan aman
            $angka1 = isset($_POST['angka1']) ? filter_var($_POST['angka1'], FILTER_SANITIZE_NUMBER_FLOAT, FILTER_FLAG_ALLOW_FRACTION) : '';
            $angka2 = isset($_POST['angka2']) ? filter_var($_POST['angka2'], FILTER_SANITIZE_NUMBER_FLOAT, FILTER_FLAG_ALLOW_FRACTION) : '';
            $operasi = isset($_POST['operasi']) ? htmlspecialchars($_POST['operasi']) : '';

            // Validasi bahwa angka telah diisi
            if (is_numeric($angka1) && is_numeric($angka2)) {
                switch ($operasi) {
                    case 'tambah':
                        $hasil = $angka1 + $angka2;
                        break;
                    case 'kurang':
                        $hasil = $angka1 - $angka2;
                        break;
                    case 'kali':
                        $hasil = $angka1 * $angka2;
                        break;
                    case 'bagi':
                        if ($angka2 != 0) {
                            $hasil = $angka1 / $angka2;
                        } else {
                            $hasil = "Error: Tidak bisa dibagi dengan nol!";
                        }
                        break;
                    default:
                        $hasil = "Error: Operasi tidak valid!";
                }
            } else {
                $hasil = "Error: Mohon masukkan angka yang valid.";
            }
        }
        ?>

        <form action="index.php" method="POST">
            <div class="form-group">
                <label for="angka1">Angka 1</label>
                <input type="number" step="any" id="angka1" name="angka1" value="<?php echo $angka1; ?>" required>
            </div>
            
            <div class="form-group">
                <label for="operasi">Operasi</label>
                <select id="operasi" name="operasi">
                    <option value="tambah" <?php echo ($operasi == 'tambah') ? 'selected' : ''; ?>>+</option>
                    <option value="kurang" <?php echo ($operasi == 'kurang') ? 'selected' : ''; ?>>-</option>
                    <option value="kali" <?php echo ($operasi == 'kali') ? 'selected' : ''; ?>>*</option>
                    <option value="bagi" <?php echo ($operasi == 'bagi') ? 'selected' : ''; ?>>/</option>
                </select>
            </div>

            <div class="form-group">
                <label for="angka2">Angka 2</label>
                <input type="number" step="any" id="angka2" name="angka2" value="<?php echo $angka2; ?>" required>
            </div>
            
            <button type="submit">Hitung</button>
        </form>

        <?php
        // Tampilkan hasil jika ada
        if ($hasil !== null) {
            echo "<div class='result'>Hasil: " . htmlspecialchars($hasil) . "</div>";
        }
        ?>
    </div>

</body>
</html>
