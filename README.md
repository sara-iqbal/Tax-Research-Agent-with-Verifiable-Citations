# Tax Research Agent with Verifiable Citations

**A question-answering tool for UK VAT guidance that shows its sources, proves its quotes are real, and says "I don't know" when it doesn't know.**

> Not tax advice. Guidance changes; always check GOV.UK.

## The story

Priya runs a small building firm. Her accountant is busy, so she asks a chatbot: *"Can I claim back the VAT on dinner with a client?"*

The chatbot replies confidently: *"Yes, business meals are deductible."* It sounds perfect. It is also wrong, and the rule it should have used says input tax on this kind of business entertainment is generally blocked. Priya claims it, HMRC finds it later, and she pays back the money plus a penalty.

**The problem is not that AI sometimes makes mistakes. It's that a wrong answer sounds exactly like a right one, and nobody can check it.**

This project is a chatbot built to be checkable:

1. It only answers from real GOV.UK guidance.
2. Every answer comes with the exact quote and a link to the page.
3. A separate check confirms the quote really appears, word for word, in that section. If it doesn't, the answer is thrown away.
4. If the documents don't cover the question, it says so instead of guessing.

Now Priya can click through, read the rule herself, and decide.

## The data

Five public GOV.UK pages, downloaded when the notebook runs (nothing is copied into this repo):

| Document | Covers |
|---|---|
| VAT guide (Notice 700) | General VAT rules and procedures |
| Business entertainment (Notice 700/65) | When VAT on entertainment can be claimed |
| Reverse charge: buying construction services | When the buyer accounts for VAT |
| Reverse charge: supplying construction services | The supplier's side of the same rule |
| HMRC manual VATREVCON22000 | Which services the construction reverse charge covers |

Each page is cut into small sections. Each section keeps its heading and a link, so every quote points to the exact place it came from. Check GOV.UK's licence terms before republishing any text.

## How it's tested

18 test questions: 13 that the documents should answer, and 5 they should not (for example about corporation tax or R&D relief, which are not in the five documents). The agent is scored on:

- **Retrieval hit rate:** did it find the right document?
- **Answer rate:** did it answer the answerable questions?
- **Abstention accuracy:** did it refuse the unanswerable ones?
- **Citation validity:** were all quotes genuinely in the cited section?

The "is this covered?" threshold was set in advance and not tuned on the answers.

**Results:** run the notebook, then copy the numbers into this section. The dashboard reads `docs/results.json` and shows every question with its answer and quote.

## Run it

1. Open `notebooks/Tax_Research_Agent.ipynb` in Google Colab and choose *Runtime > Run all*.
2. Download `results.json` and put it in `docs/`.
3. Optional: add a Colab secret `ANTHROPIC_API_KEY` so a language model writes fuller answers. Without it, the tool quotes the source directly. Model answers are still rejected if their quotes fail verification.

Publish the dashboard: repo *Settings > Pages > Deploy from a branch > main, folder /docs*.

## Limits

- Five documents and 18 questions: a demonstration, not proof.
- Keyword search only, so questions worded very differently from the guidance may be missed.
- A real quote can still be an incomplete answer. Read the sources.
- The language-model path has not been tested here with a live key; the fallback is tested.
- Covers VAT only.

## Next steps

Add meaning-based search, more documents (R&D and capital allowances manuals), and a larger independently written test set.
