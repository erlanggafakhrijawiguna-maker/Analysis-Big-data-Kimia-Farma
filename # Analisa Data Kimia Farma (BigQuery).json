# Analisa Data Kimia Farma (BigQuery)

## Deskripsi
Project ini berisi query analisis data transaksi Kimia Farma menggunakan Google BigQuery.

## Dataset
- kf_transaksi
- kf_produk
- kf_KantorCabang

## Output Analisa Tabel
- Rating_cabang 
- customer_name 
- product_id 
- product_name 
- actual_price 
- Perhitungan nett sales
- Nett profit
- Kategori rating transaksi
- discount_percentage 
- persentase_gross_laba

## Author
ERLANGGA FAKHRIJA WIGUNA

# Analisis-Table-Kmia-Farma
CREATE OR REPLACE TABLE `rakamin-academy-data-ascience.KimiaFarma.tabel_analisa` AS
SELECT 
    -- 1. Informasi Dasar Transaksi (dari kf_transaksi)
    t.transaction_id,
    t.date,
    t.branch_id,
    t.customer_name,

    
    -- 2. Informasi Cabang (JOIN dengan kf_KantorCabang)
    c.branch_name,
    c.kota,
    c.provinsi,
    
    -- 3. Informasi Produk (JOIN dengan kf_produk)
    t.product_id,
    p.product_name,
    p.price AS actual_price, -- Harga asli dari tabel produk
    
    -- 4. Informasi Finansial Transaksi
    t.discount_percentage,
    
    -- Menghitung Persentase Laba (Sesuai ketentuan harga)
    CASE 
        WHEN p.price <= 50000 THEN 0.10
        WHEN p.price > 50000 AND p.price <= 100000 THEN 0.15
        WHEN p.price > 100000 AND p.price <= 300000 THEN 0.20
        WHEN p.price > 300000 AND p.price <= 500000 THEN 0.25
        ELSE 0.30
    END AS persentase_gross_laba,

    -- Menghitung Nett Sales (Harga setelah diskon)
    (p.price * (1 - t.discount_percentage)) AS nett_sales,

    -- Menghitung Nett Profit (Keuntungan setelah diskon dikali persentase laba)
    ((p.price * (1 - t.discount_percentage)) * CASE 
            WHEN p.price <= 50000 THEN 0.10
            WHEN p.price > 50000 AND p.price <= 100000 THEN 0.15
            WHEN p.price > 100000 AND p.price <= 300000 THEN 0.20
            WHEN p.price > 300000 AND p.price <= 500000 THEN 0.25
            ELSE 0.30
        END
    ) AS nett_profit,

    -- 5. Informasi Rating
    t.rating AS rating_transaksi
    CASE 
        WHEN t.rating >= 4.5 THEN 'Tinggi'
        WHEN t.rating >= 4.0 THEN 'Sedang'
        ELSE 'Rendah'
    END AS kategori_rating

FROM 
    `rakamin-academy-data-ascience.KimiaFarma.kf_transaksi` AS t
LEFT JOIN 
    `rakamin-academy-data-ascience.KimiaFarma.kf_KantorCabang` AS c 
    ON t.branch_id = c.branch_id
LEFT JOIN 
    `rakamin-academy-data-ascience.KimiaFarma.kf_produk` AS p 
    ON t.product_id = p.product_id;