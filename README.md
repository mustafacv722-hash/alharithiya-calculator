# alharithiya-calculator
<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="UTF-8">
<title>Al-Harithiya Calculator</title>

<style>
body {
  background: linear-gradient(135deg, #0f2027, #203a43, #2c5364);
  color: white;
  text-align: center;
  font-family: 'Arial', sans-serif;
  padding: 20px;
}

h1 {
  font-size: 2em;
  margin-bottom: 15px;
  color: #00d4ff;
}

input, select {
  padding: 12px;
  font-size: 16px;
  margin: 5px;
  border-radius: 8px;
  border: none;
  width: 280px;
}

button {
  padding: 12px 20px;
  margin: 5px;
  font-size: 16px;
  cursor: pointer;
  border: none;
  border-radius: 8px;
  background: linear-gradient(90deg, #6a11cb, #2575fc);
  color: white;
  transition: 0.3s;
}

button:hover {
  background: linear-gradient(90deg, #2575fc, #6a11cb);
}

#explain {
  text-align: left;
  margin: 10px auto;
  width: 90%;
  background: rgba(0,0,0,0.6);
  padding: 15px;
  border-radius: 10px;
  white-space: pre-line;
  font-size: 14px;
}

.operations, .physics-buttons {
  margin: 10px auto;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.operations button, .physics-buttons button {
  min-width: 60px;
}
</style>
</head>

<body>

<h1>Al-Harithiya Calculator</h1>

<input id="input" placeholder="اكتب العملية أو اضغط الأزرار">
<br>

<!-- لوحة العمليات -->
<div class="operations">
  <button onclick="addSymbol('+')">+</button>
  <button onclick="addSymbol('-')">-</button>
  <button onclick="addSymbol('×')">×</button>
  <button onclick="addSymbol('÷')">÷</button>
  <button onclick="addSymbol('^')">^</button>
  <button onclick="addSymbol('%')">%</button>
  <button onclick="addSymbol('(')">(</button>
  <button onclick="addSymbol(')')">)</button>
  <button onclick="clearInput()">C</button>
</div>

<!-- قائمة القوانين الفيزيائية -->
<div class="physics-buttons">
  <button onclick="physicsPrompt('F')">القوة: F = m × a</button>
  <button onclick="physicsPrompt('v')">السرعة: v = d / t</button>
  <button onclick="physicsPrompt('E')">الطاقة: E = m × c²</button>
  <button onclick="physicsPrompt('P')">الضغط: P = F / A</button>
</div>

<br>
<button onclick="calculate()">Confirm</button>

<h3>Result:</h3>
<p id="result"></p>

<h3>Step by Step:</h3>
<p id="explain"></p>

<br><br>

<p style="font-size:12px; color: #c0c0c0;">
Made by Ali Saif Ali Mustafa Ahmed Shaheed Amir Ghaith Shawqi<br>
with the assistance of Mustafa Ahmed Khurshid<br>
First Grade A
</p>

<script>
function addSymbol(symbol) {
  document.getElementById("input").value += symbol;
}

function clearInput() {
  document.getElementById("input").value = '';
  document.getElementById("result").innerText = '';
  document.getElementById("explain").innerText = '';
}

function physicsPrompt(law) {
  let m, a, d, t, F, A;
  if(law === "F") {
    m = prompt("أدخل الكتلة (m) بالكيلوغرام:");
    a = prompt("أدخل التسارع (a) بالمتر/ثانية²:");
    let result = parseFloat(m)*parseFloat(a);
    document.getElementById("result").innerText = result + " N";
    document.getElementById("explain").innerText = 
      `1. القانون: F = m × a\n2. التعويض: F = ${m} × ${a}\n3. النتيجة: F = ${result} نيوتن`;
  } else if(law === "v") {
    d = prompt("أدخل المسافة (d) بالمتر:");
    t = prompt("أدخل الزمن (t) بالثواني:");
    let result = parseFloat(d)/parseFloat(t);
    document.getElementById("result").innerText = result + " m/s";
    document.getElementById("explain").innerText = 
      `1. القانون: v = d / t\n2. التعويض: v = ${d} / ${t}\n3. النتيجة: v = ${result} m/s`;
  } else if(law === "E") {
    m = prompt("أدخل الكتلة (m) بالكيلوغرام:");
    const c = 3e8;
    let result = parseFloat(m) * Math.pow(c,2);
    document.getElementById("result").innerText = result + " J";
    document.getElementById("explain").innerText = 
      `1. القانون: E = m × c²\n2. التعويض: E = ${m} × (3×10^8)²\n3. النتيجة: E = ${result} جول`;
  } else if(law === "P") {
    F = prompt("أدخل القوة (F) بالنيوتن:");
    A = prompt("أدخل المساحة (A) بالمتر²:");
    let result = parseFloat(F)/parseFloat(A);
    document.getElementById("result").innerText = result + " Pa";
    document.getElementById("explain").innerText = 
      `1. القانون: P = F / A\n2. التعويض: P = ${F} / ${A}\n3. النتيجة: P = ${result} باسكال`;
  }
}

function calculate() {
  let input = document.getElementById("input").value;
  input = input.replace(/×/g,"*").replace(/÷/g,"/").replace(/\^/g,"**");
  let explanation = "";

  try {
    let result;
    if(input.includes("%")) {
      let parts = input.split("من");
      if(parts.length===2){
        let percent = parseFloat(parts[0].replace("%",""));
        let number = parseFloat(parts[1]);
        result = (percent/100)*number;
        explanation += `1. أخذ النسبة: ${percent}%\n`;
        explanation += `2. ضربها في العدد: ${percent/100} × ${number} = ${result}\n`;
      } else result="صيغة النسبة غير صحيحة";
    } else if(input.match(/[\d\+\-\*\/\(\)\.]/) || input.includes("**")) {
      explanation += "1. تحليل العملية الحسابية\n2. استخدام ترتيب العمليات (PEMDAS)\n";
      result = eval(input);
      explanation += `3. النتيجة = ${result}\n`;
    } else if(input.includes("إذا عندي")) {
      let numbers = input.match(/\d+/g);
      if(numbers && numbers.length===2){
        result = numbers[0]-numbers[1];
        explanation += `1. لديك ${numbers[0]} وصرفت ${numbers[1]}\n2. الطرح: ${numbers[0]} - ${numbers[1]} = ${result}\n`;
      }
    } else result="لا أعرف كيف أحسب هذا";
    document.getElementById("result").innerText = result;
    document.getElementById("explain").innerText = explanation;
  } catch(err) {
    document.getElementById("result").innerText = "Error: "+err;
    document.getElementById("explain").innerText = "";
  }
}
</script>

</body>
</html>
