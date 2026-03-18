import { useState, useRef, useEffect } from "react";

const FONT = "https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=DM+Mono:wght@300;400;500&display=swap";

const CATEGORIES = [
  "animal","corpo humano","cozinha","natureza","esporte","transporte","roupa",
  "construção","escola","música","tecnologia","jardim","praia","floresta",
  "fazenda","instrumento musical","ferramenta","móvel","fruta","vegetal",
  "profissão","clima","hobby","mar"
];

const DIFFICULTY = {
  facil:  { label: "Fácil",   desc: "Categoria + primeira letra + número de letras disponíveis",  color: "#4ade80" },
  medio:  { label: "Médio",   desc: "Categoria + número de letras disponíveis",                    color: "#facc15" },
  dificil:{ label: "Difícil", desc: "Apenas categoria disponível",                                  color: "#f87171" },
};

async function getSecretWord(difficulty) {
  const category = CATEGORIES[Math.floor(Math.random() * CATEGORIES.length)];
  const seed = Math.floor(Math.random() * 99999);
  const complexityNote = difficulty === "dificil"
    ? "Prefira palavras menos comuns, mais específicas."
    : difficulty === "medio"
    ? "Prefira palavras de dificuldade média."
    : "Prefira palavras comuns e simples.";

  const res = await fetch("https://api.anthropic.com/v1/messages", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      model: "claude-sonnet-4-20250514",
      max_tokens: 80,
      system: "Você gera palavras aleatórias para jogos. Varie sempre. Nunca repita.",
      messages: [{
        role: "user",
        content: `Semente: ${seed}. Categoria: "${category}". ${complexityNote}
Escolha 1 substantivo concreto e simples dessa categoria, em português.
Use a semente ${seed} para garantir variedade.
Responda APENAS com JSON: {"word":"palavra","category":"${category}"}`
      }]
    })
  });
  const data = await res.json();
  const text = data.content?.[0]?.text || "";
  try {
    const parsed = JSON.parse(text.replace(/```json|```/g, "").trim());
    if (parsed.word && !parsed.word.includes(" ")) return parsed;
    throw new Error();
  } catch {
    const fb = ["martelo","janela","espelho","tapete","garrafa","chapéu","relógio","pincel","escada","travesseiro"];
    return { word: fb[seed % fb.length], category };
  }
}

async function getWordHint(word, category) {
  const res = await fetch("https://api.anthropic.com/v1/messages", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      model: "claude-sonnet-4-20250514",
      max_tokens: 100,
      messages: [{
        role: "user",
        content: `Crie uma dica curta e enigmática para a palavra "${word}" (categoria: ${category}) no jogo Contexto.
A dica NÃO pode conter a palavra nem suas variações. Deve ter no máximo 12 palavras. Seja criativo e indireto.
Responda APENAS com JSON: {"hint":"sua dica aqui"}`
      }]
    })
  });
  const data = await res.json();
  const text = data.content?.[0]?.text || "";
  try {
    const p = JSON.parse(text.replace(/```json|```/g, "").trim());
    return p.hint || "Nenhuma dica disponível.";
  } catch { return "Nenhuma dica disponível."; }
}

async function getSimilarity(guess, secret) {
  const res = await fetch("https://api.anthropic.com/v1/messages", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      model: "claude-sonnet-4-20250514",
      max_tokens: 60,
      messages: [{
        role: "user",
        content: `Similaridade semântica em português entre "${secret}" e "${guess}".
Score 0-1000: 1000=exata, 900-999=sinônimo, 700-899=muito relacionado, 400-699=relacionado, 100-399=pouco relacionado, 0-99=sem relação.
Responda SOMENTE com JSON: {"score": número}`
      }]
    })
  });
  const data = await res.json();
  const text = data.content?.[0]?.text || '{"score":0}';
  try {
    const p = JSON.parse(text.replace(/```json|```/g, "").trim());
    return Math.min(1000, Math.max(0, p.score ?? 0));
  } catch { return 0; }
}

function scoreColor(s) {
  if (s >= 900) return "#4ade80";
  if (s >= 700) return "#facc15";
  if (s >= 400) return "#fb923c";
  return "#64748b";
}

function scoreLabel(s) {
  if (s === 1000) return "exato";
  if (s >= 900) return "muito quente";
  if (s >= 700) return "quente";
  if (s >= 400) return "morno";
  if (s >= 100) return "frio";
  return "gelado";
}

export default function Contexto() {
  const [phase, setPhase] = useState("select"); // select | loading | playing | won
  const [difficulty, setDifficulty] = useState(null);
  const [secret, setSecret] = useState(null);
  const [guesses, setGuesses] = useState([]);
  const [input, setInput] = useState("");
  const [checking, setChecking] = useState(false);
  const [error, setError] = useState("");
  const [showCatHint, setShowCatHint] = useState(false);
  const [wordHint, setWordHint] = useState(null);
  const [loadingHint, setLoadingHint] = useState(false);
  const [hintRevealed, setHintRevealed] = useState(false);
  const [gameId, setGameId] = useState(0);
  const inputRef = useRef(null);

  useEffect(() => {
    const link = document.createElement("link");
    link.rel = "stylesheet"; link.href = FONT;
    document.head.appendChild(link);
  }, []);

  async function startGame(diff) {
    setDifficulty(diff);
    setPhase("loading");
    setGuesses([]);
    setInput("");
    setError("");
    setShowCatHint(false);
    setWordHint(null);
    setHintRevealed(false);
    const data = await getSecretWord(diff);
    setSecret(data);
    setPhase("playing");
    setTimeout(() => inputRef.current?.focus(), 100);
  }

  function newGame() {
    setPhase("select");
    setDifficulty(null);
    setSecret(null);
    setGuesses([]);
    setGameId(n => n + 1);
  }

  async function revealWordHint() {
    if (wordHint || loadingHint) return;
    setLoadingHint(true);
    const hint = await getWordHint(secret.word, secret.category);
    setWordHint(hint);
    setLoadingHint(false);
    setHintRevealed(true);
  }

  async function submit() {
    const word = input.trim().toLowerCase();
    if (!word || checking) return;
    if (guesses.find(g => g.word === word)) {
      setError("Já tentada."); return;
    }
    setError(""); setChecking(true); setInput("");

    const isExact = word === secret.word.toLowerCase();
    const score = isExact ? 1000 : await getSimilarity(word, secret.word);
    const next = [...guesses, { word, score }].sort((a, b) => b.score - a.score);
    setGuesses(next);
    setChecking(false);
    if (score === 1000) setPhase("won");
    else setTimeout(() => inputRef.current?.focus(), 50);
  }

  const best = guesses[0]?.score ?? 0;
  const diff = difficulty ? DIFFICULTY[difficulty] : null;

  // hint availability by difficulty
  const canCatHint = true;
  const canWordHint = true;
  const showLetterCount = difficulty === "facil" || difficulty === "medio";
  const showFirstLetter = difficulty === "facil";

  return (
    <>
      <style>{`
        *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
        body { background: #18181b; }

        .root {
          min-height: 100vh;
          background: #18181b;
          font-family: 'DM Mono', monospace;
          color: #e4e4e7;
          display: flex;
          flex-direction: column;
          align-items: center;
          padding: 0 20px 100px;
        }

        .header {
          width: 100%; max-width: 500px;
          display: flex;
          align-items: center;
          justify-content: space-between;
          padding: 40px 0 32px;
          border-bottom: 1px solid #2d2d30;
        }

        .title {
          font-family: 'DM Serif Display', serif;
          font-size: 26px;
          letter-spacing: -0.5px;
          color: #f4f4f5;
        }
        .title em { font-style: italic; color: #52525b; }

        .header-right { display: flex; align-items: center; gap: 8px; }

        .diff-badge {
          font-size: 10px;
          letter-spacing: .08em;
          text-transform: uppercase;
          padding: 4px 9px;
          border-radius: 4px;
          border: 1px solid;
          font-family: 'DM Mono', monospace;
        }

        .new-btn {
          font-family: 'DM Mono', monospace;
          font-size: 10px;
          letter-spacing: 0.1em;
          color: #71717a;
          text-transform: uppercase;
          background: none;
          border: 1px solid #3f3f46;
          border-radius: 5px;
          padding: 6px 11px;
          cursor: pointer;
          transition: color .15s, border-color .15s;
        }
        .new-btn:hover { color: #e4e4e7; border-color: #71717a; }
        .new-btn:disabled { opacity: .25; cursor: default; }

        .body { width: 100%; max-width: 500px; padding-top: 32px; }

        /* ── Difficulty select ── */
        .select-screen {
          padding: 48px 0 0;
        }
        .select-title {
          font-family: 'DM Serif Display', serif;
          font-size: 20px;
          color: #a1a1aa;
          margin-bottom: 6px;
        }
        .select-sub {
          font-size: 11px; color: #52525b;
          letter-spacing: .08em; text-transform: uppercase;
          margin-bottom: 32px;
        }
        .diff-cards { display: flex; flex-direction: column; gap: 10px; }

        .diff-card {
          background: #1f1f23;
          border: 1px solid #2d2d30;
          border-radius: 9px;
          padding: 18px 20px;
          cursor: pointer;
          display: flex;
          align-items: center;
          justify-content: space-between;
          transition: border-color .15s, background .15s;
        }
        .diff-card:hover { background: #27272a; border-color: #52525b; }

        .dc-left {}
        .dc-name {
          font-family: 'DM Serif Display', serif;
          font-size: 18px;
          color: #f4f4f5;
          margin-bottom: 4px;
        }
        .dc-desc { font-size: 11px; color: #71717a; letter-spacing: .04em; }
        .dc-arrow { font-size: 18px; color: #3f3f46; }

        /* ── Loading ── */
        .loading {
          padding: 64px 0;
          display: flex; flex-direction: column; align-items: center; gap: 16px;
          font-size: 11px; color: #52525b; letter-spacing: .1em; text-transform: uppercase;
        }
        .dots span {
          display: inline-block; width: 4px; height: 4px;
          background: #3f3f46; border-radius: 50%; margin: 0 3px;
          animation: dp 1s ease-in-out infinite;
        }
        .dots span:nth-child(2) { animation-delay: .18s; }
        .dots span:nth-child(3) { animation-delay: .36s; }
        @keyframes dp {
          0%,100% { opacity:.2; transform:scale(.7); }
          50% { opacity:1; transform:scale(1); }
        }

        /* ── Input ── */
        .input-row { display: flex; gap: 8px; margin-bottom: 6px; }

        .inp {
          flex: 1;
          font-family: 'DM Mono', monospace;
          font-size: 14px;
          color: #e4e4e7;
          background: #1f1f23;
          border: 1px solid #3f3f46;
          border-radius: 7px;
          padding: 11px 14px;
          outline: none;
          transition: border-color .15s;
        }
        .inp::placeholder { color: #3f3f46; }
        .inp:focus { border-color: #71717a; }

        .send {
          font-family: 'DM Mono', monospace;
          font-size: 12px;
          color: #18181b;
          background: #e4e4e7;
          border: none;
          border-radius: 7px;
          padding: 11px 18px;
          cursor: pointer;
          letter-spacing: .04em;
          transition: opacity .15s;
        }
        .send:disabled { opacity: .2; cursor: default; }
        .send:hover:not(:disabled) { opacity: .8; }

        .err { font-size: 11px; color: #f87171; margin-bottom: 14px; letter-spacing: .04em; }

        /* ── Meta ── */
        .meta {
          display: flex; gap: 18px; margin-bottom: 20px;
          font-size: 11px; color: #52525b; letter-spacing: .06em; text-transform: uppercase;
        }
        .meta strong { color: #a1a1aa; font-weight: 500; }

        /* ── Hints panel ── */
        .hints-panel {
          background: #1f1f23;
          border: 1px solid #2d2d30;
          border-radius: 8px;
          padding: 14px 16px;
          margin-bottom: 24px;
          display: flex;
          flex-direction: column;
          gap: 10px;
        }

        .hint-row {
          display: flex; align-items: center; gap: 10px;
        }

        .hint-trigger {
          font-family: 'DM Mono', monospace;
          font-size: 10px; letter-spacing: .08em; text-transform: uppercase;
          color: #71717a; background: none;
          border: 1px solid #3f3f46; border-radius: 5px;
          padding: 5px 10px; cursor: pointer;
          transition: color .15s, border-color .15s;
          white-space: nowrap;
        }
        .hint-trigger:hover:not(:disabled) { color: #e4e4e7; border-color: #71717a; }
        .hint-trigger:disabled { opacity: .3; cursor: default; }

        .hint-val {
          font-size: 11px; color: #a1a1aa; letter-spacing: .04em;
          animation: fi .2s ease;
          line-height: 1.5;
        }
        .hint-val strong { color: #e4e4e7; }

        .hint-meta {
          font-size: 10px; color: #3f3f46; letter-spacing: .06em; text-transform: uppercase;
          margin-bottom: -4px;
        }

        /* ── Bar ── */
        .bar { height: 1.5px; background: #27272a; border-radius: 99px; margin-bottom: 28px; }
        .bar-fill {
          height: 100%; background: #71717a; border-radius: 99px;
          transition: width .5s cubic-bezier(.4,0,.2,1);
        }

        /* ── List ── */
        .col-heads {
          display: grid;
          grid-template-columns: 20px 1fr 56px 88px;
          gap: 10px;
          padding: 0 2px 8px;
          font-size: 9px; color: #3f3f46; letter-spacing: .12em; text-transform: uppercase;
          border-bottom: 1px solid #27272a;
          margin-bottom: 4px;
        }

        .row {
          display: grid;
          grid-template-columns: 20px 1fr 56px 88px;
          gap: 10px;
          align-items: center;
          padding: 10px 2px;
          border-bottom: 1px solid #1f1f23;
          animation: fi .2s ease;
        }
        @keyframes fi {
          from { opacity:0; transform:translateY(5px); }
          to { opacity:1; transform:translateY(0); }
        }

        .r-num { font-size: 10px; color: #3f3f46; text-align: center; }
        .r-word { font-size: 13px; font-weight: 500; color: #e4e4e7; letter-spacing: .02em; }
        .r-score { font-size: 13px; font-weight: 500; text-align: right; }
        .r-lbl { font-size: 10px; color: #52525b; text-align: right; letter-spacing: .06em; }

        .checking {
          display: flex; align-items: center; gap: 10px;
          padding: 11px 2px;
          border-bottom: 1px solid #1f1f23;
          font-size: 11px; color: #3f3f46; letter-spacing: .06em;
        }

        .empty {
          padding: 48px 0; text-align: center;
          font-size: 11px; color: #3f3f46; letter-spacing: .1em; text-transform: uppercase;
        }

        /* ── Win ── */
        .win {
          background: #1f1f23;
          border: 1px solid #2d2d30;
          border-radius: 10px;
          padding: 36px 28px;
          text-align: center;
          margin-bottom: 28px;
          animation: fi .35s ease;
        }
        .win-eyebrow { font-size: 10px; color: #52525b; letter-spacing: .12em; text-transform: uppercase; margin-bottom: 14px; }
        .win-word {
          font-family: 'DM Serif Display', serif;
          font-size: 56px; letter-spacing: -1px;
          color: #f4f4f5; line-height: 1; margin-bottom: 6px;
        }
        .win-cat { font-size: 11px; color: #52525b; letter-spacing: .08em; margin-bottom: 28px; }
        .win-rule { height: 1px; background: #27272a; margin-bottom: 24px; }
        .win-stats { display: flex; justify-content: center; gap: 48px; margin-bottom: 28px; }
        .ws-label { font-size: 10px; color: #52525b; letter-spacing: .1em; text-transform: uppercase; margin-bottom: 4px; }
        .ws-val { font-family: 'DM Serif Display', serif; font-size: 36px; color: #f4f4f5; }
        .play-again {
          font-family: 'DM Mono', monospace; font-size: 11px;
          color: #e4e4e7; background: none;
          border: 1px solid #3f3f46; border-radius: 7px;
          padding: 9px 22px; cursor: pointer;
          letter-spacing: .08em; text-transform: uppercase;
          transition: background .15s, border-color .15s;
        }
        .play-again:hover { background: #27272a; border-color: #71717a; }
      `}</style>

      <div className="root">
        <div className="header">
          <div className="title">Contexto<em>.</em></div>
          <div className="header-right">
            {diff && (
              <span className="diff-badge" style={{ color: diff.color, borderColor: diff.color + "44" }}>
                {diff.label}
              </span>
            )}
            {phase !== "select" && (
              <button className="new-btn" disabled={phase === "loading"} onClick={newGame}>
                Novo jogo
              </button>
            )}
          </div>
        </div>

        <div className="body">

          {/* ── Difficulty selection ── */}
          {phase === "select" && (
            <div className="select-screen">
              <div className="select-title">Escolha a dificuldade</div>
              <div className="select-sub">Isso define quais dicas estarão disponíveis</div>
              <div className="diff-cards">
                {Object.entries(DIFFICULTY).map(([key, d]) => (
                  <div className="diff-card" key={key} onClick={() => startGame(key)}>
                    <div className="dc-left">
                      <div className="dc-name" style={{ color: d.color }}>{d.label}</div>
                      <div className="dc-desc">{d.desc}</div>
                    </div>
                    <div className="dc-arrow">›</div>
                  </div>
                ))}
              </div>
            </div>
          )}

          {/* ── Loading ── */}
          {phase === "loading" && (
            <div className="loading">
              <div className="dots"><span /><span /><span /></div>
              <span>Gerando palavra</span>
            </div>
          )}

          {/* ── Game ── */}
          {(phase === "playing" || phase === "won") && secret && (
            <>
              {phase === "won" && (
                <div className="win">
                  <div className="win-eyebrow">Palavra encontrada</div>
                  <div className="win-word">{secret.word}</div>
                  <div className="win-cat">{secret.category} · {diff?.label}</div>
                  <div className="win-rule" />
                  <div className="win-stats">
                    <div>
                      <div className="ws-label">Tentativas</div>
                      <div className="ws-val">{guesses.length}</div>
                    </div>
                    {hintRevealed && (
                      <div>
                        <div className="ws-label">Dica usada</div>
                        <div className="ws-val" style={{ fontSize: 22, paddingTop: 6 }}>sim</div>
                      </div>
                    )}
                  </div>
                  <button className="play-again" onClick={newGame}>Jogar novamente</button>
                </div>
              )}

              {phase === "playing" && (
                <>
                  <div className="input-row">
                    <input
                      ref={inputRef}
                      className="inp"
                      type="text"
                      placeholder="Digite uma palavra..."
                      value={input}
                      onChange={e => { setInput(e.target.value); setError(""); }}
                      onKeyDown={e => e.key === "Enter" && submit()}
                      disabled={checking}
                      autoComplete="off" autoCorrect="off" spellCheck="false"
                    />
                    <button className="send" onClick={submit} disabled={checking || !input.trim()}>
                      {checking ? "…" : "Tentar"}
                    </button>
                  </div>

                  {error && <div className="err">{error}</div>}

                  <div className="meta">
                    <span>{guesses.length} <strong>tentativa{guesses.length !== 1 ? "s" : ""}</strong></span>
                    {best > 0 && <span>melhor <strong>{best}</strong></span>}
                  </div>

                  {/* Hints panel */}
                  <div className="hints-panel">
                    <div className="hint-meta">Dicas disponíveis</div>

                    {/* Category hint */}
                    <div className="hint-row">
                      <button className="hint-trigger" onClick={() => setShowCatHint(h => !h)}>
                        {showCatHint ? "Ocultar" : "Categoria"}
                      </button>
                      {showCatHint && (
                        <span className="hint-val">
                          {secret.category}
                          {showFirstLetter && <> · primeira letra: <strong>{secret.word[0].toUpperCase()}</strong></>}
                          {showLetterCount && <> · <strong>{secret.word.length}</strong> letras</>}
                        </span>
                      )}
                    </div>

                    {/* Word hint */}
                    <div className="hint-row">
                      <button
                        className="hint-trigger"
                        onClick={revealWordHint}
                        disabled={loadingHint}
                      >
                        {loadingHint ? "…" : hintRevealed ? "Ocultar dica" : "Dica da palavra"}
                      </button>
                      {hintRevealed && wordHint && !loadingHint && (
                        <span className="hint-val" onClick={() => setHintRevealed(h => !h)} style={{ cursor: "pointer" }}>
                          {wordHint}
                        </span>
                      )}
                    </div>
                  </div>

                  {guesses.length > 0 && (
                    <div className="bar">
                      <div className="bar-fill" style={{ width: `${best / 10}%` }} />
                    </div>
                  )}
                </>
              )}

              {/* Guesses list */}
              {guesses.length > 0 && (
                <>
                  <div className="col-heads">
                    <div /><div>Palavra</div>
                    <div style={{ textAlign: "right" }}>Score</div>
                    <div style={{ textAlign: "right" }}>Temperatura</div>
                  </div>

                  {checking && (
                    <div className="checking">
                      <div className="dots"><span /><span /><span /></div>
                      <span>analisando</span>
                    </div>
                  )}

                  {guesses.map((g, i) => (
                    <div className="row" key={g.word}>
                      <div className="r-num">{i + 1}</div>
                      <div className="r-word">{g.word}</div>
                      <div className="r-score" style={{ color: scoreColor(g.score) }}>{g.score}</div>
                      <div className="r-lbl">{scoreLabel(g.score)}</div>
                    </div>
                  ))}
                </>
              )}

              {guesses.length === 0 && !checking && phase === "playing" && (
                <div className="empty">Faça sua primeira tentativa</div>
              )}
            </>
          )}
        </div>
      </div>
    </>
  );
}
