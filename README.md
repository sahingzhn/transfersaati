# transfersaati
import React, { useEffect, useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import Parser from "rss-parser";

const parser = new Parser();

export default function TransferSaati() {
  const [haberler, setHaberler] = useState([]);

  useEffect(() => {
    const fetchRSS = async () => {
      const CORS_PROXY = "https://api.allorigins.win/raw?url="; // CORS sorunu için proxy
      const feedUrl = "https://www.fotomac.com.tr/rss/transfer"; // Transfer RSS kaynağı
      try {
        const feed = await parser.parseURL(CORS_PROXY + feedUrl);
        setHaberler(feed.items.slice(0, 10));
      } catch (error) {
        console.error("RSS verisi çekilemedi:", error);
      }
    };

    fetchRSS();
  }, []);

  return (
    <main className="p-6 max-w-4xl mx-auto">
      <h1 className="text-3xl font-bold mb-4 text-center">Transfer Saati</h1>
      <p className="text-center mb-8 text-gray-600">Futbol dünyasından en güncel transfer haberleri</p>

      <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
        {haberler.map((item, index) => (
          <Card key={index} className="hover:shadow-xl transition duration-300">
            <CardContent>
              <a href={item.link} target="_blank" rel="noopener noreferrer">
                <h2 className="text-xl font-semibold mb-2">{item.title}</h2>
                <p className="text-gray-700 text-sm">{item.contentSnippet}</p>
                <p className="text-xs text-gray-400 mt-2">{new Date(item.pubDate).toLocaleString()}</p>
              </a>
            </CardContent>
          </Card>
        ))}
      </div>
    </main>
  );
}
