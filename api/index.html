const fetch = require("node-fetch");

const EBAY_APP_ID = process.env.EBAY_APP_ID;
const EBAY_CERT_ID = process.env.EBAY_CERT_ID;

let cachedToken = null;
let tokenExpiry = 0;

async function getToken() {
  if (cachedToken && Date.now() < tokenExpiry) return cachedToken;
  const credentials = Buffer.from(`${EBAY_APP_ID}:${EBAY_CERT_ID}`).toString("base64");
  const res = await fetch("https://api.ebay.com/identity/v1/oauth2/token", {
    method: "POST",
    headers: {
      "Authorization": `Basic ${credentials}`,
      "Content-Type": "application/x-www-form-urlencoded",
    },
    body: "grant_type=client_credentials&scope=https%3A%2F%2Fapi.ebay.com%2Foauth%2Fapi_scope",
  });
  const data = await res.json();
  if (!data.access_token) throw new Error("Token error: " + JSON.stringify(data));
  cachedToken = data.access_token;
  tokenExpiry = Date.now() + (data.expires_in - 60) * 1000;
  return cachedToken;
}

async function searchSoldItems(query, limit = 10) {
  const token = await getToken();
  const url = `https://svcs.ebay.com/services/search/FindingService/v1` +
    `?OPERATION-NAME=findCompletedItems` +
    `&SERVICE-VERSION=1.0.0` +
    `&SECURITY-APPNAME=${EBAY_APP_ID}` +
    `&GLOBAL-ID=EBAY-US` +
    `&RESPONSE-DATA-FORMAT=JSON` +
    `&REST-PAYLOAD` +
    `&keywords=${encodeURIComponent(query)}` +
    `&categoryId=212` +
    `&itemFilter(0).name=SoldItemsOnly&itemFilter(0).value=true` +
    `&sortOrder=EndTimeSoonest` +
    `&paginationInput.entriesPerPage=${limit}`;

  const res = await fetch(url);
  const data = await res.json();
  const items = data?.findCompletedItemsResponse?.[0]?.searchResult?.[0]?.item || [];
  return items.map(item => ({
    title: item.title?.[0] || "",
    price: parseFloat(item.sellingStatus?.[0]?.currentPrice?.[0]?.__value__ || 0),
    endTime: item.listingInfo?.[0]?.endTime?.[0] || "",
    url: item.viewItemURL?.[0] || "",
    condition: item.condition?.[0]?.conditionDisplayName?.[0] || "Unknown",
    image: item.galleryURL?.[0] || "",
  }));
}

async function getTrending(sport) {
  const queries = {
    NBA: ["Cooper Flagg rookie card refractor","Victor Wembanyama rookie card chrome","Shai Gilgeous-Alexander card 2025","Dylan Harper rookie card","LeBron James card 2025"],
    NFL: ["Patrick Mahomes card 2025 chrome","Cam Ward rookie card 2025","Jaxson Dart rookie card 2025","Caleb Williams rookie card chrome","Saquon Barkley card 2025"],
    MLB: ["Paul Skenes rookie card chrome","Jackson Holliday rookie card","Shohei Ohtani card 2025","Juan Soto card 2025","Elly De La Cruz rookie card"],
    Pokemon: ["Charizard ex 151 PSA","Pikachu ex special art rare","Umbreon VMAX alt art PSA","Charizard ex obsidian flames SAR","Gardevoir ex special art rare PSA"],
  };

  const sportQueries = queries[sport] || queries.NBA;
  const results = await Promise.all(
    sportQueries.map(q => searchSoldItems(q, 5).catch(() => []))
  );

  return sportQueries.map((query, i) => {
    const sales = results[i];
    const psa10Sales = sales.filter(s => s.title.toLowerCase().includes("psa 10"));
    const rawSales = sales.filter(s => !s.title.toLowerCase().includes("psa") && !s.title.toLowerCase().includes("bgs"));
    const psa10Avg = psa10Sales.length ? Math.round(psa10Sales.reduce((a,b) => a+b.price, 0) / psa10Sales.length) : 0;
    const rawAvg = rawSales.length ? Math.round(rawSales.reduce((a,b) => a+b.price, 0) / rawSales.length) : 0;
    return { query, totalSales: sales.length, psa10Avg, rawAvg, recentSales: sales.slice(0,5), sport };
  });
}

module.exports = async (req, res) => {
  res.setHeader("Access-Control-Allow-Origin", "*");
  res.setHeader("Access-Control-Allow-Methods", "GET, OPTIONS");
  if (req.method === "OPTIONS") return res.status(200).end();

  try {
    const { action, query, sport } = req.query;
    if (action === "trending") {
      const data = await getTrending(sport || "NBA");
      return res.json({ success: true, data });
    }
    if (action === "search" && query) {
      const data = await searchSoldItems(query, 20);
      return res.json({ success: true, data });
    }
    res.json({ success: false, error: "Unknown action" });
  } catch (e) {
    res.status(500).json({ success: false, error: e.message });
  }
};
