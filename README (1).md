# AI Finance Controller — Reconciliation Agent

Built for the Razorpay AI Buildathon 2026 — Track 04: AI Finance Controller

![Report screenshot](report_screenshot.png)

## The problem

Every business runs two parallel records of money: what customers were charged (**orders**) and what actually arrived in the bank (**payments**). In the real world these rarely match perfectly — payments go missing, amounts get short-paid due to partial refunds, charges get duplicated, or settlements arrive late. Today, finding and resolving these mismatches is largely manual, done by a human scanning spreadsheets.

This project builds an AI agent that automates that first pass: it reconciles the two record sets, explains every discrepancy in plain English, and suggests a concrete next action — closing the loop instead of just producing a report.

## How it works

1. **Data generation** — synthetic `orders.csv` and `payments.csv` (80 orders, 78 payments) with realistic, deliberately injected mismatches: missing payments, amount mismatches, duplicate payments, and late payments.
2. **Matching engine** — merges the two datasets on order/payment ID, classifies every row as matched or as one of four exception types, and calculates a match rate.
3. **Risk ranking** — scores every exception by ₹ amount at risk and sorts worst-first, so the costliest problems surface first.
4. **AI explanation layer** — for the top exceptions, an LLM (Gemini) generates a plain-English explanation of what likely happened and a concrete suggested next action (e.g. drafting a follow-up email, flagging for manual review).
5. **Reporting** — all results are compiled into a clean HTML ledger-style report.

## Results

- **80** orders processed
- **75.0%** match rate
- **20** exceptions found across 4 categories (missing payment, amount mismatch, duplicate payment, late payment)
- **₹26,044** total amount at risk identified

## Tech stack

- Python, pandas — data generation and reconciliation logic
- Google Gemini API — AI-generated explanations and suggested actions
- HTML/CSS — final report rendering

## How to run

1. Open `reconciliation_agent.ipynb` in Google Colab
2. Run all cells top to bottom (Day 1 → Day 4)
3. You'll be prompted to paste a free Gemini API key (get one at [aistudio.google.com](https://aistudio.google.com)) when the AI explanation cell runs
4. The final report is saved as `reconciliation_report.html` — open it in any browser

## Honest limitations

- Matching is done on exact ID — real-world data would need fuzzy matching for typos or reformatted IDs
- Currently only the top 5 highest-impact exceptions get AI-generated explanations, to keep API usage minimal; this is easily scalable
- Data is synthetic; not yet tested against real-world data volume or edge cases (e.g. partial payments across multiple transactions)

## What makes this different

Most reconciliation tools stop at detection — flagging what doesn't match. This agent goes further: it prioritizes by financial impact, explains *why* in plain language, and proposes the next concrete action — actually closing the finance-ops loop rather than just reporting on it.
