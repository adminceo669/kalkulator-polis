# kalkulator-polis
Kalkulator polis nilai tunai
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Kalkulator Nilai Tunai</title>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; max-width: 600px; margin: auto; }
    input, select { padding: 5px; margin: 5px 0; width: 100%; }
    button { padding: 10px; width: 100%; background-color: #1d72b8; color: white; border: none; cursor: pointer; }
    .result { margin-top: 20px; font-weight: bold; font-size: 18px; }
    label { margin-top: 10px; display: block; }
  </style>
</head>
<body>
  <h2>Kalkulator Nilai Tunai Polis</h2>

  <label for="startDate">Tanggal Awal Polis:</label>
  <input type="date" id="startDate">

  <label for="endDate">Tanggal Kejadian:</label>
  <input type="date" id="endDate">

  <label for="premi">Premi Wajib Bayar (Rp):</label>
  <input type="number" id="premi" placeholder="Contoh: 100000000">

  <label for="pengganda">Premi Terbayarkan (kali tahun bayar):</label>
  <select id="pengganda">
    <option value="1">1 tahun</option>
    <option value="2">2 tahun</option>
    <option value="5" selected>5 tahun</option>
  </select>

  <label for="ma">Masa Aktuaria (MA):</label>
  <input type="number" id="ma" value="1200">

  <button onclick="hitung()">Hitung Nilai Tunai</button>

  <div class="result" id="hasil"></div>

  <script>
    function hitung() {
      const startDate = new Date(document.getElementById('startDate').value);
      const endDate = new Date(document.getElementById('endDate').value);
      const premi = parseFloat(document.getElementById('premi').value);
      const pengganda = parseInt(document.getElementById('pengganda').value);
      const ma = parseInt(document.getElementById('ma').value);

      if (isNaN(premi) || isNaN(ma) || !startDate || !endDate || endDate < startDate) {
        document.getElementById('hasil').innerText = "Mohon lengkapi semua input dengan benar.";
        return;
      }

      const bulan = (endDate.getFullYear() - startDate.getFullYear()) * 12 + (endDate.getMonth() - startDate.getMonth());

      const Pt = premi * pengganda;
      const Ft = (bulan >= 349 || bulan >= 30 * 12) ? 1 : 1; // Disederhanakan, karena hasil akhirnya selalu 100%

      const nilaiTunai = (Pt - ((bulan / ma) * Pt)) * Ft;

      const hasilFormat = nilaiTunai.toLocaleString('id-ID', {
        style: 'currency',
        currency: 'IDR',
        maximumFractionDigits: 2
      });

      document.getElementById('hasil').innerHTML =
        `Nilai Tunai yang Diperkirakan: <br><span style="font-size:24px">${hasilFormat}</span><br><br>
        <small>
        Rumus: (Pt − (t / MA × Pt)) × Ft<br>
        Di mana:<br>
        Pt = Premi Terbayarkan (Premi Wajib Bayar × pengganda)<br>
        t = jumlah bulan dari awal polis hingga kejadian<br>
        MA = Masa Aktuaria (default 1200)<br>
        Ft = Faktor Nilai Penebusan (100% jika t ≥ 349 bulan)
        </small>`;
    }
  </script>
</body>
</html>

