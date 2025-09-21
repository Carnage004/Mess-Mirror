import React, { useEffect, useState } from "react";

export default function Dashboard({ onViewMenu }) {
  const [messes, setMesses] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    setLoading(true);
    fetch("/api/messes")
      .then(res => res.json())
      .then(data => {
        setMesses(data);
        setLoading(false);
      });
  }, []);

  if (loading) {
    return (
      <section className="bg-white rounded-2xl shadow p-6">
        <div>Loading...</div>
      </section>
    );
  }

  return (
    <section className="bg-white rounded-2xl shadow p-6" aria-label="Dashboard">
      <div className="flex items-center justify-between">
        <div>
          <h3 className="text-lg font-semibold">Today's Messes</h3>
          <p className="text-sm text-gray-500">Select a mess to view today's menu</p>
        </div>
        {/* Optionally show today's date */}
        <div className="text-sm text-gray-600">{new Date().toLocaleDateString()}</div>
      </div>
      <div className="mt-4 grid grid-cols-1 md:grid-cols-2 gap-4">
        {messes.map(mess => (
          <div key={mess.id} className="border rounded-lg p-4 flex items-start gap-4">
            <div className="w-14 h-14 rounded-lg bg-amber-100 flex items-center justify-center text-2xl" aria-hidden>
              {mess.emoji || "🏫"}
            </div>
            <div className="flex-1">
              <div className="flex items-center justify-between">
                <div>
                  <h4 className="font-medium">{mess.name}</h4>
                  <p className="text-xs text-gray-500">Highlight: {mess.highlight}</p>
                </div>
                <button
                  className="text-sm bg-emerald-50 px-3 py-1 rounded-full"
                  onClick={() => onViewMenu(mess)}
                >
                  View Menu
                </button>
              </div>
              <div className="mt-3 text-xs text-gray-600">
                Avg rating: ⭐ {mess.avgRating} • Cost/day ₹{mess.cost}
              </div>
            </div>
          </div>
        ))}
      </div>
    </section>
  );
}# Mess-Mirror
