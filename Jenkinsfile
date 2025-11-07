<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Calculadora rápida</title>
  <style>
    :root{font-family:system-ui,-apple-system,Segoe UI,Roboto,"Helvetica Neue",Arial;}
    body{display:flex;align-items:center;justify-content:center;min-height:100vh;background:#f4f6f8;margin:0}
    .calc{background:#fff;padding:18px;border-radius:12px;box-shadow:0 6px 18px rgba(20,30,50,0.08);width:320px}
    h1{font-size:18px;margin:0 0 12px;text-align:center}
    .row{display:flex;gap:8px;margin-bottom:10px}
    input[type="number"]{flex:1;padding:10px;border:1px solid #d8dee6;border-radius:8px;font-size:16px}
    button{flex:1;padding:10px;border-radius:8px;border:0;font-size:16px;cursor:pointer;background:#0b72ff;color:#fff}
    button.op{background:#1f9d7a}
    button.clear{background:#ef5350}
    .result{margin-top:12px;padding:10px;border-radius:8px;background:#f1f5f9;border:1px solid #e6eef7;text-align:center;font-weight:600}
    .small{font-size:13px;color:#6b7280;text-align:center;margin-top:8px}
  </style>
</head>
<body>
  <div class="calc" role="application" aria-label="Calculadora básica">
    <h1>Calculadora rápida</h1>

    <div class="row">
      <input id="n1" type="number" step="any" placeholder="Número 1" aria-label="Número 1">
      <input id="n2" type="number" step="any" placeholder="Número 2" aria-label="Número 2">
    </div>

    <div class="row">
      <button id="sum">+</button>
      <button id="sub" class="op">−</button>
      <button id="mul" class="op">×</button>
      <button id="div" class="op">÷</button>
    </div>

    <div class="row">
      <button id="clear" class="clear">Limpiar</button>
      <button id="equals" style="background:#374151">Calcular</button>
    </div>

    <div id="output" class="result" aria-live="polite">Resultado: —</div>
    <div class="small">Puedes usar teclado: Tab para moverte, Enter para calcular.</div>
  </div>

  <script>
    // Elementos
    const n1 = document.getElementById('n1');
    const n2 = document.getElementById('n2');
    const output = document.getElementById('output');
    let operacion = null;

    // Helpers
    function parseValues() {
      const a = parseFloat(n1.value);
      const b = parseFloat(n2.value);
      if (Number.isNaN(a) || Number.isNaN(b)) {
        return { valid: false };
      }
      return { valid: true, a, b };
    }

    function show(msg) {
      output.textContent = 'Resultado: ' + msg;
    }

    function calculate(op) {
      const vals = parseValues();
      if (!vals.valid) {
        show('Por favor ingresa ambos números.');
        return;
      }
      const { a, b } = vals;
      let res;
      switch (op) {
        case '+': res = a + b; break;
        case '-': res = a - b; break;
        case '*': res = a * b; break;
        case '/':
          if (b === 0) { show('Error: división entre cero.'); return; }
          res = a / b;
          break;
        default: show('Operación desconocida'); return;
      }
      // Presentar de forma legible: si es entero, sin decimales; si no, 6 decimales max sin ceros sobrantes
      const formatted = Number.isInteger(res) ? res.toString() : parseFloat(res.toFixed(6)).toString();
      show(formatted);
    }

    // Botones
    document.getElementById('sum').addEventListener('click', () => { operacion = '+'; calculate('+'); });
    document.getElementById('sub').addEventListener('click', () => { operacion = '-'; calculate('-'); });
    document.getElementById('mul').addEventListener('click', () => { operacion = '*'; calculate('*'); });
    document.getElementById('div').addEventListener('click', () => { operacion = '/'; calculate('/'); });

    document.getElementById('equals').addEventListener('click', () => {
      // Si ya se seleccionó una operación por botones, usa esa; si no, intenta suma por defecto
      if (operacion) calculate(operacion);
      else calculate('+');
    });

    document.getElementById('clear').addEventListener('click', () => {
      n1.value = '';
      n2.value = '';
      operacion = null;
      show('—');
      n1.focus();
    });

    // Soporte teclado: Enter en cualquiera de los inputs calcula (usa última operación o suma por defecto)
    [n1, n2].forEach(el => {
      el.addEventListener('keydown', (e) => {
        if (e.key === 'Enter') {
          e.preventDefault();
          if (operacion) calculate(operacion);
          else calculate('+');
        }
      });
    });

    // Mejora UX: marcar operación al hacer click largo (opcional visual)
    const ops = { sum: document.getElementById('sum'), sub: document.getElementById('sub'), mul: document.getElementById('mul'), div: document.getElementById('div') };
    Object.values(ops).forEach(btn => {
      btn.addEventListener('click', () => {
        Object.values(ops).forEach(b => b.style.outline = 'none');
        btn.style.outline = '3px solid rgba(11,114,255,0.12)';
      });
    });
  </script>
</body>
</html>
