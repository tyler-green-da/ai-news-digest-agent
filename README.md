# AI News Digest Agent

> A sophisticated, multi-stage n8n automation that aggregates, analyzes, curates, and delivers a professional AI news newsletter.

![Alt text for your GIF](URL_you_just_copied)


## 🚀 Project Overview

In the fast-paced world of artificial intelligence, staying informed is critical for professionals in finance, tech, and business strategy. However, the sheer volume of daily news makes it impossible to keep up, leading to information overload where key signals are lost in the noise.

This project solves that problem. The AI News Digest is an automated agent that acts as a personal intelligence analyst. It systematically gathers news from top-tier sources, uses multiple AI models to analyze and understand the content, curates only the most strategically important stories, and delivers a polished, easy-to-read newsletter to your inbox every morning.

It's designed for a professional audience, like traders and hedge fund managers, who need timely, curated, and actionable intelligence.

## ✨ Key Features

* **Automated News Aggregation:** Scans multiple RSS feeds from sources like NVIDIA, Google AI, and MIT Technology Review.
* **AI-Powered Analysis & Categorization:** A first-pass AI reads every article to generate a multi-part summary (`Rundown`, `Details`, `Why it matters`) and assigns it to a relevant category (e.g., "Hardware & Infrastructure").
* **AI-Powered Importance Curation:** A second AI acts as a critical editor, reading all the initial summaries and assigning each an "importance score" from 1-10 based on its potential market impact.
* **Dynamic Filtering:** The workflow automatically filters out low-scoring articles, ensuring only the most noteworthy stories (e.g., score 7+) make it into the final digest.
* **AI-Generated Executive Summary:** A third AI pass performs a meta-analysis on the *curated* stories to write a conversational, high-level introduction, just like a human editor.
* **Professional HTML Template:** The final output is a professionally designed HTML email with a custom banner, category icons, and a clean, scannable layout.
* **Interactive Navigation:** The newsletter includes a dynamically generated, clickable Table of Contents that allows users to jump directly to the categories they care about most.

## 🛠️ Tech Stack

* **Automation:** n8n (Cloud Version)
* **AI Models:** OpenAI API (gpt-4o-mini)
* **Data Sources:** RSS Feeds
* **Core Logic:** JavaScript (within the n8n Code Node)

## 🌊 Workflow Breakdown

The n8n workflow executes in a sophisticated, multi-stage pipeline:

1.  **Trigger:** A `Schedule` node kicks off the workflow every morning.
2.  **Aggregation:** Multiple `RSS Read` nodes fetch the latest articles in parallel.
3.  **Deduplication:** The articles are merged and duplicates are removed.
4.  **Initial Analysis (`summaries` node):** Every article is sent to the OpenAI API with a prompt that instructs it to return a structured JSON object containing a detailed summary, a category, and a unique `link` identifier.
5.  **Curation (`Code` node logic):** The final `Code` node takes all the analyzed articles and performs the main business logic:
    * It filters the articles, keeping only those with an `importance_score` of 7 or higher.
    * It prepares a text block of the curated summaries and sends it to another AI model to generate the conversational executive summary.
    * It builds the final HTML, inserting the executive summary, a clickable Table of Contents, and the formatted "cards" for each curated article.
6.  **Delivery:** The generated HTML is passed to a `Send Email` node and delivered to the user.

## 🧠 Challenges & Key Learnings

This project involved significant real-world problem-solving:

* **AI Unpredictability:** A core challenge was handling the AI's tendency to not always return perfectly structured JSON. The final code was engineered to be highly resilient, able to parse data even if the AI's output format varied slightly.
* **Advanced Prompt Engineering:** Moving from simple summarization to a curated digest required iteratively refining the AI prompts. I had to "train" the AI to act as a critical editor by giving it specific instructions on how to distribute its importance scores, transforming it from a simple tool into a system with judgment.
* **Data Integrity:** Early versions of the workflow failed because they used the article `title` as a unique ID. I discovered the AI would subtly rephrase titles, breaking the matching logic. The solution was to re-architect the workflow to use the article's URL (`link`) as a stable, immutable identifier, a key principle in data engineering.

## 🔮 Future Improvements

This project serves as a robust platform for further enhancements:

* **Tier 2:** Integrate AI-powered **Sentiment Analysis** for each story and identify/link **Stock Tickers** of mentioned companies.
* **Tier 3:** Add user-defined **Keyword Alerts** to highlight stories relevant to a specific portfolio or area of interest.
