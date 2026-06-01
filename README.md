# Shalom Education Centre Website
## Setup Guide

---

## STEP 1 — Add Your Supabase Credentials

Open `js/supabase.js` and replace these two lines at the top:

```js
const SUPABASE_URL = 'YOUR_SUPABASE_URL';
const SUPABASE_KEY = 'YOUR_SUPABASE_ANON_KEY';
```

Find these values at:
Supabase Dashboard → Your Project → Project Settings → API

---

## STEP 2 — Create Supabase Tables

Go to Supabase → Table Editor and create these tables:

### school_info
| Column       | Type        |
|--------------|-------------|
| id           | uuid (PK)   |
| school_name  | text        |
| tagline      | text        |
| address      | text        |
| phone        | text        |
| email        | text        |
| facebook_url | text        |
| youtube_url  | text        |
| line_id      | text        |
| office_hours | text        |
| logo_url     | text        |
| hero_url     | text        |

→ Insert ONE row with your school's info.

### articles
| Column        | Type        |
|---------------|-------------|
| id            | uuid (PK)   |
| title         | text        |
| excerpt       | text        |
| content       | text        |
| category      | text        |
| author        | text        |
| thumbnail_url | text        |
| published_at  | timestamptz |

### teachers
| Column        | Type    |
|---------------|---------|
| id            | uuid (PK) |
| name          | text    |
| position      | text    |
| subject       | text    |
| bio           | text    |
| photo_url     | text    |
| display_order | int     |

### gallery_photos
| Column     | Type        |
|------------|-------------|
| id         | uuid (PK)   |
| photo_url  | text        |
| caption    | text        |
| category   | text        |
| created_at | timestamptz |

### contact_messages
| Column     | Type        |
|------------|-------------|
| id         | uuid (PK)   |
| name       | text        |
| email      | text        |
| message    | text        |
| created_at | timestamptz |

---

## STEP 3 — Enable Public Read Access (Row Level Security)

For each table (school_info, articles, teachers, gallery_photos):
1. Go to Supabase → Authentication → Policies
2. Select the table
3. Click "New Policy" → "Enable read access for all users"
4. Save

For contact_messages:
- Add a policy: "Enable insert for all users" (so the form can submit)

---

## STEP 4 — Create Storage Buckets

Go to Supabase → Storage → New Bucket:

| Bucket Name    | Public? |
|----------------|---------|
| general        | ✅ Yes  |
| article-images | ✅ Yes  |
| teacher-photos | ✅ Yes  |
| gallery-photos | ✅ Yes  |

Upload your school logo and hero photo to the `general` bucket,
then copy the public URLs into your school_info row.

---

## STEP 5 — Update the Google Maps Embed

Open `contact.html` and find the `<iframe>` tag.
Replace the `src` with your actual Google Maps embed URL:
1. Go to maps.google.com
2. Search for your school location
3. Click Share → Embed a map
4. Copy the URL from the src="..." attribute

---

## STEP 6 — Deploy to Netlify

1. Go to netlify.com and sign in
2. Click "Add new site" → "Deploy manually"
3. Drag and drop this entire folder into the upload area
4. Done! Your site is live.

---

## Pages

| File           | URL path       |
|----------------|----------------|
| index.html     | /              |
| articles.html  | /articles      |
| article.html   | /article?id=XX |
| media.html     | /media         |
| teachers.html  | /teachers      |
| contact.html   | /contact       |

---

## Updating Content

All content is managed through Supabase Table Editor:
- **Articles** → articles table
- **Teachers** → teachers table
- **Gallery photos** → Upload to gallery-photos bucket, add row to gallery_photos table
- **Contact info** → Edit the single row in school_info table
- **Logo / Hero photo** → Upload to general bucket, update logo_url / hero_url in school_info
