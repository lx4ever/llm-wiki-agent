# App for AI Personal OS

- Notion page id: `eb07a51c-1360-83cc-9064-0194136024ac`

# 1) High-level architecture diagram (cloud-minimal)

Core principle: Photos never leave device. Cloud is only for billing + optional metadata sync.

# 2) “Millions of users” scaling strategy

## Why this scales cheaply

- The heavy compute (OCR, embeddings, clustering) runs on the user’s phone

- You’re not paying GPU inference cost per photo

- Your cloud only handles:

This is exactly how you avoid becoming a “cloud inference startup” that burns cash.

# 3) Detailed on-device pipeline (what happens when user installs)

### Stage 0 — Index

- Enumerate assets (asset_id, timestamp, location, type, size)

- Store into encrypted local DB

### Stage 1 — Fast wins (first 30–60 seconds)

- Duplicates: pHash (fast)

- Screenshots: OS metadata/heuristics

- Blur score: simple Laplacian variance / platform heuristics

### Stage 2 — OCR

- OCR only on:

### Stage 3 — Embeddings

- Compute embeddings for:

- Persist vectors in DB

### Stage 4 — Clustering

- Duplicate clusters: pHash + embedding verification

- Event clusters: time window + location + embedding proximity

- Theme clusters: embeddings + k-means/HDBSCAN

### Stage 5 — Rules Engine

- Generate Review Queue cards:

- Add exclusions (tax/legal/insurance)

# 4) Storage + indexing design (fast and safe)

## Local database tables (practical schema)

Assets

- asset_id (primary key)

- created_at

- location_lat/lon (nullable)

- media_type (photo/video)

- is_screenshot

- size_bytes

- burst_id (nullable)

Derived

- asset_id (FK)

- ocr_text (nullable)

- embedding (vector blob or quantized vector)

- phash (nullable)

- blur_score

- best_shot_score

- categories (array)

- confidence (float)

- why_explanation (string)

Clusters

- cluster_id

- cluster_type (duplicate/event/theme)

- members (list of asset_id)

- representative_asset_id

- confidence

Boards

- board_id

- board_type (context/vision)

- title

- member_asset_ids

- template_id (vision board only)

Rules

- rule_id

- trigger_type (age/category/quality)

- params (json)

- enabled (bool)

ReviewQueue

- queue_item_id

- rule_id

- asset_ids

- excluded_asset_ids

- created_at

- status (new/seen/done)

## Performance trick

- Store embeddings in a compact format (e.g., 256–512 dims, quantized)

- Lazy compute: do not embed everything on day 1; prioritize recency + likely value

# 5) UI/interaction architecture (how the AI explains itself)

To earn trust, every category/cleanup suggestion should show:

Why

- “Detected Interac e-Transfer keywords”

- “Screenshot + text contains ‘transfer’ + reference number”

- “Older than 180 days”

Confidence

- High / Medium / Low

User control

- “Correct this” → improves future suggestions (personalization)

This turns the app from “mysterious AI” into “assistant.”

# 6) Cloud components (keep it minimal)

If you choose to add cloud early, do only these:

## A) Subscriptions & entitlements

- Apple/Google in-app purchases

- Server verifies receipts (prevents fraud)

## B) Remote config

- Toggle features, thresholds (e.g., screenshot age 90 vs 180 days)

## C) Optional metadata sync (premium)

Sync across devices:

- boards (IDs + membership lists)

- rules (user settings)

- not the photos, not embeddings if you want to avoid privacy concerns

## D) Analytics (privacy-conscious)

Only log aggregate events:

- “cleanup_card_completed”

- “vision_board_exported”

- “recap_shared”

# 7) The simplest tech stack to ship fast

## iOS-first (recommended for “privacy + premium”)

- Swift + Photos framework

- Vision for OCR & faces/text

- Core ML for embeddings (converted model)

- SQLite + SQLCipher for encrypted DB

## Android-first (recommended for file ops + broad device base)

- Kotlin + MediaStore

- ML Kit for OCR & face detection

- TFLite for embeddings

- Room (SQLite) + SQLCipher

# 8) Concrete build plan aligned to this architecture

### Week 1–2: “Local-first pipeline”

- Media access + DB + indexer

- duplicates + screenshots + blur scoring

- Cleanup deck UI + Undo

### Week 3: “OCR + finance rules”

- OCR on screenshots

- finance classifier + rule card (>6 months)

### Week 4: “Embeddings + semantic search”

- embeddings for subset

- semantic search UI

### Week 5: “Boards”

- theme clustering → outfits/travel/food/ideas

- board UI

### Week 6: “Vision board builder”

- templates + auto-select + export

### Week 7: “Recap generator”

- highlight selection + local rendering

### Week 8: “Beta hardening”

- performance, battery, privacy text, onboarding

# Alternative perspectives (two architecture variants)

## Variant 1: “No embeddings in MVP”

If you want the fastest possible MVP:

- duplicates + screenshots + OCR categories + rules

- boards based on OCR + heuristics only

## Variant 2: “Embeddings computed on-demand”

Only compute embeddings when user:

- searches

- opens boards

- taps “create vision board”

# Action plan you can apply today

1. Decide platform: iOS-first or Android-first

1. Lock MVP outcomes:

1. Implement pipeline in this order:

1. Add embeddings + boards after you already have a “wow” cleanup moment
