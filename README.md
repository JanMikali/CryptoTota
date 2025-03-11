<!DOCTYPE html>
<html lang="fa">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>۲۰۰ رمز ارز برتر</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; background: #f4f4f4; margin: 0; padding: 20px; }
        h1 { color: #333; }
        table { width: 90%; margin: 20px auto; border-collapse: collapse; background: white; box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1); }
        th, td { padding: 10px; border: 1px solid #ddd; text-align: center; }
        th { background: #007bff; color: white; }
        .loading { font-size: 18px; color: #555; margin-top: 20px; }
        .container { overflow-x: auto; }
    </style>
</head>
<body>

    <h1>۲۰۰ رمز ارز برتر جهان</h1>
    <div id="loading" class="loading">در حال بارگذاری...</div>
    <div class="container">
        <table id="currencyTable">
            <thead>
                <tr>
                    <th>رتبه</th>
                    <th>نام ارز</th>
                    <th>نماد</th>
                    <th>قیمت (دلار)</th>
                    <th>حجم بازار (دلار)</th>
                    <th>تغییر ۲۴ ساعت (%)</th>
                </tr>
            </thead>
            <tbody></tbody>
        </table>
    </div>

    <script>
        const apiUrl = 'https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=200&page=1&sparkline=false';

        async function fetchCurrencyData() {
            try {
                const response = await fetch(apiUrl);
                const data = await response.json();
                
                const currencyTable = document.getElementById('currencyTable').getElementsByTagName('tbody')[0];
                currencyTable.innerHTML = ""; // پاک کردن محتوای قبلی

                data.forEach((currency, index) => {
                    const row = currencyTable.insertRow();
                    row.innerHTML = `
                        <td>${index + 1}</td>
                        <td>${currency.name}</td>
                        <td>${currency.symbol.toUpperCase()}</td>
                        <td>$${currency.current_price.toLocaleString()}</td>
                        <td>$${currency.market_cap.toLocaleString()}</td>
                        <td style="color: ${currency.price_change_percentage_24h >= 0 ? 'green' : 'red'};">
                            ${currency.price_change_percentage_24h.toFixed(2)}%
                        </td>
                    `;
                });

                document.getElementById('loading').style.display = 'none';
            } catch (error) {
                console.error('Error fetching data:', error);
                document.getElementById('loading').textContent = 'خطا در دریافت اطلاعات!';
            }
        }

        fetchCurrencyData();
        setInterval(fetchCurrencyData, 10000); // بروزرسانی هر 10 ثانیه
    </script>

</body>
</html>
