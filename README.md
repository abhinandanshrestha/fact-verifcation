# Nepali Fact-Check Dataset: Combating Misinformation in Low-Resource Languages

## Overview

This project addresses a critical gap in misinformation research by creating a comprehensive, machine-readable dataset of fact-checked claims in Nepali, a low-resource language spoken by over 16 million people in Nepal and neighboring regions. The dataset aggregates fact-checks from multiple Nepali fact-checking organizations, providing researchers, journalists, and AI practitioners with structured data to combat the spread of misinformation in underserved linguistic communities.

## Background & Research Context

### The Misinformation Challenge in Low-Resource Languages

Misinformation poses a significant threat to democratic societies, public health, and social cohesion worldwide. While substantial research and technological solutions exist for high-resource languages like English, Spanish, and Mandarin, **low-resource languages like Nepali face a critical data scarcity problem**:

1. **Limited Training Data**: Machine learning models for fact-checking, claim detection, and misinformation classification require large labeled datasets. Such datasets are virtually non-existent for Nepali.

2. **Cross-lingual Transfer Limitations**: While multilingual models (mBERT, XLM-R) show promise, their performance on low-resource languages remains suboptimal without language-specific fine-tuning data.

3. **Cultural and Contextual Nuances**: Misinformation in Nepal often involves local political figures, cultural contexts, and regional issues that global models fail to capture.

4. **Digital Divide**: Nepal's growing internet penetration (especially via mobile devices) has accelerated information spread, but fact-checking infrastructure lags behind.

### Why This Dataset Matters

This dataset enables several critical research directions:

- **Automated Fact-Checking**: Training models to detect and verify claims in Nepali text
- **Claim Detection**: Identifying check-worthy claims in social media and news articles
- **Misinformation Pattern Analysis**: Understanding how false information spreads in Nepali-speaking communities
- **Cross-lingual Research**: Comparing misinformation patterns across linguistic and cultural boundaries
- **Multimodal Analysis**: Studying the role of AI-generated images and manipulated media in Nepali misinformation
- **Temporal Analysis**: Tracking how misinformation evolves during elections, health crises, and social movements

## Dataset Description

### Data Sources

The dataset aggregates fact-checks from **5 active Nepali fact-checking organizations**:

| Source | Type | Articles | Status | Notes |
|--------|------|----------|--------|-------|
| **Nepal Factcheck** | WordPress API | 659 | Active | Most comprehensive; includes claim extraction and evidence |
| **Karnali Factcheck** | WordPress API | 51 | Active | Regional focus on Karnali Province |
| **Lumbini Factcheck** | WordPress API | 47 | Active | Regional focus on Lumbini Province |
| **Sachilo** | Sitemap + JSON-LD | 131 | Active | Uses schema.org ClaimReview standard |
| **TechPana Fact Check** | Static JS Bundle | 115 | Active | Limited fields (no article text/URLs) |
| The Ruju | N/A | 0 | Blocked | HTTP 403 (access restricted) |
| NepalCheck | N/A | 0 | Blocked | robots.txt disallows scraping |
| South Asia Check | N/A | 0 | Defunct | Domain now serves unrelated content |

**Total Records**: 1,003 fact-checked articles (as of November 2026)

### Data Schema

Each record contains up to 21 fields:

| Field | Description | Coverage |
|-------|-------------|----------|
| `source` | Fact-checking organization name | 100% |
| `article_url` | URL of the fact-check article | ~88% |
| `article_id` | Unique identifier within source | 100% |
| `title` | Article headline | 100% |
| `claim` | The claim being fact-checked | ~70% |
| `verdict` | Fact-check verdict (e.g., "मिथ्या सूचना" = False) | ~95% |
| `claimant` | Person/entity who made the claim | ~15% |
| `claim_date` | When the claim was made | ~10% |
| `fact_check_date` | When fact-check was published | ~40% |
| `published_date` | Article publication timestamp | ~90% |
| `author` | Fact-checker name(s) | ~75% |
| `category` | Article category/topic | ~60% |
| `language` | Detected language (ne/en/mixed) | 100% |
| `description` | Article excerpt/summary | ~70% |
| `article_text` | Full article text (Nepali) | ~88% |
| `evidence` | Evidence and reasoning | ~70% |
| `sources` | External source URLs cited | ~65% |
| `correction` | Correction information | ~5% |
| `image_url` | Featured image URL | ~85% |
| `tags` | Article tags | ~40% |
| `scraped_at` | Data collection timestamp | 100% |

### Verdict Categories

The dataset includes standardized verdict labels in Nepali:

- **मिथ्या सूचना** (False Information)
- **भ्रामक सूचना** (Misleading Information)
- **अपुष्ट सूचना** (Unverified Information)
- **सही सूचना** (True Information)
- **एआई जेनेरेटेड** (AI Generated)
- **आंशिक सत्य** (Partially True)
- **संदिग्ध** (Doubtful)

## Research Applications

### 1. Automated Fact-Checking Systems

Train transformer-based models (mBERT, IndicBERT, XLM-R) for:
- Binary classification (True/False)
- Multi-class verdict prediction
- Evidence retrieval and ranking
- Claim-evidence entailment

**Example Research Questions**:
- Can we fine-tune multilingual models to achieve >80% accuracy on Nepali fact-checking?
- How much training data is needed for effective transfer learning?

### 2. Claim Detection & Check-Worthiness

Identify which statements in Nepali text warrant fact-checking:
- Social media posts (Twitter/X, Facebook)
- News articles
- Political speeches
- WhatsApp forwards

**Example Research Questions**:
- What linguistic features indicate check-worthy claims in Nepali?
- Can we detect claims without explicit fact-checking labels?

### 3. Misinformation Propagation Studies

Analyze patterns in false information spread:
- Temporal trends (election periods, COVID-19, natural disasters)
- Topic distribution (politics, health, religion, technology)
- Source credibility and citation patterns
- AI-generated content prevalence

**Example Research Questions**:
- How does misinformation in Nepal differ from global patterns?
- What role do AI-generated images play in Nepali misinformation?

### 4. Multimodal Misinformation Detection

Combine text and image analysis:
- Detect manipulated images in fact-checks
- Image-text consistency verification
- AI-generated image detection (using the "एआई जेनेरेटेड" label)

### 5. Cross-lingual Transfer Learning

Use this dataset to improve low-resource NLP:
- Zero-shot and few-shot learning experiments
- Multilingual model evaluation
- Language-agnostic feature extraction

### 6. Explainable AI for Fact-Checking

Build interpretable models:
- Evidence highlighting
- Reasoning chain generation
- Verdict explanation in Nepali

## Installation & Usage

### Prerequisites

```bash
# Python 3.8+
pip install pandas requests beautifulsoup4 lxml jupyter
```

### Quick Start

#### Option 1: Use the Pre-collected Dataset

```python
import pandas as pd

# Load the dataset
df = pd.read_csv('nepali_fact_check_dataset.csv')

# Basic statistics
print(f"Total records: {len(df)}")
print(f"Sources: {df['source'].nunique()}")
print(f"Date range: {df['published_date'].min()} to {df['published_date'].max()}")

# Verdict distribution
print(df['verdict'].value_counts())

# Language distribution
print(df['language'].value_counts())
```

#### Option 2: Run the Scraper Locally

Use the included `Nepali_Fact_Check_Local.ipynb` notebook:

```bash
jupyter notebook Nepali_Fact_Check_Local.ipynb
```

The notebook will:
1. Scrape all active fact-checking websites
2. Parse and structure the data
3. Export to CSV in the same directory
4. Generate a failures log for debugging

**Estimated runtime**: 15-20 minutes (respects rate limits)

### Configuration

Edit the configuration section in the notebook:

```python
REQUEST_DELAY = 1.5           # Delay between requests (seconds)
REQUEST_TIMEOUT = 30          # Request timeout (seconds)
MAX_RETRIES = 4               # Retry attempts for failed requests
MAX_ARTICLES_PER_SITE = 3000  # Safety limit per source
```

## Data Quality & Limitations

### Strengths

- **Comprehensive Coverage**: Aggregates all major Nepali fact-checking sources
- **Structured Format**: Machine-readable CSV with consistent schema
- **Rich Metadata**: Includes claims, evidence, sources, and verdicts
- **Temporal Span**: Covers multiple years of fact-checking activity
- **Reproducible**: Fully documented scraping methodology

### Limitations

1. **Incomplete Fields**: Not all sources provide all fields (e.g., TechPana lacks article URLs and full text)
2. **Language Mixing**: Some articles mix Nepali and English, complicating NLP tasks
3. **Verdict Standardization**: Different sources use slightly different verdict taxonomies
4. **Claim Extraction**: Claim boundaries are sometimes ambiguous in article text
5. **Temporal Bias**: More recent articles are over-represented
6. **Source Availability**: Two sources (The Ruju, NepalCheck) are currently inaccessible

### Data Quality Assurance

The scraper includes multiple validation steps:
- robots.txt compliance
- HTML validation against API metadata
- Duplicate URL detection
- Missing field logging
- Failure tracking (see `nepali_fact_check_failures.csv`)

## Ethical Considerations

### Responsible Use

This dataset should be used to:
- **Combat misinformation**, not spread it
- **Empower fact-checkers** with better tools
- **Advance research** in low-resource NLP
- **Promote media literacy** in Nepal

### Do Not Use This Dataset To:

- Train models that generate misinformation
- Dox or harass individuals mentioned in claims
- Violate the terms of service of source websites
- Commercially exploit fact-checkers' work without permission

### Privacy & Attribution

- All data is publicly available from fact-checking organizations
- Always cite the original fact-checking source when using specific claims
- Respect the intellectual property of fact-checkers
- Do not scrape more frequently than the configured rate limits

## Citation

If you use this dataset in your research, please cite:

```bibtex
@dataset{nepali_factcheck_2026,
  title={Nepali Fact-Check Dataset: A Comprehensive Collection for Misinformation Research in Low-Resource Languages},
  author={[Your Name/Organization]},
  year={2026},
  month={November},
  url={https://github.com/[your-repo]},
  note={Aggregated from Nepal Factcheck, Karnali Factcheck, Lumbini Factcheck, Sachilo, and TechPana Fact Check}
}
```

### Acknowledge Source Organizations

Please also acknowledge the fact-checking organizations:
- [Nepal Factcheck](https://nepalfactcheck.org)
- [Karnali Factcheck](https://karnalifactcheck.org)
- [Lumbini Factcheck](https://lumbinifactcheck.org)
- [Sachilo](https://sachilo.com)
- [TechPana Fact Check](https://tpfactcheck.com)

## Contributing

Contributions are welcome! Areas for improvement:

- **Additional Sources**: Identify new Nepali fact-checking organizations
- **Field Extraction**: Improve claim/evidence parsing algorithms
- **Verdict Mapping**: Standardize verdict labels across sources
- **Annotation**: Add human annotations for model training
- **Multimodal Data**: Enhance image analysis capabilities
- **Temporal Updates**: Schedule regular dataset updates

## License

This dataset is provided for **research and educational purposes only**. The content belongs to the respective fact-checking organizations. Please review each source's terms of service before commercial use.

**Scraper Code**: MIT License

## Contact & Support

For questions, issues, or collaboration opportunities:
- Open an issue on GitHub
- Email: [your-email]
- Twitter: [your-handle]

## Acknowledgments

This project was inspired by the critical work of Nepali fact-checkers who combat misinformation daily, often with limited resources. Their dedication to truth and accountability makes this research possible.

Special thanks to:
- The teams at Nepal Factcheck, Karnali Factcheck, Lumbini Factcheck, Sachilo, and TechPana
- The global fact-checking community (IFCN, Poynter Institute)
- Researchers advancing low-resource NLP

---

**Last Updated**: November 22, 2026  
**Dataset Version**: 1.0  
**Total Records**: 1,003 fact-checks  
**Languages**: Nepali (primary), English (secondary)
