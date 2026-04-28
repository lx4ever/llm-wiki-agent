# market_intel.py

- Notion page id: `34b7a51c-1360-80c1-88d0-fe3a6c47c129`

```javascript
import os
import re
import json
import time
import hashlib
from typing import Any, Dict, List, Optional, Tuple

import requests
import feedparser
from dateutil import parser as date_parser


# ============================================================
# Environment variables
# ============================================================

NOTION_TOKEN = os.getenv("NOTION_TOKEN")
NOTION_MARKET_DB_ID = os.getenv("NOTION_MARKET_DB_ID")
NOTION_OPPORTUNITIES_DB_ID = os.getenv("NOTION_OPPORTUNITIES_DB_ID")
NOTION_PROGRAM_MAP_DB_ID = os.getenv("NOTION_PROGRAM_MAP_DB_ID")

if not NOTION_TOKEN:
    raise RuntimeError("Missing NOTION_TOKEN environment variable.")
if not NOTION_MARKET_DB_ID:
    raise RuntimeError("Missing NOTION_MARKET_DB_ID environment variable.")
if not NOTION_OPPORTUNITIES_DB_ID:
    raise RuntimeError("Missing NOTION_OPPORTUNITIES_DB_ID environment variable.")
if not NOTION_PROGRAM_MAP_DB_ID:
    raise RuntimeError("Missing NOTION_PROGRAM_MAP_DB_ID environment variable.")


# ============================================================
# Config
# ============================================================

STATE_FILE = "market_intel_state.json"
NOTION_VERSION = "2022-06-28"
REQUEST_TIMEOUT = 30
MIN_RELEVANCE_SCORE_TO_SAVE = 5
MIN_OPPORTUNITY_SCORE_TO_CREATE = 8
HIGH_PRIORITY_OPPORTUNITY_SCORE = 11

NOTION_HEADERS = {
    "Authorization": f"Bearer {NOTION_TOKEN}",
    "Notion-Version": NOTION_VERSION,
    "Content-Type": "application/json",
}

RSS_FEEDS = [
    ("Defense News", "https://www.defensenews.com/arc/outboundfeeds/rss/"),
    ("Breaking Defense", "https://breakingdefense.com/feed/"),
    ("Defense Industry Europe", "https://defence-industry.eu/feed/"),
    ("Naval News", "https://www.navalnews.com/feed/"),
    ("Naval Today", "https://www.navaltoday.com/feed/"),
    ("Navy Lookout", "https://www.navylookout.com/feed/"),
    ("Canadian Defence Review", "https://www.canadiandefencereview.com/feed/"),
    ("Government of Canada News", "https://www.canada.ca/en/news/web-feeds/all.atom.xml"),
    ("SpaceNews", "https://spacenews.com/feed/"),
    ("NASA News", "https://www.nasa.gov/news-release/feed/"),
    ("FlightGlobal", "https://www.flightglobal.com/rss"),
    ("Aviation Week", "https://aviationweek.com/rss.xml"),
    ("GovCon Wire", "https://www.govconwire.com/feed/"),
    ("Canada Defence Search", "https://news.google.com/rss/search?q=Canada+defence+contract"),
    ("US DoD Contracts", "https://news.google.com/rss/search?q=US+DoD+contract+award"),
    ("Satellite Contracts", "https://news.google.com/rss/search?q=satellite+contract+award"),
]

TARGET_COMPANIES = [
    "MDA",
    "L3Harris",
    "General Dynamics",
    "General Dynamics Mission Systems",
    "GDMS",
    "PAL Aerospace",
    "Lockheed Martin",
    "Boeing",
    "Northrop Grumman",
    "Raytheon",
    "RTX",
    "Textron",
    "CAE",
    "De Havilland",
    "NASA",
    "Canadian Space Agency",
    "CSA",
    "Airbus",
    "KF Aerospace",
    "IMP Aerospace",
    "Irving Shipbuilding",
    "Seaspan",
    "SkyAlyne",
    "TKMS",
    "Hanwha Ocean",
    "Kongsberg",
]

POSITIVE_KEYWORDS = {
    "contract": 3,
    "awarded": 3,
    "award": 3,
    "procurement": 3,
    "partnership": 2,
    "teaming agreement": 3,
    "supplier": 2,
    "subcontract": 2,
    "program": 2,
    "production": 2,
    "integration": 2,
    "avionics": 2,
    "electronics": 2,
    "electrical": 2,
    "wiring": 2,
    "wire harness": 3,
    "harness": 2,
    "cable": 2,
    "assembly": 2,
    "satellite": 2,
    "payload": 2,
    "mission systems": 2,
    "modernization": 2,
    "sustainment": 2,
    "in-service support": 2,
    "training": 1,
    "isr": 2,
    "c4isr": 2,
    "naval": 2,
    "aircraft": 2,
    "space": 2,
    "defence": 2,
    "defense": 2,
}

NEGATIVE_KEYWORDS = {
    "podcast": -2,
    "opinion": -2,
    "lifestyle": -3,
    "travel": -3,
    "passenger route": -2,
    "earnings call": -1,
    "sports": -3,
    "entertainment": -3,
}

REGION_KEYWORDS = {
    "Canada": ["canada", "canadian", "dnd", "caf", "pspc", "itb", "ottawa", "rcn", "rcaf", "coast guard"],
    "US": ["united states", "u.s.", "dod", "pentagon", "u.s. navy", "u.s. army", "usaf", "space force", "nasa"],
}

PROGRAM_RULES = [
    {
        "program": "River-class Destroyer",
        "match": ["river-class destroyer", "canadian surface combatant", "type 26", "irving shipbuilding"],
        "prime": "Irving Shipbuilding / Lockheed Martin Canada",
        "domain": "Naval",
        "country": "Canada",
        "customer": "Royal Canadian Navy / Government of Canada",
        "major_team": "Irving Shipbuilding, Lockheed Martin Canada, BAE Systems ecosystem",
        "stage": "Active",
        "kanata_fit": "Electrical assemblies, harnessing, subsystem integration support, electronics subassemblies, test support",
        "likely_work_packages": "Electrical assemblies, harnesses, cabinet/internal electronics subassemblies, test/interface support",
        "why_now": "Program remains active with design, engineering, procurement, and implementation-phase work creating supplier positioning opportunities.",
        "reference_url": "https://www.canada.ca/en/department-national-defence/services/procurement/canadian-surface-combatant.html",
    },
    {
        "program": "Canadian Multi-Mission Aircraft",
        "match": ["p-8a", "poseidon", "canadian multi-mission aircraft", "cmma"],
        "prime": "Boeing",
        "domain": "Air",
        "country": "Canada",
        "customer": "Royal Canadian Air Force / Government of Canada",
        "major_team": "Boeing, CAE, GE Aerospace Canada, IMP Aerospace & Defence, KF Aerospace, Honeywell Aerospace Canada, Raytheon Canada, StandardAero",
        "stage": "Active",
        "kanata_fit": "Aircraft electrical assemblies, harness support, integration kits, test cabling, subassemblies",
        "likely_work_packages": "Electrical subassemblies, aircraft wiring harnesses, retrofit/integration kits, test cabling",
        "why_now": "Program execution, training, and in-service support preparation can create supplier entry points.",
        "reference_url": "https://boeing.mediaroom.com/2023-11-30-Canada-Selects-Boeings-P-8A-Poseidon-as-its-Multi-Mission-Aircraft",
    },
    {
        "program": "CC-330 Husky",
        "match": ["cc-330", "husky", "strategic tanker transport capability", "a330"],
        "prime": "Airbus / L3Harris MAS",
        "domain": "Air",
        "country": "Canada",
        "customer": "Royal Canadian Air Force / Government of Canada",
        "major_team": "Airbus Defence and Space, L3Harris MAS",
        "stage": "Active",
        "kanata_fit": "Sustainment electrical assemblies, repair kits, support equipment wiring, retrofit assemblies",
        "likely_work_packages": "Repair kits, sustainment assemblies, support equipment wiring, retrofit electrical subassemblies",
        "why_now": "Current activity is strongest in sustainment and long-term support implementation.",
        "reference_url": "https://www.canada.ca/en/department-national-defence/services/procurement/strategic-tanker-transport-capability-project.html",
    },
    {
        "program": "FAcT",
        "match": ["future aircrew training", "fact", "skyalyne"],
        "prime": "SkyAlyne",
        "domain": "Training",
        "country": "Canada",
        "customer": "Royal Canadian Air Force / Government of Canada",
        "major_team": "SkyAlyne, CAE, KF Aerospace",
        "stage": "Active",
        "kanata_fit": "Training-system support hardware, avionics/electrical subassemblies, support equipment",
        "likely_work_packages": "Training hardware, electrical subassemblies, support equipment, simulation-related integration hardware",
        "why_now": "Execution-stage downstream supplier opportunities can emerge through training, site, and support contracts.",
        "reference_url": "https://skyalyne.ca/",
    },
    {
        "program": "Canadian Patrol Submarine Project",
        "match": ["canadian patrol submarine", "cpsp", "tkms", "hanwha"],
        "prime": "TBD finalist / TKMS / Hanwha Ocean",
        "domain": "Naval",
        "country": "Canada",
        "customer": "Royal Canadian Navy / Government of Canada",
        "major_team": "TKMS, Hanwha Ocean, future Canadian sustainment ecosystem",
        "stage": "Pursuit",
        "kanata_fit": "Sustainment, training systems, support equipment, electronics packaging, shore-side support systems",
        "likely_work_packages": "Support equipment, sustainment assemblies, electronics packaging, training/shoreside electrical systems",
        "why_now": "This is still a positioning-stage opportunity; best value is early supplier mapping and ecosystem monitoring.",
        "reference_url": "https://www.canada.ca/en/defence-investment-agency.html",
    },
    {
        "program": "Mid-Shore Multi-Mission Vessels",
        "match": ["mid-shore multi-mission", "msmm", "multi-mission vessel", "kongsberg vanguard"],
        "prime": "Kongsberg Vanguard LP",
        "domain": "Naval",
        "country": "Canada",
        "customer": "Canadian Coast Guard / Government of Canada",
        "major_team": "Kongsberg Defence & Aerospace, Salt Ship Design, Adaptive Marine Solutions",
        "stage": "Active",
        "kanata_fit": "Marine electrical assemblies, panels, wiring packages, subsystem support",
        "likely_work_packages": "Marine electrical assemblies, panel wiring, subsystem support packages",
        "why_now": "Design-stage activity often precedes downstream subsystem and supplier engagement.",
        "reference_url": "https://www.canada.ca/en/public-services-procurement/news.html",
    },
    {
        "program": "Joint Support Ship",
        "match": ["joint support ship", "jss", "seaspan"],
        "prime": "Seaspan Vancouver Shipyards",
        "domain": "Naval",
        "country": "Canada",
        "customer": "Royal Canadian Navy / Government of Canada",
        "major_team": "Seaspan ecosystem and major marine systems suppliers",
        "stage": "Active",
        "kanata_fit": "Marine electrical systems, support assemblies, harnessing, equipment subassemblies",
        "likely_work_packages": "Marine electrical support systems, harnessing, equipment-level subassemblies, test/interface hardware",
        "why_now": "Later-stage integration and sustainment positioning may be stronger than greenfield design opportunities.",
        "reference_url": "https://www.canada.ca/en/department-national-defence/services/procurement/joint-support-ship.html",
    },
]


# ============================================================
# Helpers
# ============================================================

def load_state() -> Dict[str, Any]:
    if os.path.exists(STATE_FILE):
        with open(STATE_FILE, "r", encoding="utf-8") as f:
            return json.load(f)
    return {
        "seen_urls": [],
        "saved_market_urls": [],
        "saved_opportunity_keys": [],
        "program_page_ids": {}
    }


def save_state(state: Dict[str, Any]) -> None:
    with open(STATE_FILE, "w", encoding="utf-8") as f:
        json.dump(state, f, indent=2)


def stable_hash(text: str) -> str:
    return hashlib.sha256(text.encode("utf-8")).hexdigest()


def clean_text(text: str) -> str:
    text = text or ""
    text = re.sub(r"<[^>]+>", " ", text)
    text = re.sub(r"\s+", " ", text).strip()
    return text


def parse_date(entry: Dict[str, Any]) -> Optional[str]:
    candidates = [entry.get("published"), entry.get("updated"), entry.get("created")]
    for candidate in candidates:
        if candidate:
            try:
                return date_parser.parse(candidate).date().isoformat()
            except Exception:
                continue
    return None


def extract_full_text(entry: Dict[str, Any]) -> str:
    parts: List[str] = []
    parts.append(clean_text(entry.get("title", "")))
    parts.append(clean_text(entry.get("summary", "")))
    parts.append(clean_text(entry.get("description", "")))
    if entry.get("content"):
        for c in entry["content"]:
            parts.append(clean_text(c.get("value", "")))
    return " ".join([p for p in parts if p]).strip()


def detect_companies(text: str) -> List[str]:
    lower = text.lower()
    found = []
    for company in TARGET_COMPANIES:
        if company.lower() in lower:
            found.append(company)
    return sorted(set(found))


def detect_regions(text: str) -> List[str]:
    lower = text.lower()
    found = []
    for region, terms in REGION_KEYWORDS.items():
        if any(term in lower for term in terms):
            found.append(region)
    if not found:
        found.append("Global")
    return found


def detect_category(text: str) -> str:
    lower = text.lower()
    if any(k in lower for k in ["policy", "budget", "itb", "regulation", "procurement reform", "export control"]):
        return "Policy"
    if any(k in lower for k in ["partnership", "teaming agreement", "memorandum of understanding", "mou", "joint venture"]):
        return "Partnerships"
    if any(k in lower for k in ["satellite", "space", "payload", "launch", "orbital", "csa", "nasa"]):
        return "Space"
    if any(k in lower for k in ["aircraft", "avionics", "airframe", "flight", "aerospace"]):
        return "Aerospace"
    return "Defence"


def score_relevance(text: str) -> int:
    lower = text.lower()
    score = 0

    for kw, val in POSITIVE_KEYWORDS.items():
        if kw in lower:
            score += val

    for kw, penalty in NEGATIVE_KEYWORDS.items():
        if kw in lower:
            score += penalty

    for _, words in REGION_KEYWORDS.items():
        if any(word in lower for word in words):
            score += 2

    for company in TARGET_COMPANIES:
        if company.lower() in lower:
            score += 2

    if "contract" in lower and any(c.lower() in lower for c in TARGET_COMPANIES):
        score += 3

    if "canada" in lower and "contract" in lower:
        score += 3

    if any(k in lower for k in ["electrical", "wiring", "harness", "wire harness", "cable", "assembly"]):
        score += 3

    return score


def score_opportunity(text: str, category: str, companies: List[str], regions: List[str]) -> int:
    lower = text.lower()
    score = 0

    if any(k in lower for k in ["contract", "awarded", "award", "procurement", "design contract"]):
        score += 4

    if any(k in lower for k in ["partnership", "teaming agreement", "mou", "joint venture"]):
        score += 3

    if any(k in lower for k in [
        "electrical", "electronics", "wiring", "wire harness", "harness",
        "cable", "assembly", "avionics", "integration", "mission systems",
        "power distribution", "control systems", "subsystems"
    ]):
        score += 4

    if "canada" in lower or "canadian" in lower:
        score += 2
    if "US" in regions:
        score += 1

    focus_companies = [
        "MDA", "L3Harris", "General Dynamics", "GDMS", "Boeing",
        "Lockheed Martin", "PAL Aerospace", "CAE", "KF Aerospace",
        "Airbus", "Irving Shipbuilding", "Seaspan", "Kongsberg", "SkyAlyne"
    ]
    for company in focus_companies:
        if company.lower() in lower:
            score += 2

    if any(k in lower for k in [
        "full-rate production", "implementation", "in-service support",
        "long-term support", "training", "qualification", "construction",
        "retrofit", "modernization", "sustainment"
    ]):
        score += 3

    if category in ["Defence", "Aerospace", "Space", "Partnerships"]:
        score += 1

    return score


def summarize_article(title: str, text: str, companies: List[str], category: str, regions: List[str]) -> Tuple[str, str]:
    sentences = re.split(r"(?<=[.!?])\s+", text)
    selected = []

    for s in sentences:
        s_clean = s.strip()
        if len(s_clean) < 40:
            continue
        if any(k in s_clean.lower() for k in [
            "contract", "award", "awarded", "program", "partnership",
            "supplier", "procurement", "modernization", "sustainment"
        ]):
            selected.append(s_clean)
        if len(selected) >= 2:
            break

    if not selected:
        selected = [s.strip() for s in sentences[:2] if s.strip()]

    summary = " ".join(selected)[:1800].strip()
    companies_text = ", ".join(companies) if companies else "the companies involved"
    regions_text = ", ".join(regions)

    if category == "Partnerships":
        insight = (
            f"This signals active teaming or supply-chain alignment involving {companies_text}. "
            f"Review whether Kanata can position electrical assemblies, harnessing, or integration support "
            f"into the emerging work scope in {regions_text}."
        )
    elif category == "Policy":
        insight = (
            f"This may affect procurement access, domestic content expectations, or supplier positioning in {regions_text}. "
            f"Check whether it changes ITB, localization, qualification, or bidding requirements relevant to Kanata."
        )
    elif category == "Space":
        insight = (
            f"This may indicate demand for high-reliability electrical assemblies, cable/harness work, or subsystem support "
            f"for space-related programs involving {companies_text}."
        )
    elif category == "Aerospace":
        insight = (
            f"This may create opportunities in aircraft modification, avionics integration, retrofit kits, or electrical subassemblies "
            f"for programs involving {companies_text}."
        )
    else:
        insight = (
            f"This may indicate defence procurement or integration activity where Kanata could support "
            f"{companies_text} with electrical assemblies, harnesses, or complex build-to-print manufacturing."
        )

    return summary, insight


def detect_program(text: str) -> Optional[Dict[str, str]]:
    lower = text.lower()
    for rule in PROGRAM_RULES:
        if any(match_term in lower for match_term in rule["match"]):
            return rule
    return None


def infer_opportunity_type(text: str, category: str) -> str:
    lower = text.lower()
    if any(k in lower for k in ["in-service support", "sustainment", "maintenance", "long-term support"]):
        return "Sustainment"
    if any(k in lower for k in ["retrofit", "modernization", "upgrade"]):
        return "Retrofit"
    if any(k in lower for k in ["design contract", "development", "new program", "construction"]):
        return "New program"
    if category == "Partnerships":
        return "Subcontract"
    return "Build-to-print"


def unique_opportunity_key(program_name: str, url: str) -> str:
    return stable_hash(f"{program_name}|{url}")


def truncate(text: str, max_len: int = 1900) -> str:
    return (text or "")[:max_len]


# ============================================================
# Notion helpers
# ============================================================

def notion_create_page(database_id: str, properties: Dict[str, Any]) -> Dict[str, Any]:
    url = "https://api.notion.com/v1/pages"
    payload = {"parent": {"database_id": database_id}, "properties": properties}
    response = requests.post(url, headers=NOTION_HEADERS, json=payload, timeout=REQUEST_TIMEOUT)
    response.raise_for_status()
    return response.json()


def notion_query_database(database_id: str, filter_payload: Dict[str, Any]) -> Dict[str, Any]:
    url = f"https://api.notion.com/v1/databases/{database_id}/query"
    response = requests.post(url, headers=NOTION_HEADERS, json={"filter": filter_payload}, timeout=REQUEST_TIMEOUT)
    response.raise_for_status()
    return response.json()


def notion_build_title(content: str) -> Dict[str, Any]:
    return {"title": [{"text": {"content": truncate(content, 2000)}}]}


def notion_build_rich_text(content: str) -> Dict[str, Any]:
    return {"rich_text": [{"text": {"content": truncate(content, 2000)}}]}


def notion_build_select(name: str) -> Dict[str, Any]:
    return {"select": {"name": truncate(name, 100)}}


def notion_build_multi_select(values: List[str]) -> Dict[str, Any]:
    return {"multi_select": [{"name": truncate(v, 100)} for v in values[:25]]}


def notion_build_number(value: int) -> Dict[str, Any]:
    return {"number": value}


def notion_build_date(date_str: Optional[str]) -> Dict[str, Any]:
    return {"date": {"start": date_str}} if date_str else {"date": None}


def notion_build_url(url: str) -> Dict[str, Any]:
    return {"url": url}


def notion_build_checkbox(value: bool) -> Dict[str, Any]:
    return {"checkbox": value}


def notion_build_relation(page_ids: List[str]) -> Dict[str, Any]:
    return {"relation": [{"id": pid} for pid in page_ids if pid]}


def find_program_page_id_by_name(program_name: str) -> Optional[str]:
    filter_payload = {
        "property": "Program Name",
        "title": {
            "equals": program_name
        }
    }
    data = notion_query_database(NOTION_PROGRAM_MAP_DB_ID, filter_payload)
    results = data.get("results", [])
    if results:
        return results[0]["id"]
    return None


def ensure_program_map_entry(program_rule: Dict[str, str], program_page_ids_cache: Dict[str, str]) -> str:
    program_name = program_rule["program"]

    if program_name in program_page_ids_cache:
        return program_page_ids_cache[program_name]

    existing_id = find_program_page_id_by_name(program_name)
    if existing_id:
        program_page_ids_cache[program_name] = existing_id
        return existing_id

    properties = {
        "Program Name": notion_build_title(program_name),
        "Domain": notion_build_select(program_rule["domain"]),
        "Country": notion_build_select(program_rule["country"]),
        "Customer": notion_build_rich_text(program_rule["customer"]),
        "Prime": notion_build_rich_text(program_rule["prime"]),
        "Major Team / Key Suppliers": notion_build_rich_text(program_rule["major_team"]),
        "Program Stage": notion_build_select(program_rule["stage"]),
        "Kanata Fit": notion_build_rich_text(program_rule["kanata_fit"]),
        "Likely Work Packages": notion_build_rich_text(program_rule["likely_work_packages"]),
        "Why Now": notion_build_rich_text(program_rule["why_now"]),
        "Reference URL": notion_build_url(program_rule["reference_url"]),
    }

    created = notion_create_page(NOTION_PROGRAM_MAP_DB_ID, properties)
    page_id = created["id"]
    program_page_ids_cache[program_name] = page_id
    return page_id


def notion_create_market_item(article: Dict[str, Any], program_page_id: Optional[str] = None) -> None:
    properties = {
        "Title": notion_build_title(article["title"]),
        "Published Date": notion_build_date(article["published_date"]),
        "Source": notion_build_select(article["source"]),
        "Region": notion_build_multi_select(article["regions"]),
        "Category": notion_build_select(article["category"]),
        "Companies": notion_build_multi_select(article["companies"]),
        "Relevance Score": notion_build_number(article["relevance_score"]),
        "Summary": notion_build_rich_text(article["summary"]),
        "BD Insight": notion_build_rich_text(article["bd_insight"]),
        "URL": notion_build_url(article["url"]),
        "Status": notion_build_select("New"),
        "Follow Up?": notion_build_checkbox(article["relevance_score"] >= 7),
    }

    # Optional: only works if your Market Intelligence DB has a relation property named "Program"
    if program_page_id:
        properties["Program"] = notion_build_relation([program_page_id])

    notion_create_page(NOTION_MARKET_DB_ID, properties)


def notion_create_opportunity_item(opp: Dict[str, Any], program_page_id: str) -> None:
    properties = {
        "Opportunity Name": notion_build_title(opp["opportunity_name"]),
        "Program Name": notion_build_select(opp["program_name"]),
        "Account / Prime": notion_build_select(opp["account_prime"]),
        "Why Relevant": notion_build_rich_text(opp["why_relevant"]),
        "Opportunity Type": notion_build_select(opp["opportunity_type"]),
        "Score": notion_build_number(opp["score"]),
        "Status": notion_build_select(opp["status"]),
        "Next Action": notion_build_rich_text(opp["next_action"]),
        "Suggested Contact": notion_build_rich_text(opp["suggested_contact"]),
        "URL": notion_build_url(opp["url"]),
        "Program": notion_build_relation([program_page_id]),
    }
    notion_create_page(NOTION_OPPORTUNITIES_DB_ID, properties)


# ============================================================
# Feed processing
# ============================================================

def process_feed(source_name: str, feed_url: str, seen_urls: set) -> List[Dict[str, Any]]:
    parsed = feedparser.parse(feed_url)
    items: List[Dict[str, Any]] = []

    for entry in parsed.entries:
        url = entry.get("link")
        if not url or url in seen_urls:
            continue

        title = clean_text(entry.get("title", "Untitled"))
        full_text = extract_full_text(entry)
        if not full_text:
            continue

        relevance_score = score_relevance(full_text)
        if relevance_score < MIN_RELEVANCE_SCORE_TO_SAVE:
            continue

        published_date = parse_date(entry)
        companies = detect_companies(full_text)
        regions = detect_regions(full_text)
        category = detect_category(full_text)
        summary, bd_insight = summarize_article(title, full_text, companies, category, regions)

        items.append({
            "title": title,
            "source": source_name,
            "url": url,
            "published_date": published_date,
            "companies": companies,
            "regions": regions,
            "category": category,
            "relevance_score": relevance_score,
            "summary": summary,
            "bd_insight": bd_insight,
            "full_text": full_text,
        })

    return items


def build_opportunity_from_article(article: Dict[str, Any]) -> Optional[Dict[str, Any]]:
    combined_text = " ".join([
        article["title"],
        article["summary"],
        article["bd_insight"],
        article["full_text"],
    ])

    program = detect_program(combined_text)
    opportunity_score = score_opportunity(
        text=combined_text,
        category=article["category"],
        companies=article["companies"],
        regions=article["regions"],
    )

    if opportunity_score < MIN_OPPORTUNITY_SCORE_TO_CREATE:
        return None
    if not program:
        return None

    status = "High Priority" if opportunity_score >= HIGH_PRIORITY_OPPORTUNITY_SCORE else "New"
    opportunity_type = infer_opportunity_type(combined_text, article["category"])
    suggested_contact = (
        f"Start with supplier management, program management, or business development contacts at {program['prime']}."
    )
    next_action = (
        f"Review this article, map current supplier ecosystem for {program['program']}, "
        f"and identify a Kanata-fit entry point tied to {program['kanata_fit']}."
    )

    return {
        "opportunity_name": f"{program['program']} | {article['source']}",
        "program_name": program["program"],
        "account_prime": program["prime"],
        "why_relevant": f"{article['bd_insight']} Program fit: {program['kanata_fit']}.",
        "opportunity_type": opportunity_type,
        "score": opportunity_score,
        "status": status,
        "next_action": next_action,
        "suggested_contact": suggested_contact,
        "url": article["url"],
        "program_rule": program,
        "opportunity_key": unique_opportunity_key(program["program"], article["url"]),
    }


# ============================================================
# Main
# ============================================================

def main() -> None:
    print("Loading state...")
    state = load_state()
    seen_urls = set(state.get("seen_urls", []))
    saved_market_urls = set(state.get("saved_market_urls", []))
    saved_opportunity_keys = set(state.get("saved_opportunity_keys", []))
    program_page_ids_cache = dict(state.get("program_page_ids", {}))

    all_articles: List[Dict[str, Any]] = []

    print("Reading feeds...")
    for source_name, feed_url in RSS_FEEDS:
        try:
            print(f"  - {source_name}")
            items = process_feed(source_name, feed_url, seen_urls)
            all_articles.extend(items)
            time.sleep(0.5)
        except Exception as e:
            print(f"[WARN] Failed feed '{source_name}': {e}")

    all_articles.sort(
        key=lambda x: (
            -x["relevance_score"],
            x["published_date"] or "",
            x["source"],
        )
    )

    market_saved = 0
    opp_saved = 0
    program_saved_or_linked = 0

    for article in all_articles:
        try:
            opp = build_opportunity_from_article(article)
            program_page_id = None

            if opp:
                program_page_id = ensure_program_map_entry(opp["program_rule"], program_page_ids_cache)
                program_saved_or_linked += 1

            if article["url"] not in saved_market_urls:
                try:
                    notion_create_market_item(article, program_page_id=program_page_id)
                except requests.HTTPError as e:
                    # Fallback if Market DB does not have the Program relation yet
                    if e.response is not None and e.response.status_code == 400:
                        notion_create_market_item(article, program_page_id=None)
                    else:
                        raise
                saved_market_urls.add(article["url"])
                market_saved += 1
                print(f"[OK] Market item: {article['title']} (score={article['relevance_score']})")

            if opp and opp["opportunity_key"] not in saved_opportunity_keys:
                notion_create_opportunity_item(opp, program_page_id)
                saved_opportunity_keys.add(opp["opportunity_key"])
                opp_saved += 1
                print(f"[OK] Opportunity: {opp['opportunity_name']} (score={opp['score']})")

            seen_urls.add(article["url"])

        except requests.HTTPError as e:
            print(f"[WARN] Notion API error for '{article['title']}': {e}")
            try:
                print(e.response.text)
            except Exception:
                pass
        except Exception as e:
            print(f"[WARN] Failed article '{article['title']}': {e}")

    state["seen_urls"] = sorted(seen_urls)
    state["saved_market_urls"] = sorted(saved_market_urls)
    state["saved_opportunity_keys"] = sorted(saved_opportunity_keys)
    state["program_page_ids"] = program_page_ids_cache
    save_state(state)

    print()
    print("Done.")
    print(f"Saved market items: {market_saved}")
    print(f"Saved opportunities: {opp_saved}")
    print(f"Programs linked/ensured: {program_saved_or_linked}")


if __name__ == "__main__":
    main()
```
