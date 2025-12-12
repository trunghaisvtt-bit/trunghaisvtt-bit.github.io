<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Công Cụ Tính Chỉ Số BMI</title>
    
    <style>
        /* --- 2. Định dạng CSS (Bắt đầu) --- */
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f9;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
        }

        .container {
            background-color: #ffffff;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
            width: 100%;
            max-width: 450px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 25px;
        }

        .input-group {
            margin-bottom: 20px;
            text-align: left;
        }

        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
            color: #555;
        }

        input[type="number"] {
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
            box-sizing: border-box; 
            font-size: 16px;
        }

        button {
            background-color: #007bff;
            color: white;
            padding: 12px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 18px;
            font-weight: bold;
            transition: background-color 0.3s ease;
            width: 100%;
            margin-top: 10px;
        }

        button:hover {
            background-color: #0056b3;
        }

        .result {
            margin-top: 30px;
            padding: 15px;
            border: 1px solid #ddd;
            border-radius: 5px;
            background-color: #e9ecef;
            min-height: 50px; 
            text-align: center;
        }

        .result p {
            margin: 5px 0;
            font-size: 1.1em;
            font-weight: bold;
        }

        /* Bảng phân loại */
        .bmi-categories {
            margin-top: 30px;
            text-align: left;
        }

        .bmi-categories h2 {
            font-size: 1.2em;
            margin-bottom: 10px;
            color: #333;
        }

        .bmi-categories table {
            width: 100%;
            border-collapse: collapse;
        }

        .bmi-categories th, .bmi-categories td {
            border: 1px solid #dee2e6;
            padding: 8px;
            text-align: center;
        }

        .bmi-categories th {
            background-color: #007bff;
            color: white;
        }
        /* --- 2. Định dạng CSS (Kết thúc) --- */
    </style>
</head>
<body>
    <div class="container">
        <h1>Tính Chỉ Số BMI</h1>
        <div class="input-group">
            <label for="weight">Cân nặng (kg):</label>
            <input type="number" id="weight" step="0.1" min="1" placeholder="Ví dụ: 70" required>
        </div>
        <div class="input-group">
            <label for="height">Chiều cao (cm):</label>
            <input type="number" id="height" step="1" min="50" placeholder="Ví dụ: 175" required>
        </div>
        <button onclick="calculateBMI()">Tính BMI</button>
        
        <div class="result" id="result-display">
            </div>

        <div class="bmi-categories">
            <h2>Phân loại BMI (WHO)</h2>
            <table>
                <thead>
                    <tr>
                        <th>BMI</th>
                        <th>Phân loại</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>&lt; 18.5</td>
                        <td>Thiếu cân</td>
                    </tr>
                    <tr>
                        <td>18.5 - 24.9</td>
                        <td>Bình thường</td>
                    </tr>
                    <tr>
                        <td>25.0 - 29.9</td>
                        <td>Thừa cân</td>
                    </tr>
                    <tr>
                        <td>&ge; 30.0</td>
                        <td>Béo phì</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>

    <script>
        /* --- 3. Logic JavaScript (Bắt đầu) --- */
        /**
         * Hàm tính chỉ số BMI và hiển thị kết quả
         */
        function calculateBMI() {
            // 1. Lấy giá trị đầu vào
            const weightInput = document.getElementById('weight').value;
            const heightInput = document.getElementById('height').value;
            const resultDisplay = document.getElementById('result-display');

            // 2. Kiểm tra dữ liệu hợp lệ
            const weight = parseFloat(weightInput);
            const heightCm = parseFloat(heightInput);

            if (isNaN(weight) || isNaN(heightCm) || weight <= 0 || heightCm <= 0) {
                resultDisplay.innerHTML = '<p style="color: red;">Vui lòng nhập cân nặng và chiều cao hợp lệ!</p>';
                return;
            }

            // 3. Chuyển đổi Chiều cao từ cm sang mét
            const heightM = heightCm / 100; // 1 mét = 100 cm

            // 4. Tính toán BMI: BMI = weight / (height * height)
            const bmi = (weight / (heightM * heightM)).toFixed(2);

            // 5. Phân loại BMI
            let category = '';
            let color = '';

            if (bmi < 18.5) {
                category = 'Thiếu cân';
                color = 'orange';
            } else if (bmi >= 18.5 && bmi <= 24.9) {
                category = 'Bình thường';
                color = 'green';
            } else if (bmi >= 25.0 && bmi <= 29.9) {
                category = 'Thừa cân';
                color = '#ff8c00'; // Darker orange/amber
            } else { // bmi >= 30.0
                category = 'Béo phì';
                color = 'red';
            }

            // 6. Hiển thị kết quả
            resultDisplay.innerHTML = `
                <p>Chỉ số BMI của bạn là: <span style="font-size: 1.5em; color: #007bff;">${bmi}</span></p>
                <p>Phân loại: <span style="color: ${color}; font-weight: bold;">${category}</span></p>
            `;
        }
        /* --- 3. Logic JavaScript (Kết thúc) --- */
    </script>
</body>
</html>
