import React, { useMemo, useState } from "react";
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Badge } from "@/components/ui/badge";
import { Progress } from "@/components/ui/progress";
import { Shuffle, RotateCcw, Plus, Trash2, Search, ArrowRight, BookOpenCheck } from "lucide-react";

/**
 * Simpele flashcard-app om AI-begrippen te leren + quizmodus.
 * - Start met 5 voorbeeldwoorden
 * - Flashcards: omdraaien (3D flip), volgende/vorige, shuffle, reset, zoeken
 * - Zelf kaarten toevoegen/verwijderen (lokaal in deze sessie)
 * - Quiz: 5 meerkeuzevragen (MC) op basis van de kaarten
 */

type Flashcard = {
  term: string;
  definition: string;
  example?: string;
  tags?: string[];
};

type IndexedCard = Flashcard & { _i: number };

type QuizQuestion = {
  id: string;
  prompt: string;
  options: string[];
  correctIndex: number;
  explanation: string;
};

const seedCards: Flashcard[] = [
  {
    term: "Generatieve AI",
    definition:
      "AI die zelf nieuwe inhoud kan maken, zoals teksten, afbeeldingen, audio of code.",
    example:
      "Waarom belangrijk? Leerkrachten werken niet langer alleen met ‘zoekmachines’, maar (co-)auteurs.",
  },
  {
    term: "Prompt",
    definition:
      "De instructie of vraag die je aan een AI-tool geeft.",
    example:
      "Waarom belangrijk? De kwaliteit van de output hangt sterk af van hoe duidelijk en gericht je prompt is.",
  },
  {
    term: "Prompt engineering (basisniveau)",
    definition:
      "Het bewust formuleren en verfijnen van prompts om betere resultaten te krijgen.",
    example:
      "Waarom belangrijk? Geen technische vaardigheid, maar wel een nieuwe didactische én professionele skill.",
  },
  {
    term: "Hallucinatie",
    definition:
      "Wanneer AI overtuigend klinkende, maar foutieve of verzonnen informatie geeft.",
    example:
      "Waarom belangrijk? AI is geen betrouwbare bron op zich; kritisch checken blijft essentieel.",
  },
  {
    term: "Bias (vooringenomenheid)",
    definition:
      "Onbedoelde vertekeningen in AI-output door eenzijdige of onvolledige trainingsdata.",
    example:
      "Waarom belangrijk? AI kan bestaande ongelijkheden versterken als je er niet kritisch mee omgaat.",
  },
  {
    term: "Mens-in-de-lus (human in the loop)",
    definition:
      "De mens blijft verantwoordelijk voor controle, interpretatie en beslissingen.",
    example:
      "Waarom belangrijk? AI ondersteunt, maar vervangt het professioneel oordeel van de leerkracht niet.",
  },
  {
    term: "Datagebruik & privacy",
    definition:
      "Wat gebeurt er met de data en input die je aan een AI-tool geeft?",
    example:
      "Waarom belangrijk? Leerkrachten moeten bewust omgaan met persoonsgegevens van cursisten.",
  },
  {
    term: "AI als leerondersteuner",
    definition:
      "AI inzetten om leren te ondersteunen, zoals feedback, oefenvragen en differentiatie.",
    example:
      "Waarom belangrijk? AI is vooral krachtig als didactische assistent, niet als vervanger van leren.",
  },
  {
    term: "Transparantie",
    definition:
      "Duidelijk zijn over wanneer en hoe AI gebruikt wordt in onderwijscontexten.",
    example:
      "Waarom belangrijk? Helpt vertrouwen opbouwen bij cursisten én collega’s.",
  },
  {
    term: "Kritische AI-geletterdheid",
    definition:
      "Het vermogen om AI te gebruiken, te begrijpen én te bevragen.",
    example:
      "Waarom belangrijk? Niet ‘kunnen klikken’, maar bewust en verantwoord inzetten staat centraal.",
  },
];

function clamp(n: number, min: number, max: number) {
  return Math.max(min, Math.min(max, n));
}

function normalize(s: string) {
  return s.trim().toLowerCase();
}

function parseTags(input: string) {
  return input
    .split(",")
    .map((t) => t.trim())
    .filter(Boolean);
}

function shuffleArray<T>(arr: T[]) {
  const copy = [...arr];
  for (let i = copy.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [copy[i], copy[j]] = [copy[j], copy[i]];
  }
  return copy;
}

function makeQuiz(available: Flashcard[], count = 5): QuizQuestion[] {
  // MC-quiz: we tonen een definitie en je kiest de juiste term.
  // We nemen max 5 willekeurige kaarten (of minder als er minder kaarten zijn).
  const pool = shuffleArray(available);
  const selected = pool.slice(0, Math.min(count, pool.length));

  return selected.map((card, idx) => {
    const distractors = shuffleArray(
      available
        .filter((c) => c.term !== card.term)
        .map((c) => c.term)
    ).slice(0, 3);

    const options = shuffleArray([card.term, ...distractors]);
    const correctIndex = options.findIndex((o) => o === card.term);

    return {
      id: `${card.term}-${idx}`,
      prompt: `Welke term past bij deze definitie?\n\n“${card.definition}”`,
      options,
      correctIndex,
      explanation: card.example ? `Voorbeeld: ${card.example}` : "",
    };
  });
}

export default function AIVocabFlashcards() {
  const [mode, setMode] = useState<"study" | "quiz">("study");

  // Study state
  const [cards, setCards] = useState<Flashcard[]>(seedCards);
  const [index, setIndex] = useState(0);
  const [flipped, setFlipped] = useState(false);
  const [known, setKnown] = useState<Record<number, boolean>>({});
  const [search, setSearch] = useState("");

  const [newTerm, setNewTerm] = useState("");
  const [newDef, setNewDef] = useState("");
  const [newEx, setNewEx] = useState("");
  const [newTags, setNewTags] = useState("");

  // Quiz state
  const [quiz, setQuiz] = useState<QuizQuestion[]>([]);
  const [qIndex, setQIndex] = useState(0);
  const [answers, setAnswers] = useState<Record<string, number>>({});
  const [showResults, setShowResults] = useState(false);

  const filtered: IndexedCard[] = useMemo(() => {
    const q = normalize(search);
    const withIndex = cards.map((c, i) => ({ ...c, _i: i }));
    if (!q) return withIndex;
    return withIndex.filter((c) => {
      const hay = [c.term, c.definition, c.example || "", (c.tags || []).join(" ")]
        .join(" ")
        .toLowerCase();
      return hay.includes(q);
    });
  }, [cards, search]);

  const active: IndexedCard | undefined = filtered[index] || filtered[0];
  const activeGlobalIndex = active?._i ?? 0;

  const progress = useMemo(() => {
    const total = filtered.length || 1;
    const k = filtered.reduce((acc, c) => acc + (known[c._i] ? 1 : 0), 0);
    return { total, known: k, pct: Math.round((k / total) * 100) };
  }, [filtered, known]);

  const quizScore = useMemo(() => {
    if (!quiz.length) return { correct: 0, total: 0 };
    let correct = 0;
    for (const q of quiz) {
      const a = answers[q.id];
      if (typeof a === "number" && a === q.correctIndex) correct++;
    }
    return { correct, total: quiz.length };
  }, [quiz, answers]);

  function go(delta: number) {
    setFlipped(false);
    setIndex((i) => clamp(i + delta, 0, Math.max(0, filtered.length - 1)));
  }

  function toggleKnown(val: boolean) {
    setKnown((k) => ({ ...k, [activeGlobalIndex]: val }));
  }

  function shuffleCards() {
    // reset known for simplicity (indices change)
    setCards((prev) => shuffleArray(prev));
    setKnown({});
    setIndex(0);
    setFlipped(false);
  }

  function resetAll() {
    setMode("study");

    setCards(seedCards);
    setKnown({});
    setIndex(0);
    setFlipped(false);
    setSearch("");

    setNewTerm("");
    setNewDef("");
    setNewEx("");
    setNewTags("");

    setQuiz([]);
    setQIndex(0);
    setAnswers({});
    setShowResults(false);
  }

  function addCard() {
    if (!normalize(newTerm) || !normalize(newDef)) return;
    const tags = parseTags(newTags);
    setCards((c) => [
      ...c,
      {
        term: newTerm.trim(),
        definition: newDef.trim(),
        example: newEx.trim() || undefined,
        tags,
      },
    ]);
    setNewTerm("");
    setNewDef("");
    setNewEx("");
    setNewTags("");
  }

  function deleteActive() {
    if (!cards.length || !active) return;
    const gi = activeGlobalIndex;

    setCards((c) => c.filter((_, i) => i !== gi));
    setKnown((k) => {
      const next: Record<number, boolean> = {};
      Object.entries(k).forEach(([key, val]) => {
        const i = Number(key);
        if (i === gi) return;
        next[i > gi ? i - 1 : i] = val;
      });
      return next;
    });

    setIndex((i) => clamp(i, 0, Math.max(0, filtered.length - 2)));
    setFlipped(false);
  }

  function startQuiz() {
    // Quiz op basis van ALLE kaarten (niet alleen gefilterde), zodat zoeken niet per ongeluk je quiz verkleint.
    const q = makeQuiz(cards, 5);
    setQuiz(q);
    setQIndex(0);
    setAnswers({});
    setShowResults(false);
    setMode("quiz");
  }

  function answerQuestion(qid: string, optionIndex: number) {
    setAnswers((a) => ({ ...a, [qid]: optionIndex }));
  }

  function nextQuestion() {
    if (qIndex >= quiz.length - 1) {
      setShowResults(true);
      return;
    }
    setQIndex((i) => i + 1);
  }

  function prevQuestion() {
    setQIndex((i) => clamp(i - 1, 0, Math.max(0, quiz.length - 1)));
  }

  function backToStudy() {
    setMode("study");
  }

  return (
    <div className="min-h-screen w-full bg-gradient-to-br from-purple-50 via-white to-purple-100 p-4 md:p-8">
      <div className="mx-auto max-w-3xl space-y-6">
        <div className="flex flex-col gap-2 md:flex-row md:items-end md:justify-between">
          <div>
            <h1 className="text-2xl md:text-3xl font-semibold tracking-tight text-purple-900">
              AI-begrippen — Flashcards
            </h1>
            <p className="text-sm text-muted-foreground">
              {mode === "study"
                ? "Klik op de kaart om om te draaien. Markeer wat je al kent."
                : "Quizmodus: beantwoord 5 meerkeuzevragen."}
            </p>
          </div>

          <div className="flex flex-wrap items-center gap-2">
            {mode === "study" ? (
              <>
                <Button
                  variant="secondary"
                  onClick={startQuiz}
                  className="gap-2 bg-purple-700 text-white hover:bg-purple-800"
                >
                  <BookOpenCheck className="h-4 w-4" /> Ga naar quiz
                </Button>
                <Button
                  variant="secondary"
                  onClick={shuffleCards}
                  className="gap-2 bg-purple-100 text-purple-900 hover:bg-purple-200"
                >
                  <Shuffle className="h-4 w-4" /> Shuffle
                </Button>
              </>
            ) : (
              <Button
                variant="outline"
                onClick={backToStudy}
                className="gap-2 border-purple-300 text-purple-900 hover:bg-purple-50"
              >
                <ArrowRight className="h-4 w-4 rotate-180" /> Terug naar kaarten
              </Button>
            )}

            <Button
              variant="outline"
              onClick={resetAll}
              className="gap-2 border-purple-300 text-purple-900 hover:bg-purple-50"
            >
              <RotateCcw className="h-4 w-4" /> Reset
            </Button>
          </div>
        </div>

        {mode === "study" ? (
          <>
            <Card className="rounded-2xl shadow-md border-purple-200 bg-white/80 backdrop-blur">
              <CardHeader className="space-y-3">
                <div className="flex flex-col gap-3 md:flex-row md:items-center md:justify-between">
                  <CardTitle className="text-lg text-purple-950">
                    {filtered.length ? (
                      <span>
                        Kaart {index + 1} / {filtered.length}
                      </span>
                    ) : (
                      <span>Geen resultaten</span>
                    )}
                  </CardTitle>

                  <div className="flex items-center gap-2">
                    <Badge
                      variant={known[activeGlobalIndex] ? "default" : "secondary"}
                      className={
                        known[activeGlobalIndex]
                          ? "bg-purple-700 hover:bg-purple-700"
                          : "bg-purple-100 text-purple-900"
                      }
                    >
                      {known[activeGlobalIndex] ? "Gekend" : "Nog oefenen"}
                    </Badge>
                    <Badge variant="outline" className="border-purple-200 text-purple-900">
                      {progress.known}/{progress.total} ({progress.pct}%)
                    </Badge>
                  </div>
                </div>

                <div className="space-y-2">
                  <Progress value={progress.pct} className="[&>div]:bg-purple-700" />
                  <div className="flex items-center gap-2">
                    <Search className="h-4 w-4 text-muted-foreground" />
                    <Input
                      placeholder="Zoek (term, definitie, tag…)"
                      value={search}
                      onChange={(e) => {
                        setSearch(e.target.value);
                        setIndex(0);
                        setFlipped(false);
                      }}
                      className="border-purple-200 focus-visible:ring-purple-300"
                    />
                  </div>
                </div>
              </CardHeader>

              <CardContent className="space-y-4">
                {filtered.length ? (
                  <button
                    type="button"
                    onClick={() => setFlipped((f) => !f)}
                    className="w-full text-left"
                    aria-label="Draai kaart om"
                  >
                    <div className="[perspective:1200px]">
                      <div
                        className={
                          "relative min-h-[220px] rounded-2xl transition-transform duration-500 [transform-style:preserve-3d] " +
                          (flipped ? "[transform:rotateY(180deg)]" : "[transform:rotateY(0deg)]")
                        }
                      >
                        {/* FRONT */}
                        <div className="absolute inset-0 rounded-2xl border border-purple-200 bg-white p-5 md:p-6 shadow-sm hover:shadow-md transition-shadow [backface-visibility:hidden]">
                          <div className="text-xs uppercase tracking-wide text-purple-800/80">Term</div>
                          <div className="mt-2 text-2xl md:text-3xl font-semibold text-purple-950">
                            {active?.term}
                          </div>
                          <div className="mt-4 text-xs text-muted-foreground">Tip: klik om om te draaien.</div>
                        </div>

                        {/* BACK */}
                        <div className="absolute inset-0 rounded-2xl border border-purple-200 bg-white p-5 md:p-6 shadow-sm hover:shadow-md transition-shadow [transform:rotateY(180deg)] [backface-visibility:hidden]">
                          <div className="text-xs uppercase tracking-wide text-purple-800/80">
                            Definitie + voorbeeld
                          </div>
                          <div className="mt-2 space-y-3">
                            <div className="text-base md:text-lg leading-relaxed text-purple-950">
                              {active?.definition}
                            </div>
                            {active?.example ? (
                              <div className="rounded-xl bg-purple-50 border border-purple-100 p-3 text-sm text-purple-950">
                                <span className="font-medium">Voorbeeld:</span> {active.example}
                              </div>
                            ) : null}
                            {active?.tags?.length ? (
                              <div className="flex flex-wrap gap-2">
                                {active.tags.map((t) => (
                                  <Badge
                                    key={t}
                                    variant="secondary"
                                    className="bg-purple-100 text-purple-900"
                                  >
                                    {t}
                                  </Badge>
                                ))}
                              </div>
                            ) : null}
                            <div className="mt-2 text-xs text-muted-foreground">Tip: klik om terug te draaien.</div>
                          </div>
                        </div>
                      </div>
                    </div>
                  </button>
                ) : (
                  <div className="rounded-2xl border border-purple-200 bg-white p-6 text-sm text-muted-foreground">
                    Geen kaarten die matchen met je zoekopdracht.
                  </div>
                )}

                <div className="grid grid-cols-2 md:grid-cols-4 gap-2">
                  <Button
                    variant="outline"
                    onClick={() => go(-1)}
                    disabled={index <= 0 || filtered.length === 0}
                    className="border-purple-300 text-purple-900 hover:bg-purple-50"
                  >
                    Vorige
                  </Button>
                  <Button
                    variant="outline"
                    onClick={() => go(1)}
                    disabled={index >= filtered.length - 1 || filtered.length === 0}
                    className="border-purple-300 text-purple-900 hover:bg-purple-50"
                  >
                    Volgende
                  </Button>
                  <Button
                    onClick={() => toggleKnown(true)}
                    disabled={filtered.length === 0}
                    className="bg-purple-700 hover:bg-purple-800"
                  >
                    Gekend ✅
                  </Button>
                  <Button
                    variant="secondary"
                    onClick={() => toggleKnown(false)}
                    disabled={filtered.length === 0}
                    className="bg-purple-100 text-purple-900 hover:bg-purple-200"
                  >
                    Oefenen ↩︎
                  </Button>
                </div>

                <div className="flex items-center justify-between gap-2 pt-2">
                  <div className="text-xs text-muted-foreground">
                    Weergave: <span className="font-medium">{flipped ? "definition" : "term"}</span>
                  </div>
                  <Button
                    variant="destructive"
                    onClick={deleteActive}
                    disabled={!cards.length || filtered.length === 0}
                    className="gap-2"
                  >
                    <Trash2 className="h-4 w-4" /> Verwijder kaart
                  </Button>
                </div>
              </CardContent>
            </Card>

            <Card className="rounded-2xl shadow-md border-purple-200 bg-white/80 backdrop-blur">
              <CardHeader>
                <CardTitle className="text-lg text-purple-950">Voeg een kaart toe</CardTitle>
              </CardHeader>
              <CardContent className="space-y-3">
                <div className="grid md:grid-cols-2 gap-3">
                  <div className="space-y-2">
                    <label className="text-sm font-medium text-purple-950">Term *</label>
                    <Input
                      value={newTerm}
                      onChange={(e) => setNewTerm(e.target.value)}
                      placeholder="bv. Temperature"
                      className="border-purple-200 focus-visible:ring-purple-300"
                    />
                  </div>
                  <div className="space-y-2">
                    <label className="text-sm font-medium text-purple-950">Tags (komma’s)</label>
                    <Input
                      value={newTags}
                      onChange={(e) => setNewTags(e.target.value)}
                      placeholder="bv. basis, risico"
                      className="border-purple-200 focus-visible:ring-purple-300"
                    />
                  </div>
                </div>

                <div className="space-y-2">
                  <label className="text-sm font-medium text-purple-950">Definitie *</label>
                  <Input
                    value={newDef}
                    onChange={(e) => setNewDef(e.target.value)}
                    placeholder="Leg het begrip kort en helder uit…"
                    className="border-purple-200 focus-visible:ring-purple-300"
                  />
                </div>

                <div className="space-y-2">
                  <label className="text-sm font-medium text-purple-950">Voorbeeld (optioneel)</label>
                  <Input
                    value={newEx}
                    onChange={(e) => setNewEx(e.target.value)}
                    placeholder="Geef een mini-voorbeeld in jouw context…"
                    className="border-purple-200 focus-visible:ring-purple-300"
                  />
                </div>

                <div className="flex flex-col md:flex-row gap-2 md:items-center md:justify-between">
                  <div className="text-xs text-muted-foreground">* Verplichte velden</div>
                  <Button
                    onClick={addCard}
                    className="gap-2 bg-purple-700 hover:bg-purple-800"
                    disabled={!normalize(newTerm) || !normalize(newDef)}
                  >
                    <Plus className="h-4 w-4" /> Toevoegen
                  </Button>
                </div>
              </CardContent>
            </Card>

            <div className="text-xs text-muted-foreground">
              Ideeën om uit te breiden: spaced repetition, import/export (CSV), niveaus per thema (prompting, ethiek, data).
            </div>
          </>
        ) : (
          <Card className="rounded-2xl shadow-md border-purple-200 bg-white/80 backdrop-blur">
            <CardHeader className="space-y-2">
              <div className="flex items-center justify-between gap-2">
                <CardTitle className="text-lg text-purple-950">
                  Quiz — vraag {Math.min(qIndex + 1, quiz.length)} / {quiz.length || 0}
                </CardTitle>
                <Badge variant="outline" className="border-purple-200 text-purple-900">
                  Score: {quizScore.correct}/{quizScore.total}
                </Badge>
              </div>
              <p className="text-sm text-muted-foreground">
                Kies het juiste antwoord. Je krijgt 5 vragen.
              </p>
            </CardHeader>

            <CardContent className="space-y-4">
              {!quiz.length ? (
                <div className="rounded-2xl border border-purple-200 bg-white p-6 text-sm text-muted-foreground">
                  Geen quizvragen beschikbaar.
                </div>
              ) : showResults ? (
                <div className="space-y-4">
                  <div className="rounded-2xl border border-purple-200 bg-white p-5 md:p-6">
                    <div className="text-sm text-muted-foreground">Resultaat</div>
                    <div className="mt-1 text-2xl font-semibold text-purple-950">
                      {quizScore.correct} / {quizScore.total} correct
                    </div>
                    <div className="mt-3 text-sm text-muted-foreground">
                      Tip: ga terug naar de kaarten om de begrippen waar je op struikelde nog eens te oefenen.
                    </div>
                  </div>

                  <div className="grid gap-3">
                    {quiz.map((q) => {
                      const a = answers[q.id];
                      const isCorrect = typeof a === "number" && a === q.correctIndex;
                      return (
                        <div key={q.id} className="rounded-2xl border border-purple-200 bg-white p-4">
                          <div className="flex items-start justify-between gap-2">
                            <div className="whitespace-pre-line text-sm text-purple-950">{q.prompt}</div>
                            <Badge
                              className={
                                isCorrect
                                  ? "bg-purple-700 text-white"
                                  : "bg-purple-100 text-purple-900"
                              }
                            >
                              {isCorrect ? "Correct" : "Oefenen"}
                            </Badge>
                          </div>
                          <div className="mt-3 text-sm">
                            <span className="font-medium">Juiste antwoord:</span> {q.options[q.correctIndex]}
                          </div>
                          {q.explanation ? (
                            <div className="mt-2 text-sm text-muted-foreground">{q.explanation}</div>
                          ) : null}
                        </div>
                      );
                    })}
                  </div>

                  <div className="flex flex-wrap gap-2">
                    <Button
                      onClick={() => {
                        const q = makeQuiz(cards, 5);
                        setQuiz(q);
                        setQIndex(0);
                        setAnswers({});
                        setShowResults(false);
                      }}
                      className="bg-purple-700 hover:bg-purple-800"
                    >
                      Nieuwe quiz
                    </Button>
                    <Button
                      variant="outline"
                      onClick={backToStudy}
                      className="border-purple-300 text-purple-900 hover:bg-purple-50"
                    >
                      Terug naar kaarten
                    </Button>
                  </div>
                </div>
              ) : (
                (() => {
                  const q = quiz[qIndex];
                  const selected = answers[q.id];
                  return (
                    <div className="space-y-4">
                      <div className="rounded-2xl border border-purple-200 bg-white p-5 md:p-6">
                        <div className="whitespace-pre-line text-purple-950">{q.prompt}</div>
                      </div>

                      <div className="grid gap-2">
                        {q.options.map((opt, i) => {
                          const active = selected === i;
                          return (
                            <button
                              key={opt + i}
                              type="button"
                              onClick={() => answerQuestion(q.id, i)}
                              className={
                                "w-full rounded-2xl border p-4 text-left transition " +
                                (active
                                  ? "border-purple-400 bg-purple-50"
                                  : "border-purple-200 bg-white hover:bg-purple-50")
                              }
                            >
                              <div className="flex items-center justify-between gap-2">
                                <div className="text-sm text-purple-950">{opt}</div>
                                {active ? (
                                  <Badge className="bg-purple-700 text-white">Geselecteerd</Badge>
                                ) : null}
                              </div>
                            </button>
                          );
                        })}
                      </div>

                      <div className="flex flex-wrap items-center justify-between gap-2">
                        <div className="flex gap-2">
                          <Button
                            variant="outline"
                            onClick={prevQuestion}
                            disabled={qIndex === 0}
                            className="border-purple-300 text-purple-900 hover:bg-purple-50"
                          >
                            Vorige
                          </Button>
                          <Button
                            variant="outline"
                            onClick={nextQuestion}
                            className="border-purple-300 text-purple-900 hover:bg-purple-50"
                          >
                            {qIndex === quiz.length - 1 ? "Toon resultaat" : "Volgende"}
                          </Button>
                        </div>

                        <Button
                          onClick={() => {
                            setShowResults(true);
                          }}
                          disabled={Object.keys(answers).length === 0}
                          className="bg-purple-700 hover:bg-purple-800"
                        >
                          Resultaat
                        </Button>
                      </div>
                    </div>
                  );
                })()
              )}
            </CardContent>
          </Card>
        )}
      </div>
    </div>
  );
}
