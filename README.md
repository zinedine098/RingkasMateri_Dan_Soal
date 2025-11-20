# AI Chat App (OpenRouter API) - No Manual API Key Input

Aplikasi ini adalah client-side AI Chat yang menggunakan layanan OpenRouter untuk memanggil model LLM seperti "z-ai/glm-4.5-air". API Key sudah ditanam otomatis ke localStorage sehingga pengguna tidak perlu memasukkan API Key secara manual.


Fitur Utama
- Tidak memerlukan input API key dari pengguna
- Berbasis HTML, CSS, dan JavaScript (tanpa backend)
- Mendukung model OpenRouter (default: z-ai/glm-4.5-air)
- Pengaturan otomatis disimpan di localStorage
- Simple dan cepat digunakan

Instalasi

Clone repository:

    git clonehttps://github.com/zinedine098/RingkasMateri_Dan_Soal/

Masuk ke folder proyek:

    cd RingkasMateri_Dan_Soal

Tidak diperlukan instalasi tambahan.

Menjalankan Aplikasi

Buka file index.html secara langsung:

    (double click) index.html

atau menggunakan Live Server di VSCode:

    code .
    (klik "Open With Live Server")

Cara Kerja API Key

Project ini otomatis menyimpan API key ke localStorage menggunakan script:

    localStorage.setItem("ag_settings", {
        apiKey: "API_KEY_TERTANAM",
        temperature: 0.7,
        maxTokens: 1024,
        reasoningEnabled: false
    });

Pengguna tidak perlu:
- login
- input API key
- konfigurasi apapun

Semua berjalan otomatis di background.

Cara Menggunakan Aplikasi
1. Buka aplikasi di browser.
2. Ketik pertanyaan atau prompt.
3. Klik "Send" atau tekan Enter.
4. Sistem akan menghubungi model LLM melalui OpenRouter.
5. Hasil akan tampil pada layar chat.


Fungsi Utama: callAI()
Berikut fungsi utama untuk melakukan request ke OpenRouter:

    async function callAI(prompt) {
        const settings = JSON.parse(localStorage.getItem("ag_settings") || "{}");
        const API_KEY = settings.apiKey;
        const model = "z-ai/glm-4.5-air";

        const body = {
            model: model,
            messages: [
                { role: "system", content: "You are a helpful teacher." },
                { role: "user", content: prompt }
            ],
            reasoning: { enabled: settings.reasoningEnabled ?? false },
            temperature: settings.temperature ?? 0.7,
            max_tokens: settings.maxTokens ?? 1024
        };

        const res = await fetch("https://openrouter.ai/api/v1/chat/completions", {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
                "Authorization": `Bearer ${API_KEY}`
            },
            body: JSON.stringify(body)
        });

        if (!res.ok) throw new Error(await res.text());
        return (await res.json()).choices[0].message.content;
    }


Catatan Keamanan
- Jika repository Anda PUBLIC, semua orang dapat melihat API key.
- Sangat disarankan menjadikan repository PRIVATE atau gunakan backend proxy.
- Jangan menaruh API key pribadi di repositori publik.

Lisensi
Proyek ini bebas dimodifikasi dan digunakan sesuai kebutuhan Anda.
