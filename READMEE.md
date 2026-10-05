# DompetKu
Aplikasi catatan keuangan
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="theme-color" content="#0f766e">
  <meta name="description" content="Aplikasi pencatatan keuangan pribadi berbasis Google Sheets">
  <title>DompetKu - Catatan Keuangan</title>

  <style>
    :root {
      --primary: #0f766e;
      --primary-dark: #115e59;
      --income: #15803d;
      --income-bg: #dcfce7;
      --expense: #dc2626;
      --expense-bg: #fee2e2;
      --dark: #172033;
      --text: #334155;
      --muted: #64748b;
      --border: #e2e8f0;
      --surface: #ffffff;
      --background: #f1f5f9;
      --shadow: 0 8px 28px rgba(15, 23, 42, 0.08);
      --radius: 18px;
    }

    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      min-height: 100vh;
      font-family: Arial, Helvetica, sans-serif;
      color: var(--text);
      background:
        radial-gradient(circle at top left, rgba(20, 184, 166, 0.14), transparent 30%),
        var(--background);
    }

    button,
    input,
    select,
    textarea {
      font: inherit;
    }

    button {
      cursor: pointer;
    }

    .app {
      width: min(100%, 920px);
      margin: 0 auto;
      padding: 18px 14px 40px;
    }

    .header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 14px;
      margin-bottom: 20px;
    }

    .brand {
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .brand-icon {
      width: 48px;
      height: 48px;
      display: grid;
      place-items: center;
      color: white;
      font-size: 24px;
      border-radius: 15px;
      background: linear-gradient(135deg, var(--primary), #14b8a6);
      box-shadow: 0 8px 18px rgba(15, 118, 110, 0.25);
    }

    .brand h1 {
      margin: 0;
      color: var(--dark);
      font-size: 22px;
    }

    .brand p {
      margin: 4px 0 0;
      color: var(--muted);
      font-size: 13px;
    }

    .btn-refresh {
      padding: 10px 13px;
      color: var(--primary-dark);
      background: white;
      border: 1px solid var(--border);
      border-radius: 12px;
      font-weight: 700;
    }

    .summary-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 12px;
      margin-bottom: 18px;
    }

    .summary-card {
      min-height: 130px;
      padding: 18px;
      border: 1px solid rgba(255, 255, 255, 0.6);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
    }

    .summary-card h3 {
      margin: 0 0 10px;
      font-size: 13px;
      font-weight: 700;
      opacity: 0.85;
    }

    .summary-card .amount {
      margin: 0;
      font-size: clamp(18px, 3vw, 27px);
      font-weight: 800;
      overflow-wrap: anywhere;
    }

    .summary-card.balance {
      color: white;
      background: linear-gradient(135deg, #0f766e, #14b8a6);
    }

    .summary-card.income {
      color: var(--income);
      background: var(--income-bg);
    }

    .summary-card.expense {
      color: var(--expense);
      background: var(--expense-bg);
    }

    .card {
      margin-bottom: 18px;
      padding: 20px;
      background: var(--surface);
      border: 1px solid rgba(226, 232, 240, 0.9);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
    }

    .card-title {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 10px;
      margin-bottom: 18px;
    }

    .card-title h2 {
      margin: 0;
      color: var(--dark);
      font-size: 18px;
    }

    .card-title span {
      color: var(--muted);
      font-size: 13px;
    }

    .form-grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 14px;
    }

    .form-group {
      display: flex;
      flex-direction: column;
      gap: 7px;
    }

    .form-group.full {
      grid-column: 1 / -1;
    }

    label {
      color: var(--text);
      font-size: 13px;
      font-weight: 700;
    }

    input,
    select,
    textarea {
      width: 100%;
      padding: 12px 13px;
      color: var(--dark);
      background: #fff;
      border: 1px solid var(--border);
      border-radius: 11px;
      outline: none;
    }

    input:focus,
    select:focus,
    textarea:focus {
      border-color: var(--primary);
      box-shadow: 0 0 0 4px rgba(20, 184, 166, 0.12);
    }

    textarea {
      min-height: 88px;
      resize: vertical;
    }

    .button-row {
      display: flex;
      justify-content: flex-end;
      gap: 10px;
      margin-top: 18px;
    }

    .btn {
      padding: 12px 16px;
      border: none;
      border-radius: 11px;
      font-weight: 700;
    }

    .btn-primary {
      color: white;
      background: var(--primary);
    }

    .btn-secondary {
      color: var(--muted);
      background: #f8fafc;
      border: 1px solid var(--border);
    }

    .btn:disabled {
      cursor: not-allowed;
      opacity: 0.6;
    }

    .filter-row {
      display: grid;
      grid-template-columns: 1fr 180px;
      gap: 12px;
      margin-bottom: 16px;
    }

    .transaction-list {
      display: flex;
      flex-direction: column;
      gap: 11px;
    }

    .transaction-item {
      display: flex;
      align-items: center;
      gap: 12px;
      padding: 14px;
      background: #f8fafc;
      border: 1px solid #edf2f7;
      border-radius: 14px;
    }

    .transaction-icon {
      flex: 0 0 44px;
      width: 44px;
      height: 44px;
      display: grid;
      place-items: center;
      border-radius: 50%;
      font-size: 19px;
      font-weight: 800;
    }

    .transaction-icon.income {
      color: var(--income);
      background: var(--income-bg);
    }

    .transaction-icon.expense {
      color: var(--expense);
      background: var(--expense-bg);
    }

    .transaction-info {
      flex: 1;
      min-width: 0;
    }

    .transaction-category {
      margin: 0 0 4px;
      color: var(--dark);
      font-size: 15px;
      font-weight: 800;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .transaction-meta {
      margin: 0;
      color: var(--muted);
      font-size: 12px;
      line-height: 1.45;
    }

    .transaction-right {
      display: flex;
      align-items: flex-end;
      flex-direction: column;
      gap: 8px;
    }

    .transaction-amount {
      margin: 0;
      font-size: 14px;
      font-weight: 800;
      white-space: nowrap;
    }

    .transaction-amount.income {
      color: var(--income);
    }

    .transaction-amount.expense {
      color: var(--expense);
    }

    .btn-delete {
      padding: 5px 8px;
      color: var(--expense);
      background: transparent;
      border: 1px solid #fecaca;
      border-radius: 8px;
      font-size: 12px;
      font-weight: 700;
    }

    .empty-state {
      padding: 34px 15px;
      color: var(--muted);
      text-align: center;
      border: 1px dashed #cbd5e1;
      border-radius: 14px;
      background: #f8fafc;
    }

    .loading {
      padding: 25px;
      color: var(--muted);
      text-align: center;
    }

    .status {
      position: fixed;
      right: 16px;
      bottom: 16px;
      z-index: 99;
      max-width: min(390px, calc(100vw - 32px));
      padding: 13px 15px;
      color: white;
      border-radius: 12px;
      box-shadow: 0 12px 25px rgba(15, 23, 42, 0.2);
      transform: translateY(120px);
      opacity: 0;
      transition: 0.3s ease;
    }

    .status.show {
      transform: translateY(0);
      opacity: 1;
    }

    .status.success {
      background: var(--income);
    }

    .status.error {
      background: var(--expense);
    }

    .footer-note {
      margin: 8px 0 0;
      color: var(--muted);
      text-align: center;
      font-size: 12px;
    }

    @media (max-width: 640px) {
      .app {
        padding: 14px 12px 30px;
      }

      .header {
        align-items: flex-start;
      }

      .summary-grid,
      .form-grid,
      .filter-row {
        grid-template-columns: 1fr;
      }

      .summary-card {
        min-height: auto;
      }

      .card {
        padding: 16px;
      }

      .transaction-item {
        align-items: flex-start;
      }

      .transaction-right {
        min-width: 88px;
      }

      .button-row {
        display: grid;
        grid-template-columns: 1fr 1fr;
      }

      .btn {
        width: 100%;
      }
    }
  </style>
</head>

<body>
  <main class="app">
    <header class="header">
      <div class="brand">
        <div class="brand-icon">Rp</div>
        <div>
          <h1>DompetKu</h1>
          <p>Pencatatan keuangan pribadi</p>
        </div>
      </div>

      <button class="btn-refresh" id="refreshBtn" type="button">
        ↻ Muat Ulang
      </button>
    </header>

    <section class="summary-grid">
      <article class="summary-card balance">
        <h3>Total Saldo</h3>
        <p class="amount" id="totalBalance">Rp0</p>
      </article>

      <article class="summary-card income">
        <h3>Total Pemasukan</h3>
        <p class="amount" id="totalIncome">Rp0</p>
      </article>

      <article class="summary-card expense">
        <h3>Total Pengeluaran</h3>
        <p class="amount" id="totalExpense">Rp0</p>
      </article>
    </section>

    <section class="card">
      <div class="card-title">
        <h2>Tambah Transaksi</h2>
        <span>Data tersimpan ke Google Sheets</span>
      </div>

      <form id="transactionForm">
        <div class="form-grid">
          <div class="form-group">
            <label for="date">Tanggal</label>
            <input type="date" id="date" required>
          </div>

          <div class="form-group">
            <label for="type">Jenis Transaksi</label>
            <select id="type" required>
              <option value="Pemasukan">Pemasukan</option>
              <option value="Pengeluaran">Pengeluaran</option>
            </select>
          </div>

          <div class="form-group">
            <label for="category">Kategori</label>
            <input
              type="text"
              id="category"
              placeholder="Contoh: Makan, Gaji, Transportasi"
              required
            >
          </div>

          <div class="form-group">
            <label for="amount">Nominal</label>
            <input
              type="number"
              id="amount"
              min="1"
              inputmode="numeric"
              placeholder="Contoh: 50000"
              required
            >
          </div>

          <div class="form-group full">
            <label for="note">Catatan</label>
            <textarea
              id="note"
              placeholder="Contoh: Makan siang bersama rekan kerja"
            ></textarea>
          </div>
        </div>

        <div class="button-row">
          <button class="btn btn-secondary" type="reset">Reset</button>
          <button class="btn btn-primary" id="saveBtn" type="submit">
            Simpan Transaksi
          </button>
        </div>
      </form>
    </section>

    <section class="card">
      <div class="card-title">
        <h2>Riwayat Transaksi</h2>
        <span id="transactionCount">0 transaksi</span>
      </div>

      <div class="filter-row">
        <input
          type="search"
          id="searchInput"
          placeholder="Cari kategori atau catatan..."
        >

        <select id="filterType">
          <option value="Semua">Semua Transaksi</option>
          <option value="Pemasukan">Pemasukan</option>
          <option value="Pengeluaran">Pengeluaran</option>
        </select>
      </div>

      <div class="transaction-list" id="transactionList">
        <div class="loading">Memuat data transaksi...</div>
      </div>
    </section>

    <p class="footer-note">
      DompetKu terhubung ke Google Sheets melalui Google Apps Script.
    </p>
  </main>

  <div class="status" id="statusMessage"></div>

  <script>
    const API_URL = 'https://script.google.com/macros/s/AKfycbwWzmt8gWFjEzBS50EFnz8lfiR1B7_w8kZKqCBx6PrBTaBkhHCu2Tfso8F7kE6U-Sga1A/exec';

    let transactions = [];

    const form = document.getElementById('transactionForm');
    const dateInput = document.getElementById('date');
    const typeInput = document.getElementById('type');
    const categoryInput = document.getElementById('category');
    const amountInput = document.getElementById('amount');
    const noteInput = document.getElementById('note');
    const saveBtn = document.getElementById('saveBtn');
    const refreshBtn = document.getElementById('refreshBtn');
    const transactionList = document.getElementById('transactionList');
    const searchInput = document.getElementById('searchInput');
    const filterType = document.getElementById('filterType');

    const totalBalance = document.getElementById('totalBalance');
    const totalIncome = document.getElementById('totalIncome');
    const totalExpense = document.getElementById('totalExpense');
    const transactionCount = document.getElementById('transactionCount');
    const statusMessage = document.getElementById('statusMessage');

    document.addEventListener('DOMContentLoaded', () => {
      setTodayDate();
      loadTransactions();
    });

    function setTodayDate() {
      const today = new Date();

      const localDate = new Date(
        today.getTime() - today.getTimezoneOffset() * 60000
      ).toISOString().split('T')[0];

      dateInput.value = localDate;
    }

    function formatRupiah(value) {
      return new Intl.NumberFormat('id-ID', {
        style: 'currency',
        currency: 'IDR',
        maximumFractionDigits: 0
      }).format(Number(value) || 0);
    }

    function formatDateIndonesia(dateString) {
      if (!dateString) return '-';

      const date = new Date(`${dateString}T00:00:00`);

      return new Intl.DateTimeFormat('id-ID', {
        day: '2-digit',
        month: 'short',
        year: 'numeric'
      }).format(date);
    }

    function escapeHtml(text) {
      return String(text || '')
        .replace(/&/g, '&amp;')
        .replace(/</g, '&lt;')
        .replace(/>/g, '&gt;')
        .replace(/"/g, '&quot;')
        .replace(/'/g, '&#039;');
    }

    function showStatus(message, type = 'success') {
      statusMessage.textContent = message;
      statusMessage.className = `status ${type} show`;

      setTimeout(() => {
        statusMessage.classList.remove('show');
      }, 4000);
    }

    function loadTransactions() {
      transactionList.innerHTML = `
        <div class="loading">Memuat data transaksi...</div>
      `;

      refreshBtn.disabled = true;
      refreshBtn.textContent = 'Memuat...';

      const callbackName = `dompetKuCallback_${Date.now()}`;

      window[callbackName] = function(result) {
        try {
          if (!result.success) {
            throw new Error(result.message || 'Gagal memuat transaksi.');
          }

          transactions = Array.isArray(result.transactions)
            ? result.transactions
            : [];

          updateSummary();
          renderTransactions();
        } catch (error) {
          console.error(error);

          transactionList.innerHTML = `
            <div class="empty-state">
              Gagal memproses data transaksi.
            </div>
          `;

          showStatus('Gagal memproses data transaksi.', 'error');
        } finally {
          refreshBtn.disabled = false;
          refreshBtn.textContent = '↻ Muat Ulang';

          const oldScript = document.getElementById(callbackName);

          if (oldScript) {
            oldScript.remove();
          }

          delete window[callbackName];
        }
      };

      const script = document.createElement('script');

      script.id = callbackName;
      script.src =
        `${API_URL}?action=getTransactions&callback=${callbackName}&t=${Date.now()}`;

      script.onerror = function() {
        transactionList.innerHTML = `
          <div class="empty-state">
            Gagal memuat data. Periksa URL API dan deployment Apps Script.
          </div>
        `;

        showStatus('Gagal menghubungi API Google Sheets.', 'error');

        refreshBtn.disabled = false;
        refreshBtn.textContent = '↻ Muat Ulang';

        delete window[callbackName];
        script.remove();
      };

      document.body.appendChild(script);
    }

    function updateSummary() {
      const totalIncomeValue = transactions
        .filter(item => item.jenis === 'Pemasukan')
        .reduce((total, item) => total + Number(item.nominal || 0), 0);

      const totalExpenseValue = transactions
        .filter(item => item.jenis === 'Pengeluaran')
        .reduce((total, item) => total + Number(item.nominal || 0), 0);

      const balance = totalIncomeValue - totalExpenseValue;

      totalIncome.textContent = formatRupiah(totalIncomeValue);
      totalExpense.textContent = formatRupiah(totalExpenseValue);
      totalBalance.textContent = formatRupiah(balance);
      transactionCount.textContent = `${transactions.length} transaksi`;
    }

    function getFilteredTransactions() {
      const keyword = searchInput.value.trim().toLowerCase();
      const selectedType = filterType.value;

      return transactions.filter(item => {
        const isTypeMatch =
          selectedType === 'Semua' || item.jenis === selectedType;

        const searchableText = `
          ${item.kategori || ''}
          ${item.catatan || ''}
          ${item.tanggal || ''}
        `.toLowerCase();

        return isTypeMatch && (
          !keyword || searchableText.includes(keyword)
        );
      });
    }

    function renderTransactions() {
      const filteredTransactions = getFilteredTransactions();

      if (filteredTransactions.length === 0) {
        transactionList.innerHTML = `
          <div class="empty-state">
            Belum ada transaksi yang sesuai.
          </div>
        `;
        return;
      }

      transactionList.innerHTML = filteredTransactions
        .map(item => {
          const isIncome = item.jenis === 'Pemasukan';
          const typeClass = isIncome ? 'income' : 'expense';
          const symbol = isIncome ? '+' : '-';
          const icon = isIncome ? '↓' : '↑';

          const noteText = item.catatan
            ? ` • ${escapeHtml(item.catatan)}`
            : '';

          return `
            <article class="transaction-item">
              <div class="transaction-icon ${typeClass}">
                ${icon}
              </div>

              <div class="transaction-info">
                <p class="transaction-category">
                  ${escapeHtml(item.kategori)}
                </p>

                <p class="transaction-meta">
                  ${formatDateIndonesia(item.tanggal)}
                  • ${escapeHtml(item.jenis)}
                  ${noteText}
                </p>
              </div>

              <div class="transaction-right">
                <p class="transaction-amount ${typeClass}">
                  ${symbol}${formatRupiah(item.nominal)}
                </p>

                <button
                  class="btn-delete"
                  type="button"
                  onclick="removeTransaction('${item.id}')"
                >
                  Hapus
                </button>
              </div>
            </article>
          `;
        })
        .join('');
    }

    form.addEventListener('submit', async event => {
      event.preventDefault();

      const transactionData = {
        action: 'addTransaction',
        tanggal: dateInput.value,
        jenis: typeInput.value,
        kategori: categoryInput.value.trim(),
        nominal: Number(amountInput.value),
        catatan: noteInput.value.trim()
      };

      if (
        !transactionData.tanggal ||
        !transactionData.jenis ||
        !transactionData.kategori ||
        !transactionData.nominal ||
        transactionData.nominal <= 0
      ) {
        showStatus('Lengkapi seluruh data transaksi dengan benar.', 'error');
        return;
      }

      saveBtn.disabled = true;
      saveBtn.textContent = 'Menyimpan...';

      try {
        await fetch(API_URL, {
          method: 'POST',
          mode: 'no-cors',
          headers: {
            'Content-Type': 'text/plain;charset=utf-8'
          },
          body: JSON.stringify(transactionData)
        });

        showStatus('Data dikirim. Menunggu data masuk ke Google Sheets...');
        form.reset();
        setTodayDate();

        setTimeout(() => {
          loadTransactions();
        }, 2500);
      } catch (error) {
        console.error(error);

        showStatus(
          'Gagal mengirim transaksi. Periksa koneksi internet.',
          'error'
        );
      } finally {
        saveBtn.disabled = false;
        saveBtn.textContent = 'Simpan Transaksi';
      }
    });

    async function removeTransaction(id) {
      const confirmed = confirm('Hapus transaksi ini dari Google Sheets?');

      if (!confirmed) return;

      try {
        await fetch(API_URL, {
          method: 'POST',
          mode: 'no-cors',
          headers: {
            'Content-Type': 'text/plain;charset=utf-8'
          },
          body: JSON.stringify({
            action: 'deleteTransaction',
            id: id
          })
        });

        showStatus('Permintaan hapus dikirim. Memuat ulang data...');

        setTimeout(() => {
          loadTransactions();
        }, 2500);
      } catch (error) {
        console.error(error);

        showStatus('Gagal mengirim permintaan hapus.', 'error');
      }
    }

    refreshBtn.addEventListener('click', loadTransactions);
    searchInput.addEventListener('input', renderTransactions);
    filterType.addEventListener('change', renderTransactions);
  </script>
</body>
</html>