# Pide presupuesto

> Pide presupuesto para contratar a un empleado en Italia. Tres pasos breves, respuesta en un día laborable.


<section> <span class="eyebrow">Pide presupuesto</span> <h1 id="cuentanos-a-quien-quieres-contratar">Cuéntanos a quién quieres contratar</h1> <p class="lede">Tres pasos breves. Respondemos en un día laborable con el coste completo y el esquema del contrato.</p> </section> <section> <form id="lead" class="panel" style="display:grid;gap:18px;max-width:640px" novalidate> <p id="stepinfo" class="tag" style="justify-self:start">Paso 1 de 3</p> <fieldset data-step="1" style="border:0;padding:0;margin:0;display:grid;gap:14px"> <label for="email">Email de trabajo<input id="email" type="email" autocomplete="email" required></label> <label for="company">Empresa<input id="company" autocomplete="organization"></label> </fieldset> <fieldset data-step="2" hidden style="border:0;padding:0;margin:0;display:grid;gap:14px"> <label for="service">¿Qué necesitas?<select id="service"><option>Employer of Record (EOR)</option><option>Nóminas (tenemos sociedad en Italia)</option><option>Visado de trabajo / Tarjeta Azul</option><option>Selección</option></select></label> <label for="count">¿Cuántas personas en Italia?<input id="count" type="number" min="1" value="1"></label> <label for="city">Ciudad o región<input id="city" placeholder="Milán, Roma, en remoto…"></label> <label for="salary">Salario bruto anual previsto (EUR)<input id="salary" type="number" step="1000"></label> </fieldset> <fieldset data-step="3" hidden style="border:0;padding:0;margin:0;display:grid;gap:14px"> <label for="name">Tu nombre<input id="name" autocomplete="name"></label> <label for="start">Fecha de incorporación prevista<input id="start" type="date"></label> <label for="notes">Algo más<textarea id="notes" rows="4"></textarea></label> <label style="display:flex;gap:10px;align-items:flex-start;font-weight:400"><input id="consent" type="checkbox" style="width:auto;margin-top:5px">Acepto que ContratarEnItalia trate mis datos para responder a esta solicitud (política de privacidad).</label> </fieldset> <p id="msg" role="status" style="margin:0;color:var(--accent)"></p> <div class="cta"><button type="button" class="btn ghost" id="back" hidden>Atrás</button><button type="submit" class="btn" id="next">Continuar</button></div> </form> </section> <script>
// Il passo 1 salva subito l'email (lead abbandonato recuperabile dal nurturing). Endpoint del worker: /api/lead.
const f = document.getElementById('lead'); let step = 1;
// Oggetto dalla pagina partner: ?subject=Partner precompila le note.
{ const sj = new URLSearchParams(location.search).get('subject'); if (sj) document.getElementById('notes').value = '[' + sj.slice(0, 40) + '] '; }
const sets = [...f.querySelectorAll('fieldset')];
const data = () => Object.fromEntries([...f.querySelectorAll('input,select,textarea')].map(e => [e.id, e.type === 'checkbox' ? e.checked : e.value]));
async function save(final){ try { await fetch('/api/lead', {method:'POST', headers:{'Content-Type':'application/json'}, body: JSON.stringify({...data(), step, final, brand:'contratarenitalia'})}); } catch(e){} }
function show(){ sets.forEach(s => s.hidden = +s.dataset.step !== step);
  document.getElementById('stepinfo').textContent = `Paso ${step} de 3`;
  document.getElementById('back').hidden = step === 1;
  document.getElementById('next').textContent = step === 3 ? 'Enviar solicitud' : 'Continuar'; }
// Ripresa dal link nelle email: ?resume=<token> precompila i campi e salta al passo successivo.
(async () => { const tok = new URLSearchParams(location.search).get('resume'); if (!tok) return;
  try { const r = await fetch('/api/lead/' + encodeURIComponent(tok)); if (!r.ok) return; const d = await r.json();
    for (const [k, v] of Object.entries(d)) { const el = document.getElementById(k); if (el && v != null) el.value = v; }
    step = d.step || 2; show(); } catch(e){} })();
document.getElementById('back').onclick = () => { step--; show(); };
f.addEventListener('submit', e => { e.preventDefault(); const m = document.getElementById('msg'); m.textContent = '';
  if (step === 1 && !document.getElementById('email').checkValidity()) { m.textContent = 'Introduce un email de trabajo válido para continuar.'; return; }
  if (step === 3 && !document.getElementById('consent').checked) { m.textContent = 'Marca la casilla de consentimiento para que podamos responderte.'; return; }
  save(step === 3);
  if (step < 3) { step++; show(); } else { f.querySelectorAll('fieldset,.cta').forEach(x => x.hidden = true); m.textContent = 'Gracias. Te responderemos en un día laborable.'; } });
</script>


---
Source: https://contratarenitalia.com/contacto.html
