# Patroparadax.com
import { useState } from "react";

export default function Home() {
  const [prompt, setPrompt] = useState("");
  const [images, setImages] = useState<string[]>([]);
  const [loading, setLoading] = useState(false);

  async function handleGenerate() {
    if (!prompt.trim()) return;
    setLoading(true);
    const res = await fetch("/api/generate", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ prompt }),
    });
    const data = await res.json();
    setImages(data.images);
    setLoading(false);
  }

  return (
    <main className="min-h-screen flex flex-col items-center p-6 gap-6 bg-gray-100">
      <h1 className="text-4xl font-bold">AI Image Generator</h1>

      <div className="flex w-full max-w-2xl gap-2">
        <input
          value={prompt}
          onChange={(e) => setPrompt(e.target.value)}
          placeholder="Describe your dream image..."
          className="flex-1 p-3 rounded-lg border"
        />
        <button
          onClick={handleGenerate}
          className="px-4 py-3 rounded-lg bg-black text-white disabled:opacity-50"
          disabled={loading}
        >
          {loading ? "Creating…" : "Generate"}
        </button>
      </div>

      {images.length > 0 && (
        <section className="grid md:grid-cols-3 gap-4 w-full max-w-5xl">
          {images.map((src) => (
            <img key={src} src={src} alt="AI result" className="rounded-lg shadow" />
          ))}
        </section>
      )}
    </main>
  );
}
