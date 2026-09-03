<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>ระบบคำนวณเลขชุด</title>

    <style>
        * {
            box-sizing: border-box;
        }

        body {
            margin: 0;
            padding: 20px;
            font-family: Arial, "Tahoma", sans-serif;
            background: #f2f7fb;
            color: #222;
        }

        .container {
            max-width: 500px;
            margin: auto;
        }

        .card {
            background: #fff;
            border-radius: 12px;
            padding: 20px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.08);
        }

        h1 {
            margin: 0 0 20px;
            text-align: center;
            font-size: 25px;
            color: #1565c0;
        }

        .input-group {
            margin-bottom: 15px;
        }

        .input-group label {
            display: block;
            margin-bottom: 7px;
            font-size: 17px;
            font-weight: bold;
        }

        .input-group input {
            width: 100%;
            padding: 12px;
            border: 1px solid #b8c9d6;
            border-radius: 8px;
            font-size: 22px;
            text-align: center;
            letter-spacing: 5px;
            outline: none;
        }

        .input-group input:focus {
            border-color: #1565c0;
        }

        .buttons {
            display: flex;
            gap: 10px;
            margin-top: 15px;
        }

        button {
            flex: 1;
            border: none;
            border-radius: 8px;
            padding: 12px;
            font-size: 17px;
            font-weight: bold;
            cursor: pointer;
        }

        .btn-calc {
            background: #1565c0;
            color: #fff;
        }

        .btn-clear {
            background: #e0e0e0;
            color: #333;
        }

        button:active {
            transform: scale(0.98);
        }

        /* =========================
           ผลลัพธ์
        ========================= */

        .result-box {
            margin-top: 20px;
            display: none;
        }

        .result-row {
            margin-bottom: 14px;
        }

        /* เด่น */
        .main-result {
            width: 100%;
            padding: 12px 8px;
            border-radius: 8px;
            border: 1px solid #c8e1f5;
            background: #f7fcff;
            color: #1565c0;
            text-align: center;
            font-size: 21px;
            font-weight: bold;
            line-height: 1.8;
            white-space: pre-line;
        }

        .result-title {
            font-size: 18px;
            font-weight: bold;
            margin-bottom: 7px;
            color: #333;
        }

        .result-content {
            padding: 10px;
            border-radius: 8px;
            background: #fafafa;
            border: 1px solid #ddd;
            font-size: 20px;
            font-weight: bold;
            text-align: center;
            line-height: 1.8;
            word-break: break-word;
        }

        /* วิน */
        .win-box {
            background: #fff8e1;
            border-color: #f0c36d;
            color: #d84315;
            font-size: 24px;
            letter-spacing: 4px;
        }

        /* ปุ่มคัดลอก */
        .copy-btn {
            width: 100%;
            margin-top: 5px;
            background: #43a047;
            color: white;
        }

        .message {
            margin-top: 10px;
            text-align: center;
            font-size: 14px;
            color: #d32f2f;
            min-height: 20px;
        }

        @media (max-width: 400px) {

            body {
                padding: 10px;
            }

            .card {
                padding: 15px;
            }

            .main-result {
                font-size: 18px;
            }

            .result-content {
                font-size: 18px;
            }
        }
    </style>
</head>

<body>

<div class="container">

    <div class="card">

        <h1> 🦀 น้องคำนวณเลข 🦀 </h1>

        <!-- 3 ตัวบน -->
        <div class="input-group">
            <label for="top3">3 ตัวบน</label>

            <input
                type="text"
                id="top3"
                maxlength="3"
                inputmode="numeric"
                autocomplete="off"
            >
        </div>

        <!-- 2 ตัวล่าง -->
        <div class="input-group">
            <label for="bottom2">2 ตัวล่าง</label>

            <input
                type="text"
                id="bottom2"
                maxlength="2"
                inputmode="numeric"
                autocomplete="off"
            >
        </div>

        <!-- ปุ่ม -->
        <div class="buttons">

            <button
                type="button"
                class="btn-calc"
                onclick="calculate()"
            >
                คำนวณ
            </button>

            <button
                type="button"
                class="btn-clear"
                onclick="clearData()"
            >
                ล้างข้อมูล
            </button>

        </div>

        <div id="message" class="message"></div>


        <!-- =========================
             ผลลัพธ์
             ซ่อนไว้ก่อน
        ========================== -->

        <div
            id="resultBox"
            class="result-box"
        >

            <!-- เด่น -->
            <div class="result-row">

                <div
                    id="res-main"
                    class="main-result"
                ></div>

            </div>


            <!-- จับ 2 ตัว -->
            <div class="result-row">

                <div class="result-title">
                    จับ 2 ตัว
                </div>

                <div
                    id="res-2"
                    class="result-content"
                ></div>

            </div>


            <!-- จับ 3 ตัว -->
            <div class="result-row">

                <div class="result-title">
                    จับ 3 ตัว
                </div>

                <div
                    id="res-3"
                    class="result-content"
                ></div>

            </div>


            <!-- วิน -->
            <div class="result-row">

                <div class="result-title">
                    วิน
                </div>

                <div
                    id="res-win"
                    class="result-content win-box"
                ></div>

            </div>


            <!-- คัดลอก -->
            <button
                type="button"
                class="copy-btn"
                onclick="copyResult()"
            >
                📋 คัดลอกผลลัพธ์
            </button>

        </div>

    </div>

</div>


<script>

    /* =========================================
       รับเฉพาะตัวเลข
    ========================================= */

    document
        .getElementById("top3")
        .addEventListener("input", function () {

            this.value = this.value.replace(/\D/g, "");

        });


    document
        .getElementById("bottom2")
        .addEventListener("input", function () {

            this.value = this.value.replace(/\D/g, "");

        });


    /* =========================================
       Enter จาก 3 ตัวบน
       ไป 2 ตัวล่าง
    ========================================= */

    document
        .getElementById("top3")
        .addEventListener("keydown", function (e) {

            if (e.key === "Enter") {

                e.preventDefault();

                document
                    .getElementById("bottom2")
                    .focus();

            }

        });


    /* =========================================
       Enter จาก 2 ตัวล่าง
       คำนวณ
    ========================================= */

    document
        .getElementById("bottom2")
        .addEventListener("keydown", function (e) {

            if (e.key === "Enter") {

                e.preventDefault();

                calculate();

            }

        });


    /* =========================================
       คำนวณ
    ========================================= */

    function calculate() {

        const top3 =
            document
                .getElementById("top3")
                .value
                .trim();

        const bottom2 =
            document
                .getElementById("bottom2")
                .value
                .trim();

        const message =
            document.getElementById("message");

        const resultBox =
            document.getElementById("resultBox");

        message.textContent = "";


        /* ตรวจสอบ 3 ตัวบน */

        if (top3.length !== 3) {

            resultBox.style.display = "none";

            message.textContent =
                "กรุณากรอก 3 ตัวบนให้ครบ 3 หลัก";

            document
                .getElementById("top3")
                .focus();

            return;
        }


        /* ตรวจสอบ 2 ตัวล่าง */

        if (bottom2.length !== 2) {

            resultBox.style.display = "none";

            message.textContent =
                "กรุณากรอก 2 ตัวล่างให้ครบ 2 หลัก";

            document
                .getElementById("bottom2")
                .focus();

            return;
        }


        /* =========================================
           แยกตัวเลข
        ========================================= */

        const g2 = Number(top3[0]);
        const h2 = Number(top3[1]);

        const j2 = Number(bottom2[0]);
        const k2 = Number(bottom2[1]);


        /* =========================================
           สูตรวิน
        ========================================= */

        const base =
            g2 + h2 + k2;

        const modifiers =
            [4, 2, 7, 8, 9, 5];


        const winArr =
            modifiers.map(function (modifier) {

                return (base + modifier) % 10;

            });


        /* =========================================
           เด่น
        ========================================= */

        const den1 =
            winArr[0];

        const den2 =
            winArr[1];

        const highlight =
            `${den1}${den2}`;


        document
            .getElementById("res-main")
            .textContent =
            `เด่น      ${den1} - ${den2}\n\nเน้นๆๆ      ${highlight}`;


        /* =========================================
           จับ 2 ตัว
        ========================================= */

        const remainingNumbers =
            winArr.slice(2);


        const pair1 =
            remainingNumbers.map(function (num) {

                return `${den1}${num}`;

            });


        const pair2 =
            remainingNumbers.map(function (num) {

                return `${den2}${num}`;

            });


        document
            .getElementById("res-2")
            .innerHTML =
            pair1.join(" ") +
            "<br>" +
            pair2.join(" ");


        /* =========================================
           จับ 3 ตัว
        ========================================= */

        const s1 =
            `${winArr[2]}${winArr[0]}${winArr[3]}`;

        const s2 =
            `${winArr[3]}${winArr[0]}${winArr[4]}`;

        const s3 =
            `${winArr[4]}${winArr[1]}${winArr[2]}`;

        const s4 =
            `${winArr[5]}${winArr[0]}${winArr[1]}`;


        document
            .getElementById("res-3")
            .textContent =
            `${s1} ${s2} ${s3} ${s4}`;


        /* =========================================
           วิน
        ========================================= */

        document
            .getElementById("res-win")
            .textContent =
            winArr.join("");


        /* =========================================
           สำคัญ:
           คำนวณสำเร็จแล้วค่อยแสดงผล
        ========================================= */

        resultBox.style.display = "block";

    }


    /* =========================================
       ล้างข้อมูล
    ========================================= */

    function clearData() {

        document
            .getElementById("top3")
            .value = "";

        document
            .getElementById("bottom2")
            .value = "";


        /* ซ่อนผลลัพธ์อีกครั้ง */

        document
            .getElementById("resultBox")
            .style.display = "none";


        /* ล้างข้อความทั้งหมด */

        document
            .getElementById("res-main")
            .textContent = "";

        document
            .getElementById("res-2")
            .innerHTML = "";

        document
            .getElementById("res-3")
            .textContent = "";

        document
            .getElementById("res-win")
            .textContent = "";

        document
            .getElementById("message")
            .textContent = "";


        document
            .getElementById("top3")
            .focus();

    }


    /* =========================================
       คัดลอกผลลัพธ์
    ========================================= */

    function copyResult() {

        const resultBox =
            document.getElementById("resultBox");


        /* ถ้ายังไม่มีผลลัพธ์ */

        if (resultBox.style.display === "none") {

            document
                .getElementById("message")
                .textContent =
                "กรุณากดคำนวณก่อนคัดลอก";

            return;
        }


        const den =
            document
                .getElementById("res-main")
                .textContent
                .trim();


        const pair =
            document
                .getElementById("res-2")
                .innerText
                .trim();


        const triple =
            document
                .getElementById("res-3")
                .textContent
                .trim();


        const win =
            document
                .getElementById("res-win")
                .textContent
                .trim();


        const text =
`${den}

จับ 2 ตัว
${pair}

จับ 3 ตัว
${triple}

วิน
${win}`;


        navigator.clipboard
            .writeText(text)
            .then(function () {

                document
                    .getElementById("message")
                    .textContent =
                    "คัดลอกผลลัพธ์แล้ว";


                setTimeout(function () {

                    document
                        .getElementById("message")
                        .textContent = "";

                }, 2000);

            })
            .catch(function () {

                document
                    .getElementById("message")
                    .textContent =
                    "ไม่สามารถคัดลอกได้";

            });

    }

</script>

</body>
</html>
