# Bagindo â€” Flowchart Sistem

Flowchart lengkap cara kerja project Bagindo (Synergis ERP untuk PT Berkat Agung).
Dibuat menggunakan Mermaid diagram â€” render di GitLab, GitHub, VS Code (Mermaid extension), atau mermaid.live.

---

## 1. Alur Request & Autentikasi

```mermaid
flowchart TD
    A[Browser Request] --> B{URL Path?}
    
    B -->|"/"| C[index.php<br/>Dashboard Utama]
    B -->|"/?module=XXX"| D[main-container.php<br/>Module Router]
    B -->|"/API/resource/action"| E[API/routes.php<br/>REST API]
    B -->|"/tallyikan/"| F[tallyikan/index.php<br/>Modul Produksi]
    B -->|"/m/"| G[m/index.php<br/>Mobile Interface]
    
    C --> H[configuration.inc]
    D --> H
    E --> H
    F --> H
    
    H --> I[config_setup.inc<br/>Load common/, constants,<br/>module_path array]
    I --> J[header.php]
    
    J --> K{Cookie oneLogin<br/>ada & valid?}
    K -->|Tidak| L[Redirect ke<br/>oneLogin/login.php]
    K -->|Ya| M[Init Session:<br/>- cActivityGrantor<br/>- cSynergisUser<br/>- DB Connection]
    
    L --> N[OneLogin SSO]
    N --> O{Login berhasil?}
    O -->|Ya| P[Set Cookies:<br/>oneLogin, onelogin_id<br/>domain: .berkat-agung.com]
    O -->|Tidak| Q[Tampilkan Error Login]
    P --> J
    
    M --> R[navigation.php<br/>Render Top Navbar + Icons]
    R --> S{Lanjut ke mana?}
    S -->|Dashboard| C
    S -->|Module| D
    S -->|API| E
```

---

## 2. Dashboard Utama & Menu Navigasi

```mermaid
flowchart TD
    DASH[index.php â€” Dashboard Utama] --> ACL{Cek Permission<br/>per Menu Item}
    
    ACL --> PEOPLE["<b>People</b><br/>people_dashboard<br/>ACL: people/dashboard"]
    ACL --> KONTAK["<b>Kontak</b><br/>addressbook<br/>ACL: addressbook/main"]
    ACL --> PO_MENU["<b>Pesan Ke Supplier</b><br/>(Dropdown PO)<br/>ACL: po/main"]
    ACL --> SALES_MENU["<b>Penjualan</b><br/>(Dropdown Sales)<br/>ACL: sales/main"]
    ACL --> CRM_MENU["<b>CRM</b><br/>(Dropdown)<br/>ACL: crm/main"]
    ACL --> INV_MENU["<b>Inventaris</b><br/>(Dropdown)<br/>ACL: inventory/main_menu"]
    ACL --> TALLY["<b>Tally Ikan</b><br/>tallyikan/<br/>(Hardcoded)"]
    ACL --> PACK_MENU["<b>Packing</b><br/>(Dropdown)<br/>ACL: warehouse/packing"]
    ACL --> AR["<b>Tagihan Penjualan</b><br/>AR_invoice.php<br/>ACL: sales/AR_main"]
    ACL --> AP["<b>Tagihan Pembelian</b><br/>AP.php<br/>ACL: po/AP_main"]
    ACL --> REPORT["<b>Laporan</b><br/>reports.php<br/>ACL: reports/main_menu"]
    ACL --> ACCT["<b>Accounting</b><br/>ACCOUNTING_WEB_HOME<br/>ACL: accounting/main_page"]
    ACL --> DEV["<b>Perangkat</b><br/>devices/<br/>ACL: devices/main"]
    ACL --> HRD["<b>HRD</b><br/>HRD/<br/>ACL: hrd/main"]
    ACL --> ASSET["<b>Manajemen Aset</b><br/>assets/<br/>ACL: assets/main"]
    ACL --> GIS["<b>GIS</b><br/>gis/<br/>ACL: gis/main"]
    ACL --> ADMIN["<b>Administrasi</b><br/>admin/<br/>ACL: system/admin"]
    
    style TALLY fill:#ff9900,stroke:#cc7700,color:#000
```

---

## 3. Submenu Detail per Modul

### 3a. Purchase Order (Pesan Ke Supplier)

```mermaid
flowchart TD
    PO["<b>Purchase Order</b><br/>ACL: po/main"] --> PO1["Pesan Barang<br/>PO_create<br/>ACL: po/PurchaseOrder_create"]
    PO --> PO2["Komisi<br/>PO_commission<br/>ACL: po/komisi"]
    PO --> PO3["Daftar PO<br/>PO_list<br/>ACL: po/PurchaseOrder_list"]
    PO --> PO4["Kas Kecil<br/>pettycash<br/>ACL: po/kas_kecil"]
    PO --> PO5["Audit<br/>PO_audit<br/>ACL: po/audit"]
    
    PO1 --> PO1a[PurchaseOrder.php?action=create]
    PO3 --> PO3a[PurchaseOrder.php?action=list]
    PO4 --> PO4a[petty_cash.php?action=list]
    PO5 --> PO5a[PurchaseOrder.php?action=audit]
    
    PO3a --> PO_SUB["Sub-modul PO"]
    PO_SUB --> IP["Modifikasi Invoice<br/>PurchaseOrder_InvoiceModify.php"]
    PO_SUB --> IR["Penerimaan Invoice<br/>PurchaseOrder_InvoiceReceipt.php"]
    PO_SUB --> PDO["Delivery Order<br/>purchase_delivery_order.php"]
    PO_SUB --> PP["Pembayaran PO<br/>po_payment.php"]
```

### 3b. Penjualan (Sales)

```mermaid
flowchart TD
    SALES["<b>Penjualan</b><br/>ACL: sales/main"] --> S1["Proposal<br/>sales_proposal/<br/>ACL: sales/salpro_main_menu"]
    SALES --> S2["Daftar Harga<br/>sales_price_list.ui.php<br/>ACL: sales/daftar_harga"]
    SALES --> S3["Buat Baru<br/>sales.php?action=create<br/>ACL: sales/baru"]
    SALES --> S4["Buat Baru - Internal<br/>sales.php?action=create-internal<br/>ACL: sales/baru_internal"]
    SALES --> S5["Daftar<br/>sales.php?action=list<br/>ACL: sales/sales_list"]
    SALES --> S6["Audit<br/>sales.php?action=audit<br/>ACL: sales/audit"]
    SALES --> S7["Pendapatan Lain<br/>misc_revenue/misc_rev.php<br/>ACL: sales/pemasukan_lain"]
    
    S5 --> S_SUB["Sub-modul Sales"]
    S_SUB --> ES["Invoice Penjualan<br/>sales_invoice.php"]
    S_SUB --> SDO["Delivery Order<br/>sales_delivery_order.php"]
    S_SUB --> SP["Pembayaran<br/>sales_payment.php"]
    S_SUB --> SB["Tagihan/AR<br/>AR_invoice.php"]

    S3 --> S3a["POS<br/>sales_POS.php<br/>ACL: sales/main"]
```

### 3c. CRM

```mermaid
flowchart TD
    CRM["<b>CRM</b><br/>ACL: crm/main"] --> C1["CRM Dashboard<br/>crm.php<br/>ACL: crm/main"]
    CRM --> C2["Trouble Ticket<br/>crm.php?module=troubleticket<br/>ACL: crm/trouble_ticket"]
    CRM --> C3["Jadwal Kunjungan<br/>crm.php?module=copier_maintenance<br/>ACL: crm/kunjungan"]
```

### 3d. Inventaris

```mermaid
flowchart TD
    INV["<b>Inventaris</b><br/>ACL: inventory/main_menu"] --> I1["Inventaris<br/>inventory.php"]
    INV --> I2["Paket<br/>inventory_group.ui.php"]
    INV --> I3["Produksi Paket<br/>paket/paket.php"]
    INV --> I4["Transfer Stok<br/>inventory_transfer.php"]
    INV --> I5["Stock Opname<br/>stock_opname/"]
```

### 3e. Packing & Pengiriman

```mermaid
flowchart TD
    PACK["<b>Packing</b><br/>ACL: warehouse/packing"] --> P1["Packing<br/>warehouse/packing.php"]
    PACK --> P2["Perencanaan Rute<br/>route_planner/<br/>ACL: gis/route_planning"]
```

### 3f. Administrasi

```mermaid
flowchart TD
    ADMIN["<b>Admin</b><br/>ACL: system/admin"] --> ADM_BIZ["Administrasi Bisnis"]
    ADMIN --> ADM_FIN["Administrasi Keuangan"]
    ADMIN --> ADM_SYS["Administrasi Sistem"]
    
    ADM_BIZ --> B1["Project Management"]
    ADM_BIZ --> B2["Brand Management"]
    ADM_BIZ --> B3["Category Management"]
    ADM_BIZ --> B4["Unit Management"]
    ADM_BIZ --> B5["Discount Management"]
    ADM_BIZ --> B6["Warehouse Management"]
    ADM_BIZ --> B7["Sales Proposal Stages"]
    ADM_BIZ --> B8["Sales Commission"]
    ADM_BIZ --> B9["File Management"]
    
    ADM_FIN --> F1["Kategori Kas Kecil"]
    ADM_FIN --> F2["Rekening Bank"]
    ADM_FIN --> F3["Cek Bank"]
    ADM_FIN --> F4["Penomoran Faktur"]
    
    ADM_SYS --> S1["SQL Query Interface"]
    ADM_SYS --> S2["Data Migration"]
    ADM_SYS --> S3["User Management"]
    ADM_SYS --> S4["Salesperson Management"]
    ADM_SYS --> S5["ACL Management"]
    ADM_SYS --> S6["Konfigurasi Sistem"]
    ADM_SYS --> S7["Report Templates"]
```

### 3g. Top Navbar Icons

```mermaid
flowchart LR
    NAV[Top Navbar] --> N1["ðŸ  Home"]
    NAV --> N2["CRM"]
    NAV --> N3["Kontak"]
    NAV --> N4["PO"]
    NAV --> N5["Proposal"]
    NAV --> N6["Sales"]
    NAV --> N7["E-Commerce"]
    NAV --> N8["AR"]
    NAV --> N9["AP"]
    NAV --> N10["Laporan"]
    NAV --> N11["Inventaris"]
    NAV --> N12["Packing"]
    NAV --> N13["Perangkat"]
    NAV --> N14["Admin"]
    NAV --> N15["Manual"]
    NAV --> N16["ðŸ”” Commhub"]
    NAV --> N17["âœ‰ï¸ Messaging"]
    NAV --> N18["âš™ï¸ Settings"]
    NAV --> N19["ðŸ“ FileManager"]
    NAV --> N20["ðŸ‘¤ Profil"]
    NAV --> N21["ðŸšª Sign Out"]
```

---

## 4. Alur Bisnis Utama: Procurement â†’ Sales

```mermaid
flowchart TD
    subgraph PROCUREMENT["PROCUREMENT (Pembelian)"]
        PO_CREATE["Buat PO<br/>purchaseorder/"] --> PO_APPROVE["Approve PO"]
        PO_APPROVE --> PDO_CREATE["Buat Delivery Order<br/>(PDO)"]
        PDO_CREATE --> RECEIVE["Terima Barang<br/>+ Update Stok"]
        RECEIVE --> PO_INVOICE["Invoice Pembelian<br/>(IR)"]
        PO_INVOICE --> PO_PAY["Pembayaran<br/>(PP)"]
    end
    
    subgraph INVENTORY["INVENTORY (Stok)"]
        RECEIVE --> STOCK["Stok Masuk<br/>inventory/"]
        STOCK --> TRANSFER["Transfer Antar<br/>Warehouse"]
        STOCK --> OPNAME["Stock Opname"]
        STOCK --> PAKET["Produksi Paket<br/>(Bundle)"]
    end
    
    subgraph SALES_FLOW["SALES (Penjualan)"]
        PROPOSAL["Buat Proposal<br/>sales_proposal/"] --> SO_CREATE["Buat Sales Order<br/>sales.php"]
        SO_CREATE --> SO_APPROVE["Approve SO"]
        SO_APPROVE --> SDO_CREATE["Buat Delivery Order<br/>(SDO)"]
        SDO_CREATE --> DELIVER["Kirim Barang<br/>- Kurangi Stok"]
        DELIVER --> S_INVOICE["Invoice Penjualan<br/>(ES)"]
        S_INVOICE --> S_PAY["Pembayaran<br/>(SP)"]
    end
    
    subgraph FINANCE_FLOW["FINANCE & ACCOUNTING"]
        PO_PAY --> JOURNAL["Journal Entry<br/>accounting/"]
        S_PAY --> JOURNAL
        JOURNAL --> GL["General Ledger"]
        GL --> REPORT_FIN["Laporan Keuangan"]
        
        S_INVOICE --> AR_TRACK["Piutang (AR)<br/>AR_invoice.php"]
        PO_INVOICE --> AP_TRACK["Hutang (AP)<br/>AP.php"]
    end
    
    STOCK --> SO_CREATE
    
    style PROCUREMENT fill:#e3f2fd,stroke:#1565c0
    style INVENTORY fill:#e8f5e9,stroke:#2e7d32
    style SALES_FLOW fill:#fff3e0,stroke:#ef6c00
    style FINANCE_FLOW fill:#fce4ec,stroke:#c62828
```

---

## 5. Alur Produksi Tally Ikan (Modul Custom)

```mermaid
flowchart TD
    subgraph REG["1. REGISTRASI"]
        R1["registertally.php<br/>Daftar tally baru"]
        R1 --> R2["Input: supplier, tanggal,<br/>operator, warehouse"]
        R2 --> R3["Generate Tally ID<br/>Format: XXYYMMDDHH<br/>TL=Normal, RP=Repack,<br/>SP=Spesial, LD=Loading"]
    end

    subgraph TH1["2. PENERIMAAN (TH1)"]
        T1["th1.php<br/>Goods Receipt"]
        T1 --> T2["Buat grup penerimaan<br/>per SKU/jenis ikan"]
        T2 --> T3["Timbang per item<br/>â†’ receiving_records"]
        T3 --> T4["Total berat per grup<br/>â†’ receiving_records_group"]
    end

    subgraph TH2["3. VERIFIKASI (TH2)"]
        V1["th2.php<br/>Gross Weight Verification"]
        V1 --> V2["Verifikasi total berat<br/>per grup"]
        V2 --> V3["â†’ receive_verification_records"]
    end

    subgraph TH3["4. PRA-PROSES (TH3)"]
        PP1["th3.php<br/>Pre-Processing"]
        PP1 --> PP2["Sortir & Grading:<br/>Normal, Memar, Yellow, Reject"]
        PP2 --> PP3["Tipe: BL (Belly),<br/>NBL (Non-Belly)"]
        PP3 --> PP4["â†’ preprocess_records"]
    end

    subgraph PROSES["5. PROSES (15 Jenis)"]
        PR1["pre-th3.php<br/>Penugasan & Eksekusi"]
        PR1 --> PR2["Assign pekerja ke keloter<br/>â†’ process_executors"]
        PR2 --> PR3["15 jenis proses:<br/>Fillet, Skinless, Trim,<br/>Premium Checker, NBL Checker,<br/>Head/Bone, Tail, Belly,<br/>Skin, Dark Flesh, Debris,<br/>Side, Yellow, Bruising, Broken"]
        PR3 --> PR4["Catat hasil per proses<br/>â†’ process_result_records"]
    end

    subgraph SOAK["6. SOAKING"]
        SK1["soaking.php<br/>Perendaman/Curing"]
        SK1 --> SK2["Input: berat sebelum,<br/>berat sesudah, mesin"]
        SK2 --> SK3["â†’ soaking_result_records<br/>â†’ soaking_machine_records"]
    end

    subgraph FREEZE["7. FREEZING"]
        FR1["freezing.php<br/>Pembekuan"]
        FR1 --> FR2["Catat penyusutan berat<br/>sebelum vs sesudah beku"]
        FR2 --> FR3["â†’ freezing_result_records"]
    end

    subgraph GLAZE["8. GLAZING (4 tahap: A-D)"]
        GL1["glazing.php<br/>Pelapisan Es"]
        GL1 --> GL2["4 sub-tahap glazing<br/>(A, B, C, D)"]
        GL2 --> GL3["Assign barcode<br/>per item produk"]
        GL3 --> GL4["â†’ glazing_result_records<br/>â†’ barcode"]
    end

    subgraph PACK["9. PACKING"]
        PK1["packing.php<br/>Pengemasan"]
        PK1 --> PK2["Scan barcode<br/>(validasi: ada, belum dipakai,<br/>qty glazing >= qty packing)"]
        PK2 --> PK3["MC = Master Carton<br/>MCO = Excess"]
        PK3 --> PK4["â†’ packing_result_records"]
    end

    subgraph LOAD["10. LOADING"]
        LD1["muat-lokal.php<br/>Pengiriman"]
        LD1 --> LD2["Scan barcode carton"]
        LD2 --> LD3["Buat dokumen muat<br/>& BPB"]
        LD3 --> LD4["Selesai â€” Barang Terkirim"]
    end

    REG --> TH1
    TH1 --> TH2
    TH2 --> TH3
    TH3 --> PROSES
    PROSES --> SOAK
    SOAK --> FREEZE
    FREEZE --> GLAZE
    GLAZE --> PACK
    PACK --> LOAD

    style REG fill:#e8eaf6,stroke:#3f51b5
    style TH1 fill:#e3f2fd,stroke:#1976d2
    style TH2 fill:#e1f5fe,stroke:#0288d1
    style TH3 fill:#e0f7fa,stroke:#00838f
    style PROSES fill:#e0f2f1,stroke:#00695c
    style SOAK fill:#e8f5e9,stroke:#2e7d32
    style FREEZE fill:#f1f8e9,stroke:#558b2f
    style GLAZE fill:#fffde7,stroke:#f9a825
    style PACK fill:#fff3e0,stroke:#ef6c00
    style LOAD fill:#fbe9e7,stroke:#d84315
```

---

## 6. Barcode Lifecycle (dalam Tallyikan)

```mermaid
flowchart LR
    REQ["Request Barcode<br/>barcode_id_request"] --> GEN["Generate<br/>Barcode ID"]
    GEN --> PRINT["Print QR Code"]
    PRINT --> ASSIGN["Assign di Glazing<br/>glazing_result_records"]
    ASSIGN --> SCAN_PACK["Scan di Packing<br/>AJAX_scan.php"]
    SCAN_PACK --> VALID{Validasi}
    VALID -->|Barcode ada| CHECK{Sudah dipakai?}
    VALID -->|Tidak ada| ERR1["âŒ Barcode tidak ditemukan"]
    CHECK -->|Belum| QTY{qty glazing >= qty packing?}
    CHECK -->|Sudah| ERR2["âŒ Barcode sudah dipakai"]
    QTY -->|Ya| OK["âœ… Packing berhasil<br/>packing_result_records"]
    QTY -->|Tidak| ERR3["âŒ Qty tidak cukup"]
    OK --> SCAN_MUAT["Scan di Loading<br/>AJAX_scan_muat.php"]
    SCAN_MUAT --> SHIP["Barang dimuat & dikirim"]
```

---

## 7. Alur Data Antar Database

```mermaid
flowchart TD
    subgraph DB1["bergung_synergis<br/>localhost:3306"]
        KONTAK["contact_*<br/>Data Supplier & Customer"]
        INV_DB["inventory_*<br/>Master Produk & SKU"]
        SALES_DB["sales_*<br/>Transaksi Penjualan"]
        PO_DB["po_*<br/>Transaksi Pembelian"]
        JOURNAL_DB["journal_*<br/>Akuntansi"]
        WH_DB["warehouse_*<br/>Gudang"]
        USER_DB["user_*<br/>User & Session"]
        EMP_DB["employee_*<br/>Karyawan"]
        CRM_DB["crm_*<br/>CRM Data"]
    end
    
    subgraph DB2["bergung_tally_produksi<br/>192.168.7.1:3306"]
        TALLY_DB["tally_records<br/>Header Tally"]
        RECV_DB["receiving_records*<br/>Penerimaan"]
        PROC_DB["process_result_records<br/>Hasil Proses"]
        SOAK_DB["soaking_result_records<br/>Perendaman"]
        FREEZE_DB["freezing_result_records<br/>Pembekuan"]
        GLAZE_DB["glazing_result_records<br/>Glazing"]
        PACK_DB["packing_result_records<br/>Pengemasan"]
        BC_DB["barcode*<br/>Barcode Tracking"]
    end

    KONTAK -->|"Supplier ID"| TALLY_DB
    INV_DB -->|"item_sku"| RECV_DB
    WH_DB -->|"warehouse_id"| TALLY_DB
    PACK_DB -->|"Stok masuk"| INV_DB
    TALLY_DB -.->|"Referensi"| SALES_DB

    style DB1 fill:#e3f2fd,stroke:#1565c0
    style DB2 fill:#fff3e0,stroke:#ef6c00
```

---

## 8. Alur HRD & Kehadiran

```mermaid
flowchart TD
    HRD["HRD Module<br/>HRD/index.php"] --> EMP["Daftar Karyawan<br/>employee_list.php"]
    HRD --> TS["Timesheet<br/>timesheet_*.php"]
    HRD --> ABSEN["Kehadiran/Absensi<br/>act_record.php"]
    HRD --> PAYROLL["Payroll<br/>Komponen Gaji & Potongan"]
    HRD --> SHIFT["Periode & Shift"]
    HRD --> JAMSOS["Jamsostek<br/>Asuransi Sosial"]
    
    ABSEN --> API_HADIR["API: /API/hadir/<br/>AJAX_catat_act.php"]
```

---

## 9. Integrasi Layanan Eksternal

```mermaid
flowchart TD
    SYNERGIS["Synergis ERP<br/>berkat-agung.com"] --> ONELOGIN["OneLogin SSO<br/>oneLogin/<br/>Cookie: .berkat-agung.com"]
    SYNERGIS --> PHPGACL["phpgacl RBAC<br/>phpgacl/<br/>DB: bergung_acl"]
    SYNERGIS --> WA["WhatsApp Gateway<br/>messaging/ â†’ wa-web-js/<br/>Node.js + Puppeteer"]
    SYNERGIS --> SMS["SMS Gateway<br/>sms.berkat-agung.com"]
    SYNERGIS --> GIS_SVC["GIS Service<br/>gis.berkat-agung.com<br/>Google Maps"]
    SYNERGIS --> BB["BigBrother<br/>bigbrother/<br/>GPS Tracking (Traccar)"]
    SYNERGIS --> ECOM["E-Commerce Sync<br/>Bukalapak & Tokopedia"]
    SYNERGIS --> IMG["Image Server<br/>images/<br/>tinyfilemanager"]
    SYNERGIS --> FINANCE_EXT["Finance Module<br/>FINANCE_WEB_HOME"]
    SYNERGIS --> ACCT_EXT["Accounting<br/>ACCOUNTING_WEB_HOME"]
    
    WA --> WAWEB["whatsapp-web.js 1.11.2<br/>+ puppeteer 13.x"]
    GIS_SVC --> GMAP["Google Maps API"]
    BB --> TRACCAR["Traccar Server"]
```

---

## 10. Alur Environment: Dev â†’ Staging â†’ Live

```mermaid
flowchart LR
    subgraph DEV["aluciodev/ (DEV)"]
        DEV_CODE["Branch: octarian<br/>jQuery + Bootstrap<br/>AJAX polling"]
    end
    
    subgraph BRIDGE["synergis-bridge/ (STAGING)"]
        BRIDGE_CODE["Dual remote:<br/>origin + upstream"]
    end
    
    subgraph LIVE["synergis/ (LIVE)"]
        LIVE_CODE["Branch: main<br/>Tailwind + MQTT<br/>Real-time"]
    end
    
    subgraph DB["Database (SHARED!)"]
        DB_MAIN["bergung_synergis<br/>localhost:3306"]
        DB_TALLY["bergung_tally_produksi<br/>192.168.7.1:3306"]
    end
    
    DEV_CODE -->|"git merge<br/>octarian â†’ main"| BRIDGE_CODE
    BRIDGE_CODE -->|"git pull"| LIVE_CODE
    
    DEV_CODE --> DB_MAIN
    LIVE_CODE --> DB_MAIN
    DEV_CODE --> DB_TALLY
    LIVE_CODE --> DB_TALLY
    
    style DEV fill:#e3f2fd,stroke:#1976d2
    style BRIDGE fill:#fff9c4,stroke:#f9a825
    style LIVE fill:#e8f5e9,stroke:#2e7d32
    style DB fill:#ffcdd2,stroke:#c62828
```

---

## 11. REST API Endpoints

```mermaid
flowchart TD
    API["API/routes.php<br/>POST /API/resource/action"] --> AUTH_API["GET /API/auth/<br/>â†’ AJAX_configs.php"]
    API --> USER_API["POST /API/user<br/>â†’ AJAX_user.php"]
    API --> ACL_API["POST /API/acl<br/>â†’ AJAX_user.php"]
    API --> UI_API["POST /API/UI/<br/>â†’ AJAX_UI.php"]
    API --> KONTAK_API["POST /API/kontak/<br/>â†’ AJAX_kontak.php"]
    API --> INV_API["POST /API/inventory/<br/>â†’ AJAX_inventory.php"]
    API --> SALES_API["POST /API/sales/<br/>â†’ sales/AJAX_sales.php"]
    API --> SALPRO_API["POST /API/salpro/<br/>â†’ AJAX_salpro.php"]
    API --> CAT_API["POST /API/category/<br/>â†’ AJAX_category.php"]
    API --> UNIT_API["POST /API/units/<br/>â†’ AJAX_units.php"]
    API --> WH_API["POST /API/warehouse/<br/>â†’ AJAX_warehouse.php"]
    API --> GIS_API["POST /API/gis/<br/>â†’ AJAX_gis.php"]
    API --> HADIR_API["POST /API/hadir/<br/>â†’ AJAX_catat_act.php"]
    API --> TT_API["POST /API/troubleticket/<br/>â†’ AJAX_trouble_ticket.php"]
    
    subgraph TALLY_API["Tallyikan AJAX (Langsung, bukan via routes.php)"]
        AJAX_T["AJAX_timbang.php<br/>create_tally, create_recv_grp,<br/>save_recv_grp, add_recv_record,<br/>add_prepro_weight,<br/>update_*_result_status"]
        AJAX_S["AJAX_scan.php<br/>scan_packing"]
        AJAX_SM["AJAX_scan_muat.php<br/>scan barcode muat"]
        AJAX_V["AJAX_validation.php<br/>validasi tally"]
        AJAX_VM["AJAX_validation_muat.php<br/>validasi muat"]
    end
```

---

## 12. Laporan Tallyikan

```mermaid
flowchart TD
    RPT["Laporan Produksi"] --> RPT1["Daily Tally Report<br/>daily_tally_report.php<br/>Ringkasan produksi harian"]
    RPT --> RPT2["Daily Process Report<br/>daily_process_tally_report.php<br/>Detail per jenis proses"]
    RPT --> RPT3["Keloter Report<br/>daily_process_tally_keloter_report.php<br/>Per batch/keloter"]
    RPT --> RPT4["Repack Report<br/>daily_tally_repack_report.php<br/>Data pengemasan ulang"]
    RPT --> RPT5["Kinerja Team<br/>kinerja_team.php<br/>Produktivitas per tim"]
    
    RPT1 --> CODE["reports.code.php<br/>repack_reports.code.php"]
    RPT2 --> CODE
    RPT3 --> CODE
    RPT4 --> CODE
    RPT5 --> CODE
```

---

## 13. Ringkasan Alur End-to-End: Dari Ikan Masuk Sampai Terkirim

```mermaid
flowchart TD
    START(("Mulai")) --> SUPPLIER["Ikan tuna datang<br/>dari Supplier"]
    SUPPLIER --> REG_TALLY["1. Registrasi Tally<br/>(TL/RP/SP/LD)"]
    REG_TALLY --> TERIMA["2. Penerimaan & Timbang (TH1)<br/>Catat per SKU, per grup"]
    TERIMA --> VERIF["3. Verifikasi Gross Weight (TH2)"]
    VERIF --> SORTIR["4. Pra-Proses (TH3)<br/>Sortir: Normal/Memar/Yellow/Reject<br/>Tipe: Belly/Non-Belly"]
    SORTIR --> PROSES["5. Proses Produksi<br/>15 jenis: Fillet, Skinless, Trim,<br/>Checker, Head/Bone, Tail, dll.<br/>Per keloter (batch pekerja)"]
    PROSES --> RENDAM["6. Soaking/Perendaman<br/>Curing per mesin"]
    RENDAM --> BEKU["7. Freezing/Pembekuan<br/>Catat penyusutan berat"]
    BEKU --> GLAZE["8. Glazing (4 tahap A-D)<br/>Lapisan es + assign barcode"]
    GLAZE --> KEMAS["9. Packing<br/>Scan barcode â†’ MC/MCO<br/>Validasi: barcode valid & qty cukup"]
    KEMAS --> MUAT["10. Loading/Pengiriman<br/>Scan carton â†’ Dokumen BPB"]
    MUAT --> KIRIM["Barang terkirim<br/>ke Customer/Eksportir"]
    KIRIM --> DONE(("Selesai"))
    
    %% Parallel: data masuk ke Synergis
    KEMAS -.->|"Update stok"| INV_SYNC["Inventory Synergis<br/>bergung_synergis"]
    MUAT -.->|"Link ke SO"| SALES_SYNC["Sales Order<br/>â†’ Invoice â†’ Payment"]
    
    style START fill:#4caf50,stroke:#2e7d32,color:#fff
    style DONE fill:#4caf50,stroke:#2e7d32,color:#fff
    style REG_TALLY fill:#e8eaf6,stroke:#3f51b5
    style TERIMA fill:#e3f2fd,stroke:#1976d2
    style VERIF fill:#e1f5fe,stroke:#0288d1
    style SORTIR fill:#e0f7fa,stroke:#00838f
    style PROSES fill:#e0f2f1,stroke:#00695c
    style RENDAM fill:#e8f5e9,stroke:#2e7d32
    style BEKU fill:#f1f8e9,stroke:#558b2f
    style GLAZE fill:#fffde7,stroke:#f9a825
    style KEMAS fill:#fff3e0,stroke:#ef6c00
    style MUAT fill:#fbe9e7,stroke:#d84315
    style KIRIM fill:#ffccbc,stroke:#bf360c
```

---

*File ini berisi 13 diagram Mermaid yang mencakup seluruh alur kerja Bagindo.*
*Render menggunakan: GitLab Markdown, VS Code (Mermaid Preview), atau https://mermaid.live*
*Terakhir diperbarui: 2 April 2026*
