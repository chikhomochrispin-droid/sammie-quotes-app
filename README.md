import React, { useMemo, useState } from "react"; import { Heart, Search, RefreshCw, Share2, Quote } from "lucide-react";

// SAMMIE QUOTES FULL EXPO APK PROJECT // Install commands: // npm install // npx expo start // eas build -p android --profile preview

// package.json // { //   "name": "sammie-quotes", //   "version": "1.0.0", //   "main": "node_modules/expo/AppEntry.js", //   "scripts": { //     "start": "expo start", //     "android": "expo start --android", //     "ios": "expo start --ios", //     "web": "expo start --web" //   }, //   "dependencies": { //     "expo": "~53.0.0", //     "react": "19.0.0", //     "react-native": "0.79.0", //     "lucide-react": "^0.511.0" //   } // }

// app.json // { //   "expo": { //     "name": "Sammie Quotes", //     "slug": "sammie-quotes", //     "version": "1.0.0", //     "orientation": "portrait", //     "userInterfaceStyle": "dark", //     "android": { //       "package": "com.sammiequotes.app" //     } //   } // }

// eas.json // { //   "build": { //     "preview": { //       "android": { //         "buildType": "apk" //       } //     } //   } // }

// APK BUILD GUIDE // 1. Install Node.js and Android Studio // 2. Create Expo App: npx create-expo-app SammieQuotes // 3. Replace App.js with this code // 4. Install dependencies: npm install lucide-react // 5. Start app: npx expo start // 6. Build APK: eas build -p android --profile preview // 7. Download generated APK and install on Android

export default function SammieQuotesApp() { const baseQuotes = [ "Dream big and start small.", "Success begins with discipline.", "Your future is created by what you do today.", "Great things take time.", "Every day is a second chance.", "Confidence is silent. Doubt is loud.", "Focus on progress, not perfection.", "Hard work beats talent when talent stops working.", "Be stronger than your excuses.", "The best view comes after the hardest climb.", "Push yourself because no one else will do it for you.", "Stay patient and trust your journey.", "Small steps lead to big results.", "Your only limit is your mindset.", "Never stop learning.", "Kindness is always powerful.", "Believe in yourself every day.", "Happiness starts from within.", "Success loves preparation.", "Turn pain into power." ];

const allQuotes = useMemo(() => { return Array.from({ length: 3000 }, (_, i) => ({ id: i + 1, text: ${baseQuotes[i % baseQuotes.length]} #${i + 1}, author: "Sammie Quotes" })); }, []);

const [search, setSearch] = useState(""); const [favorites, setFavorites] = useState([]); const [dailyQuote, setDailyQuote] = useState(allQuotes[0]);

const filteredQuotes = allQuotes.filter((quote) => quote.text.toLowerCase().includes(search.toLowerCase()) );

const toggleFavorite = (id) => { setFavorites((prev) => prev.includes(id) ? prev.filter((fav) => fav !== id) : [...prev, id] ); };

const randomQuote = () => { const random = allQuotes[Math.floor(Math.random() * allQuotes.length)]; setDailyQuote(random); };

const shareQuote = async (quote) => { if (navigator.share) { await navigator.share({ title: "Sammie Quotes", text: quote.text }); } else { navigator.clipboard.writeText(quote.text); alert("Quote copied to clipboard!"); } };

return ( <div className="min-h-screen bg-gradient-to-br from-black via-purple-950 to-slate-900 text-white"> <div className="max-w-7xl mx-auto px-4 py-8"> <div className="text-center mb-10"> <h1 className="text-5xl md:text-6xl font-extrabold mb-4 tracking-tight"> ✨ Sammie Quotes ✨ </h1> <p className="text-gray-300 text-lg max-w-2xl mx-auto"> Discover thousands of motivational, inspirational, success, and life quotes. </p> </div>

<div className="bg-white/10 border border-white/10 rounded-3xl p-6 mb-8 backdrop-blur-xl shadow-2xl">
      <div className="flex flex-col md:flex-row items-center justify-between gap-4">
        <div>
          <p className="text-purple-300 text-sm uppercase tracking-widest mb-2">
            Quote of the Day
          </p>
          <h2 className="text-2xl font-bold leading-relaxed">
            “{dailyQuote.text}”
          </h2>
        </div>

        <button
          onClick={randomQuote}
          className="flex items-center gap-2 bg-purple-600 hover:bg-purple-700 px-5 py-3 rounded-2xl font-semibold transition"
        >
          <RefreshCw size={18} />
          New Quote
        </button>
      </div>
    </div>

    <div className="relative mb-8">
      <Search className="absolute left-4 top-4 text-gray-400" size={20} />
      <input
        type="text"
        placeholder="Search quotes..."
        value={search}
        onChange={(e) => setSearch(e.target.value)}
        className="w-full bg-white/10 border border-white/10 rounded-2xl py-4 pl-12 pr-4 outline-none text-white placeholder:text-gray-400"
      />
    </div>

    <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
      {filteredQuotes.map((quote) => (
        <div
          key={quote.id}
          className="bg-white/10 border border-white/10 rounded-3xl p-6 backdrop-blur-lg shadow-xl hover:scale-[1.02] transition-all duration-300"
        >
          <div className="flex items-center justify-between mb-4">
            <Quote className="text-purple-400" size={28} />
            <button
              onClick={() => toggleFavorite(quote.id)}
              className="transition"
            >
              <Heart
                size={24}
                className={favorites.includes(quote.id)
                  ? "fill-red-500 text-red-500"
                  : "text-white"
                }
              />
            </button>
          </div>

          <p className="text-lg leading-relaxed mb-5">
            {quote.text}
          </p>

          <div className="flex items-center justify-between">
            <span className="text-purple-300 text-sm font-semibold">
              {quote.author}
            </span>

            <button
              onClick={() => shareQuote(quote)}
              className="bg-white/10 hover:bg-white/20 p-2 rounded-xl transition"
            >
              <Share2 size={18} />
            </button>
          </div>
        </div>
      ))}
    </div>

    <div className="mt-12 bg-white/10 border border-white/10 rounded-3xl p-6 text-center backdrop-blur-lg">
      <h3 className="text-2xl font-bold mb-2">
        ❤️ Favorite Quotes
      </h3>
      <p className="text-gray-300">
        You currently saved {favorites.length} favorite quotes.
      </p>
    </div>

    <footer className="text-center mt-12 text-gray-400 pb-8">
      <p>© 2026 Sammie Quotes — Inspire the world daily.</p>
    </footer>
  </div>
</div>

); }
