<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="UTF-8">
<title>Al-Harithiya Calculator</title>
<style>
body { background: #1e1e2f; color: #fff; font-family: Arial; text-align: center; padding: 30px; }
input { padding: 12px; width: 280px; font-size: 18px; margin-top:10px; }
button { padding: 10px 20px; margin-top: 10px; font-size: 16px; background:#4caf50; color:white; border:none; cursor:pointer; border-radius:5px;}
button:hover { background:#45a049; }
p { font-size:18px; }
small { font-size:10px; color:#ccc; }
</style>
</head>
<body>

<h1>Al-Harithiya Calculator</h1>
<input id="input" placeholder="اكتب العملية هنا: 2 + 2 أو 10% من 200">
<br>
<button onclick="calculate()">Confirm</button>

<h3>Result:</h3>
<p id="result"></p>

<h3>Explain:</h3>
<p id="explain"></p>

<small>Made by Ali Saif Ali Mustafa Ahmed Shaheed Amir Ghaith Shawqi</small>

<script>
async function calculate() {
    let input = document.getElementById("input").value;

    // 🔑 ضع مفتاحك هنا
    let apiKey = "PUT_YOUR_API_KEY_HERE";

    let response = await fetch("https://api.openai.com/v1/chat/completions", {
        method: "POST",
        headers: {
            "Authorization": "Bearer " + apiKey,
            "Content-Type": "application/json"
        },
        body: JSON.stringify({
            model: "gpt-4o-mini",
            messages: [
                {
                    role: "system",
                    content: "انت آلة حاسبة ذكية جداً. افهم كل الرموز الرياضية: + - * / ^ و %، وايضاً صياغة العمليات بالكلمات. احسب أولاً، ثم اشرح خطوة خطوة باللغة العربية."
                },
                {
                    role: "user",
                    content: input
                }
            ]
        })
    });

    let data = await response.json();
    let text = data.choices[0].message.content;

    // نفصل النتيجة عن الشرح
    let lines = text.split("\n");
    document.getElementById("result").innerText = lines[0] || "";
    document.getElementById("explain").innerText = lines.slice(1).join("\n") || "";
}
</script>

</body>
</html>
