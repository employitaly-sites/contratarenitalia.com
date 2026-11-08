# Calculadora de salario neto en Italia

> Del bruto al neto en Italia en 2026: seguridad social del empleado, IRPF italiano (IRPEF) con deducciones por trabajo, recargos regionales y municipales, para Milán, Roma, Turín, Bolonia y Nápoles.


<section> <span class="eyebrow">Calculadora de neto</span> <h1 id="cuanto-cobra-tu-empleado-en-italia">Cuánto cobra en neto tu empleado en Italia</h1> <p class="lede">Introduce el salario bruto anual y la ciudad. La estimación aplica las reglas de 2026: seguridad social del empleado, impuesto sobre la renta con deducciones por trabajo y recargos locales.</p> </section> <section> <div class="grid"> <form class="panel" id="nf" style="display:grid;gap:16px"> <label for="g">Salario bruto anual, RAL (EUR)<input id="g" type="number" min="10000" step="1000" value="50000"></label> <label for="city">Ciudad<select id="city"><option value="Milan">Milán</option><option value="Rome">Roma</option><option value="Turin">Turín</option><option value="Bologna">Bolonia</option><option value="Naples">Nápoles</option></select></label> <label for="inst">Pagado en<select id="inst"><option value="13">13 mensualidades</option><option value="14">14 mensualidades</option><option value="12">12 (13.ª/14.ª a prorrata)</option></select></label> </form> <div class="panel" style="min-width:0"><div class="tbl"><table id="nt"><thead><tr><th>Concepto</th><th class="num">Al año</th></tr></thead><tbody></tbody></table></div> <p id="nm" style="margin-bottom:0;font-weight:600"></p></div> </div>  </section> <section class="prose"> <h2 id="como-funciona-el-calculo">Cómo funciona el cálculo</h2> <ol> <li>El empleado paga un 9,19 % de seguridad social sobre el bruto, más un 1 % sobre la parte que supera 56.224 € al año.</li> <li>El IRPEF se aplica a lo que queda: 23 % hasta 28.000 €, 33 % de 28.000 € a 50.000 €, 43 % por encima.</li> <li>Las deducciones por trabajo reducen el impuesto. Son mayores en salarios bajos y llegan a cero en 50.000 €. Entre 20.000 € y 40.000 € se aplica una deducción adicional de hasta 1.000 €.</li> <li>Se suman los recargos regional y municipal, salvo que la renta quede por debajo del umbral de exención de la ciudad.</li> </ol> <p>Nada de esto cambia lo que pagas como empresa: consulta la <a href="calculadora.html">calculadora de coste para la empresa</a>.</p> </section> <script>(function(){const N = {"status":"secondary","source":"data/params/2026.yaml (irpef, inps, addizionali); TUIR art. 11 and 13; L. 207/2024; L. 199/2025","employee_inps":0.0919,"extra_1pct_above":56224,"irpef":[[28000,0.23],[50000,0.33],[null,0.43]],"cities":{"Milan":{"region":"Lombardía","regional":[[null,0.0123]],"municipal":0.008,"exempt":23000},"Rome":{"region":"Lacio","regional":[[28000,0.0173],[null,0.0333]],"municipal":0.009,"exempt":14000},"Turin":{"region":"Piamonte","regional":[[null,0.0162]],"municipal":0.008,"municipal_note":"0.8% to 1.2% by band; 0.8% used","exempt":11790},"Bologna":{"region":"Emilia-Romagna","regional":[[null,0.0133]],"municipal":0.008,"exempt":15000},"Naples":{"region":"Campania","regional":[[null,0.0173]],"municipal":0.01,"exempt":12000}}};

const eur = n => (n < 0 ? '−€' : '€') + Math.round(Math.abs(n)).toLocaleString('en-US');
function bands(x, b){ let t = 0, lo = 0; for (const [hi, r] of b){ const top = hi ?? Infinity; if (x > lo) t += (Math.min(x, top) - lo) * r; lo = top; } return t; }
function credit(r){ if (r <= 15000) return 1955; if (r <= 28000) return 1910 + 1190 * (28000 - r) / 13000; if (r <= 50000) return 1910 * (50000 - r) / 22000 + (r > 25000 && r <= 35000 ? 65 : 0); return 0; }
function extra(r){ if (r > 20000 && r <= 32000) return 1000; if (r > 32000 && r < 40000) return 1000 * (40000 - r) / 8000; return 0; }
function calc(){
  const g = +document.getElementById('g').value || 0, c = N.cities[document.getElementById('city').value], n = +document.getElementById('inst').value;
  const inps = g * N.employee_inps + Math.max(0, g - N.extra_1pct_above) * 0.01;
  const tx = g - inps;
  const irpef = Math.max(0, bands(tx, N.irpef) - credit(tx) - extra(tx));
  const reg = bands(tx, c.regional), mun = tx > c.exempt ? tx * c.municipal : 0;
  const net = g - inps - irpef - reg - mun;
  const rows = [['Salario bruto', g], ['Seguridad social del empleado', -inps], ['IRPEF tras deducciones', -irpef], [`Recargo regional (${c.region})`, -reg], ['Recargo municipal', -mun]];
  document.querySelector('#nt tbody').innerHTML = rows.map(([k, v]) => `<tr><td>${k}</td><td class="num">${eur(v)}</td></tr>`).join('') + `<tr><td><b>Salario neto</b></td><td class="num"><b>${eur(net)}</b></td></tr>`;
  document.getElementById('nm').textContent = `Unos ${eur(net / n)} netos en cada una de las ${n} nóminas (${(net / g * 100).toFixed(0)} % del bruto).`;
}
document.getElementById('nf').addEventListener('input', calc); document.getElementById('nf').addEventListener('submit', e => e.preventDefault()); calc();
})();</script>


---
Source: https://contratarenitalia.com/calculadora-salario-neto.html
