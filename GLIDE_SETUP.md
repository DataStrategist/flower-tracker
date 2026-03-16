# Flower Tracker - Glide App Setup

## Step 1: Set Up Google Sheets

1. Go to [sheets.google.com](https://sheets.google.com) and create a new spreadsheet
2. Name it **Flower Tracker**
3. Create two sheets (tabs at the bottom):

### Sheet 1: `flowers`
Paste these headers in row 1:
```
id | name | photo | notes
```
Then import `flowers.csv` (File > Import > Upload, choose "Insert into current sheet")

### Sheet 2: `sightings`
Paste these headers in row 1:
```
flower_id | flower_name | date | location | notes
```
(Leave it empty - the app will fill it)

---

## Step 2: Create Glide App

1. Go to [glideapps.com](https://glideapps.com) → **New App** → **From Google Sheets**
2. Connect to your **Flower Tracker** spreadsheet
3. Name the app "Flower Tracker"

---

## Step 3: Build the Flower Cards Screen

1. In the Glide builder, click the default screen on the left
2. Set the **data source** to the `flowers` sheet
3. Change layout to **Card Collection**
4. Set:
   - **Title** → `name` column
   - **Image** → `photo` column
5. This is your home screen - a visual grid of all flowers

---

## Step 4: Build the Detail Screen (with the Log Button)

When you tap a flower card, it opens a detail screen. Configure it:

1. Click **Detail Screen** in the left panel
2. Add a **Text** component → set to `name` (big title)
3. Add an **Image** component → set to `photo`
4. Add a **Button** component:
   - Label: `✓ Seen today`
   - Action: **Add Row**
   - Table: `sightings`
   - Set columns:
     - `flower_id` → Current Row → `id`
     - `flower_name` → Current Row → `name`
     - `date` → Special Value → **Date/Time**
     - `location` → (leave blank or add a text input field above the button)

---

## Step 5: Show the Count on Each Card (optional but nice)

1. In Google Sheets, on the `flowers` sheet, add a column: `count`
2. In row 2, paste this formula (adjust if needed):
   ```
   =COUNTIF(sightings!B:B, B2)
   ```
3. Drag it down for all rows
4. Back in Glide, add `count` as a **Badge** or subtitle on the card

---

## Step 6: Adding New Flowers

Glide auto-generates an Add screen. To configure it:

1. Click the **+** button screen in the left panel
2. Keep: `name` (text input), `photo` (image picker)
3. Remove `id` from the form - add a formula in Sheets instead:
   ```
   =ROW()-1
   ```
   in the `id` column for every row

---

## Step 7: Install on Android

1. On your Android phone, open Chrome
2. Go to your Glide app's share URL
3. Tap the menu → **Add to Home Screen**
4. It installs like a native app

---

## Data for Plotting Later

Your `sightings` sheet will grow into a table like:
```
flower_id | flower_name | date       | location
1         | Bluebell    | 2026-04-12 | Hill meadow
1         | Bluebell    | 2026-04-15 | Garden
3         | Primrose    | 2026-03-20 | Woodland edge
```

From this you can plot:
- **Week of year** that each species appears (seasonality curve)
- **First sighting** per year per species (phenology tracking)
- **Species diversity** on any given walk

A Python plotting script is easy to add later - just pull from the Sheet with `gspread`.

---

## Flower List

Pre-loaded in `flowers.csv` - edit freely:
- Bluebell, Snowdrop, Primrose, Daffodil, Wood Anemone
- Lesser Celandine, Cowslip, Red Campion, Ox-eye Daisy, Foxglove

Add any others you expect to see.
