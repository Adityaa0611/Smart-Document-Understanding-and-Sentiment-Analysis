SMART DOCUMENT UNDERSTANDING SENTIMENTAL ANALYSIS (v2 - Multi Product)
========================================================================

WHAT CHANGED FROM v1
----------------------
v1 analyzed one flat pile of review documents. v2 analyzes MULTIPLE
PRODUCTS separately and produces a percentage + "why" report per product.

FOLDER LAYOUT
--------------
Documents\
  Reviews\
    Product1\   -> Wireless Bluetooth Headphones reviews (.txt)
    Product2\   -> Kitchen Blender 750W reviews (.txt)
    Product3\   -> Running Shoes Size 9 reviews (.txt)
  Sentiment_Analysis_Report.xlsx  -> output (has "Details" and "Summary" sheets)

WHAT THE WORKFLOW DOES
------------------------
1. For each of the 3 products:
   a. Reads every review document in that product's folder.
   b. Scores each review's sentiment with keyword matching (same logic
      as v1), and ALSO collects which specific positive/negative words
      were found (not just the count).
   c. Writes a row per review into the "Details" sheet (Product, filename,
      snippet, keyword counts, score, label) - one continuous table
      across all 3 products.
   d. After all reviews for that product are processed, calculates:
        - % Positive, % Negative, % Neutral reviews
        - "Why Positive" - top distinct positive words found across
          that product's reviews (e.g. excellent, durable, comfortable)
        - "Why Negative" - top distinct negative words found (e.g.
          broken, refund, late)
      and writes ONE summary row per product into the "Summary" sheet.

2. Log messages print progress for each product as it's processed.

WHERE TO LOOK IN THE OUTPUT
------------------------------
- "Details" sheet: every single review, in order, grouped by product.
- "Summary" sheet: the headline numbers per product - this is the
  sheet to show your teacher first. It directly answers:
    "What % of reviews were positive/negative for this product, and why?"

REPLACING SAMPLE REVIEWS WITH REAL SCRAPED ONES
--------------------------------------------------
Right now the reviews are sample .txt files (fast, reliable, works
offline). To make this "live" using real Flipkart/Amazon review data:

1. Use UiPath Studio's UI Explorer to record a "Use Browser" +
   "Extract Structured Data" (or Data Scraping Wizard) sequence against
   the actual product review page, the same way your ORIGINAL project's
   "Use Browser Chrome" activity worked for Flipkart.
2. Instead of writing scraped reviews straight to Excel (like the
   original project did), write each scraped review's text to a new
   .txt file inside Documents\Reviews\Product1 (or 2 or 3) using the
   "Write Text File" activity, one file per review.
3. Everything downstream (sentiment scoring, percentages, "why" themes)
   works unchanged, because it just reads whatever .txt files exist in
   each product folder.
4. To filter to "last 2-3 months" reviews specifically, check the date
   text shown next to each review on the page (e.g. "Posted 2 months
   ago") while scraping, and only save reviews that match your date
   window - this filtering has to happen in the scraping step itself,
   since it depends on reading the live page.

This split (scrape -> save as .txt -> analyze) means you never have to
touch the analysis logic again, no matter which website or product you
scrape from.

HOW TO RUN
-----------
1. Open project.json in UiPath Studio.
2. Let dependencies restore (UiPath.Excel.Activities, UiPath.System.Activities).
3. Press F5 on Main.xaml.
4. Open Documents\Sentiment_Analysis_Report.xlsx -> check "Summary" sheet.

CUSTOMIZING
------------
- Add/remove products: edit the "Set Product Names" and "Set Product
  Folders" Assign activities near the top of Main.xaml (keep both lists
  the same length and in the same order), and create/remove matching
  folders under Documents\Reviews.
- Add more reviews per product: just drop more .txt files into that
  product's folder.
